---
layout: post
title: "Nagios版本升级"
date: 2012-08-02 20:57
comments: true
categories: [Linux,监控] 
---
总体步骤：
1.下载最新安装包：nagios-3.4.1.tar.gz

2.解压编译：
    tar zxf nagios-3.4.1.tar.gz
    cd nagios
    ./configure --with-command-group=nagcmd
    make all
    make install

3.测试验证：
    /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg

4.重启服务：
    service nagios restart
