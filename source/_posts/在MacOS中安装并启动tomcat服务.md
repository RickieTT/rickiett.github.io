---
title: 在MacOS中安装并启动tomcat服务
date: 2022-12-24 22:21:23
---

一、下载Tomcat

　　在http://tomcat.apache.org/官网选择要安装的版本号，在此以Tomcat8为例。在Binary Distributions中 选择带有tar.gz 为后缀名的压缩文件下载。

二、解压文件

　　下载完之后，解压文件，并且将文件放在 /library 中(在 前往文件夹 中 输入 /library) 。

三、启动Tomcat

　　打开终端，并且输入代码

　　cd /library/apache-tomcat-8.5.75/bin

　　sudo sh startup.sh

　　在此输入密码(本机密码)

四、验证是否安装成功

　　启动tomcat后，打开浏览器，输入http://localhost:8080, 获得如下界面即成功。



五、关闭Tomcat

　　打开终端，并且输入代码

　　sudo sh shutdown.sh

 
