---
title: mysql-8.0.23的安装
date: 2021-01-27 14:14:16
categories:
- 教程
tags:
- MySQL
typora-root-url: ..
---

记录一下MySQL 8.0.23的安装配置过程，这里以windows为例



### 安装

1. 去官网下载MySQL 8.0.23版本

   ![screenshot1](/images/blog/MySQL/screenshot1.jpg)

   

   ![screenshot2](/images/blog/MySQL/screenshot2.jpg)

   

   ![screenshot3](/images/blog/MySQL/screenshot3.jpg)

   

   ![screenshot4](/images/blog/MySQL/screenshot4.jpg)

   

   ![screenshot5](/images/blog/MySQL/screenshot5.jpg)

   

2. 下载完成后解压到自己的安装目录

   我这里的解压目录是： D:\software\mysql-8.0.23-winx64

3. 将解压路径配置到环境变量中

   右键此电脑属性  -> 找到高级系统设置配置 -> 环境变量 -> 双击Path，填写你自己的解压路径

   ![screenshot10](/images/blog/MySQL/screenshot10.jpg)

   ![](/images/blog/MySQL/screenshot11.jpg)

   ![screenshot12](/images/blog/MySQL/screenshot12.jpg)

4. 在解压目录里面新建一个my.ini的配置文件，内容如下：

   ```ini
   [mysqld]
   # set basedir to your installation path
   basedir=D:/software/mysql-8.0.23-winx64
   # set datadir to the location of your data directory
   datadir=D:/software/mysql-8.0.23-winx64/data
   ```

5. 然后再新建一个data目录



### 启动mysql服务器

这里有2种，一种是安全的，一种是不安全的，实际中你任选一种就行。

##### 安全的

通过管理员权限进入cmd，接着键入 `mysqld --initialize --console` ，然后就会看到如下产生的随机密码，这个密码就是进入MySQL时root用户的初始密码，记下它，如果你不小心关掉了cmd或者没记住，删掉刚才创建的data目录，再执行一遍初始化就会重新生成。

![screenshot6](/images/blog/MySQL/screenshot6.jpg)

##### 不安全的

通过管理员权限进入cmd，接着键入 `mysqld --initialize-insecure`，这种后面不需要输入密码就可以直接进入数据库



然后键入`mysqld --install`安装mysql服务，接着键入`net start mysql`启动服务，就会看到服务成功启动的提示

![screenshot7](/images/blog/MySQL/screenshot7.jpg)



### 进入MySQL数据库

 键入`mysql -u root -p` ，会提示你输入密码，输入之前记下的密码，然后就会看到如下界面，那么恭喜你，表示你已经成功进入MySQL了

![screenshot8](/images/blog/MySQL/screenshot8.jpg)



### 修改密码

 键入 `ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';` 出现如下界面，表示更改成功。	

![screenshot9](/images/blog/MySQL/screenshot9.jpg)

