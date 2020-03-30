# 部署Jenkins

## Jenkins介绍

- 自动化各种任务，包括 构建、测试和部署 软件。


- 过程：编码	构建	集成	测试	交付	部署


​    1.持续集成：CI,将个人研发部分向软件整体部分交付。频繁的将代码集成到主干。

开发人员每一次代码提交，都能自动在jenkins上自动build构建。

​	2.持续交付：频繁的将软件的新版本交付给质量团队或用户，以供评审

​	3.持续部署：代码通过评审以后，自动部署到生产环境

功能：持续的软件版本发布/测试项目；监控外部调用执行的工作

开发或测试人员操作：项目所需参数传给jenkins下对应你的项目的job，jenkins自动完成部署。

运维：监控jenkins的相关指标，保证健康状态下运转，以及项目部署配置中出现的权限、参数配置、工具集成调用等问题

流水线

插件，支持实现和集成continuous delivery pipelines到Jenkins，用于从版本控制向用户和客户获取软件。

软件的每次变更在被释放的路上都经历了一个复杂的过程 

Blue Ocean

持续交付Pipeline的复杂可视化，



## 环境准备

本地PC正常访问外网，CentOS7桥接本机pc网卡上

##### 1.添加yum仓库源

若无wget命令，则安装：yum install wget -y

![1582618170005](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582618170005.png)

##### 2.安装java版本8.0或以上

![1582618303696](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582618303696.png)

##### 3.关闭系统防火墙

![1582618333177](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582618333177.png)

##### 4.关闭selinux并重启系统

![1582618383075](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582618383075.png)

## Jenkins安装与初始化配置

![1582620949947](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582620949947.png)

![1582621962029](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582621962029.png)

更改jenkins默认的家目录以及log日志目录的数主和数组权限： 

chown -R deploy:deploy /var/lib/jenkins

chown -R deploy:deploy /var/log/jenkins

![1582622337468](C:\Users\mimo\AppData\Roaming\Typora\typora-user-images\1582622337468.png)

确认jenkins服务是否正常启动：

lsof -i:8080

运行输入：

c:\windows\system32\drivers\etc\hosts 用记事本打开，

## Jenkins Job构建

代表一个任务或项目

可配置与可执行

执行后的记录称之为Bulid

日志监控与记录

所有文件集中保存

Freestyle Job：

需在页面添加模块配置项与参数完成配置

每个job只能实现一个开发功能

无法将配置代码化

Pipeline Job： 原先是jenkins的插件，现内嵌入

能实现持续集成和持续交付

可定义多个stage构建一个管道工作集

所有配置代码化，方便job配置迁移与版本控制

需要pipeline脚本语法基础