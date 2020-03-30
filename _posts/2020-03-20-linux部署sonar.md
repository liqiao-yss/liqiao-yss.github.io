---
layout: post
title: linux环境部署sonar
date: 2020-03-20
Author: mimo
categories: 
tags: [部署]
comments: true
---

# Linux部署sonar

### 安装前准备环境

Centos7(Linux2.6/3.x/4/x  64-bit):

- vm.max_map_count 大于或等于262144

- fs.file-max 大于或等于65536
- 运行SonarQube的用户可以打开至少65536个文件描述符
- 运行SonarQube的用户可以打开至少2048个线程

用root用户新增两个配置文件即可满足上述要求：

　　vim /etc/sysctl.d/99-sonarqube.conf（加入下面两行内容）

​      　　　vm.max_map_count=262144

​     　　　 fs.file-max=65536

​      vim /etc/security/limits.d/99-sonarqube.conf（加入下面两行内容）

​      　　　sonarqube - nofile 65536

​    　　　  sonarqube - nproc 4096

------

### 安装jdk

下载链接：https://www.oracle.com/java/technologies/javase-jdk8-downloads.html

选择jdk-8u211-linux-x64.rpm下载

```
# 查看安装的路径
rpm -qpl jdk-8u211-linux-x64.rpm
## 安装
rpm -i jdk-8u211-linux-x64.rpm
```

### 安装MySQL

MySQL 5.7

[下载链接](https://dev.mysql.com/downloads/repo/yum/) [参考文档](https://dev.mysql.com/doc/refman/5.7/en/linux-installation-yum-repo.html)

下载mysql80-community-release-el7-3.noarch.rpm

```
# 安装rpm源
sudo rpm -Uvh mysql80-community-release-el7-3.noarch.rpm

# 编辑，找到Enable to use MySQL 5.7，改为enabled=1，其他版本设置成enabled=0
vim /etc/yum.repos.d/mysql-community.repo

# 检查只有MySQL 5.7启动
yum repolist enabled | grep mysql

# 安装MySQL
sudo yum install mysql-community-server

# 启动MySQL服务器
sudo service mysqld start

# MySQL服务器的状态
sudo service mysqld status

# 查看超级用户的密码
sudo grep 'temporary password' /var/log/mysqld.log

# 登录mysql
mysql -uroot -p

# 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';

# 修改密码校验
set global validate_password_policy=0;
set global validate_password_length=1;

# 默认mysql的root用户不支持远程访问，开启访问权限
GRANT ALL ON *.* TO root@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
update user set host='%' where user='root';
flush privileges;

# 创建数据库sonarqube
CREATE DATABASE `sonarqube` CHARACTER SET 'utf8';

新增用户sonarqube并授予sonarqube数据库全部权限
rant all privileges on sonarqube.* to sonarqube@'%' identified by "password";

开启3306端口
irewall-cmd --add-port=3306/tcp

(a)数据库目录
var/lib/mysql/
(b)配置文件
usr/share /mysql（mysql.server命令及配置文件）
etc/my.cnf
(c)相关命令
/usr/bin（mysqladmin mysqldump等命令）
(d)启动脚本
/etc/rc.d/init.d/（启动脚本文件mysql的目录）
```

------

### 或连接windows版mysql

在配置文件中配置好url、password等信息

------

### 安装SonarQube

1.下载 SonarQube

当前版本： 7.7 [下载链接](https://www.sonarqube.org/downloads/)

2.解压

得到当前路径: `/opt/sonarqube-7.7`

3.修改配置文件

vi  /opt/sonarqube-7.7/conf/sonar.properties

```
# 对应的数据库信息
sonar.jdbc.username=
sonar.jdbc.password=
sonar.jdbc.url=
```

4.新增用户

> SonarQube不能以root启动

```
# 添加用户
adduser sonarqube
# 设置密码
passwd sonarqube
# 授权
chown -R sonarqube:sonarqube /opt/sonarqube-7.7
```

5.默认SonarQube启动在9000端口

```
firewall-cmd --add-port=9000/tcp
systemctl stop firewalld
systemctl disable firewalld
```

6.以服务启动SonarQube

- 创建文件

```
vim /etc/systemd/system/sonarqube.service
```

- 添加下面内容：

```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=simple
User=sonarqube
Group=sonarqube
PermissionsStartOnly=true
ExecStart=/bin/nohup /bin/java -Xms32m -Xmx32m -Djava.net.preferIPv4Stack=true -jar /opt/sonarqube-7.7/lib/sonar-application-7.7.jar
StandardOutput=syslog
LimitNOFILE=65536
LimitNPROC=8192
TimeoutStartSec=5
Restart=always

[Install]
WantedBy=multi-user.target
```

- 安装启动

```
su sonarqube
# 注册服务
sudo systemctl enable sonarqube.service
# 启动服务
sudo systemctl start sonarqube.service
# 重启
sudo systemctl restart sonarqube.service
#查看日志
sudo systemctl status sonarqube.service
```

- 查看日志

  ```
  vim /opt/sonarqube-7.7/logs/web.log
  ```

------

### 常见问题

1.使用sudo命令时，出现问题：

![1583482617566](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1583482617566.png)

解决：

vi  /etc/sudoers

```
root    ALL=(ALL:ALL) ALL
sonarqube    ALL=(ALL:ALL) ALL
```

