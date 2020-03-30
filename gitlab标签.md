# Gitlab

## 工作流程：

创建并克隆项目

创建项目某feature分支

编写代码并提交至该分支

推送该项目分支至远程Gitlab服务器

进行代码检查并提交Master主分支合并申请

项目领导审查代码并确认合并申请

## linux环境安装配置

#### 防火墙配置

关闭防火墙

systemctl stop firewalld

禁止开机启动

systemctl disable firewalld

关闭强制访问控制安全策略

<img src="C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582618383075.png" alt="1582618383075"  />

查看策略是否被禁用

getenforce

#### 安装gitlab-ce包

![1582794869018](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582794869018.png)

![1582794910889](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582794910889.png)

![1582794948558](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582794948558.png)

或yum -y install gitlab-ce

![1582794975435](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582794975435.png)

创建目录

mkdir -p /etc/gitlab/ssl，进入ssl

创建私有秘钥

openssl genrsa -out "/etc/gitlab/ssl/gitlab.example.com.key" 2048

创建csr证书

openssl -new -key "/etc/gitlab/ssl/gitlab.example.com.key" -out "/etc/gitlab/ssl/gitlab.example.com.csr"

创建签署证书

openssl x509 -req -days 365 -in "/etc/gitlab/ssl/gitlab.example.com.csr" -signkey "/etc/gitlab/ssl/gitlab.example.com.key" -out "/etc/gitlab/ssl/gitlab.example.com.crt"

输出pem证书

openssl dhparam -out /etc/gitlab/ssl/dhparams.pem 2048

更改权限

chmod 600 *

vi /etc/gitlab/gitlab.rb

将external_url   的 http改成https，并

![1583133872239](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1583133872239.png)

![1583133922689](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1583133922689.png)

初始化gitlab相关的所有配置

gitlab-ctl reconfigure

更改Nginx下gitlab的配置文件

vi /var/opt/gitlab/nginx/conf/gitlab-http.conf

找到server_name下一行加

重定向gitlab https请求

rewrite ^(.*)$ https://$host$1 permanent;

nginx重启生效

gitlab-ctl restart

运行找到c:\windows\system32\drivers\etc\hosts

加一行

ip     gitlab.example.com

直接在浏览器找域名

![1579246169691](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1579246169691.png)





![](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1579247951076.png)

![](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1579252179390.png)

![](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1579252348473.png)