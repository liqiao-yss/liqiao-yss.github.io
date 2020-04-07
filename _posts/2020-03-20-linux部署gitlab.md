---
layout: post
title: linux环境部署Gitlab
date: 2020-03-20
Author: mimo
categories: 
tags: [部署,gitlab]
comments: true
typora-root-url: ..\
---

#### 防火墙配置

1.关闭防火墙

```
systemctl stop firewalld
```

2.禁止开机启动

```
systemctl disable firewalld
```

3.关闭强制访问控制安全策略

4.关闭selinux并重启系统

```
vi /etc/sysconfig/selinux
...
selinux=disabled
...
#reboot
```

5.查看策略是否被禁用

```
getenforce
```

------

#### 安装gitlab-ce包

安装Omnibus Gitlab-ce package

1.安装Gitlab组件

```
# yum -y install curl policycoreutils openssh-server openssh-clients postfix
```

2.配置yum仓库

```
# curl - sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
```

3.启动postfix邮件服务

```
# systemctl start postfix && systemctl enable postfix
```

4.安装Gitlab-ce社区版本

```
# yum install -y gitlab-ce
或
yum -y install gitlab-ce
```

------

#### Omnibus Gitlab等相关配置初始化并完成安装

1.证书创建与配置加载

2.Nginx SSL代理服务配置

3.初始化Gitlab相关服务并完成安装

创建目录

```
mkdir -p /etc/gitlab/ssl
```

进入ssl

```
cd /etc/gitlab/ssl
```

创建私有秘钥

```
openssl genrsa -out "/etc/gitlab/ssl/gitlab.example.com.key" 2048
```

创建csr证书

```
openssl -new -key "/etc/gitlab/ssl/gitlab.example.com.key" -out "/etc/gitlab/ssl/gitlab.example.com.csr"
```

创建签署证书

```
openssl x509 -req -days 365 -in "/etc/gitlab/ssl/gitlab.example.com.csr" -signkey "/etc/gitlab/ssl/gitlab.example.com.key" -out "/etc/gitlab/ssl/gitlab.example.com.crt"
```

输出pem证书

```
openssl dhparam -out /etc/gitlab/ssl/dhparams.pem 2048
```

更改权限

```
chmod 600 *
```

修改配置文件

```
vi /etc/gitlab/gitlab.rb

将 external_url 的 http改成https
```

![1583133872239](/document_image/1583133872239.png)

![1583133922689](/document_image/1583133922689.png)

初始化gitlab相关的所有配置

```
gitlab-ctl reconfigure
```

更改Nginx下gitlab的配置文件

```
vi /var/opt/gitlab/nginx/conf/gitlab-http.conf
```

找到server_name下一行加 重定向gitlab https请求

```
rewrite ^(.*)$ https://$host$1 permanent;
```

nginx重启生效

```
gitlab-ctl restart
```

访问

```
运行找到c:\windows\system32\drivers\etc\hosts

加一行

ip     gitlab.example.com

直接在浏览器找域名
```