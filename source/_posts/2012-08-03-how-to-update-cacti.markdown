---
layout: post
title: "升级Cacti版本"
date: 2012-08-03 13:57
comments: true
categories: [Linux,监控]
---
总体步骤如下：

1.下载最新安装包，并且解压；
	tar zxf cacti-0.8.8a.tar.gz
2.include/config.php 修改数据库信息；
	vim cacti-0.8.8a/include/config.php
3.备份数据库；
	mysqldump -ucacti -p cacti087e > cacti.sql
4.备份Cacti 工作目录，采用重命名方式备份；
	mv /other/web/cacti /other/web/cacti-0.8.7e
<!-- more -->

5.重命名新安装包为Cacti；
	mv /other/web/cacti-0.8.8a /other/web/cacti
6.将原来的rrd文件，RRD目录复制到新版本的RRD目录；
	cp -r /other/web/cacti-0.8.7e/rra /other/web/cacti/
7.将原来的所有脚本文件，scripts目录复制到新版本的目录scripts中；
	cp -r /other/web/cacti-0.8.7e/scripts/ /other/web/cacti/scripts/
8.将原来的XML文件，resource目录复制到新版本中的目录resource中；
	cp -r /other/web/cacti-0.8.7e/resource/  /other/web/cacti/resource/
9.修改新版本RRD、LOG目录所有者；
	chown -R cactiuser /other/web/cacti
10.打开http://ip/cacti/链接进行安装升级数据库；
	http://192.168.1.120/cacti/
11.测试；

