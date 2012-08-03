---
layout: post
title: "crontab导致/var分区空间爆满的问题"
date: 2011-12-27 14:36
comments: true
categories: [Linux,系统管理] 
---

最近清理磁盘空间时发现/var空间使用异常且节点inode数偏大即文件数(几百K甚至上M）。du -sh目录层层逐一查下去看的时候，发现/var/spool/clientmqueue这个目录，里面文件有好几十w甚至上百w个。随手Google下才了解这个问题的原因是crontab的一些计划任务中产生了大量日志信息。这些日志信息没有导入到/dev/null或者指定的文件。结果就产生了目录下的这些文件。本来这些文件要作为mail发出去的，而因为sendmail服务先前就已经关闭的，积留在这里了。故crontab的计划任务最好全部写成这个样子：
    30 3 * * * sh /root/cron/dbbackup.sh  >/dev/null 2>&1   
<!-- More -->
其中这里2 > &1表示标准错误也放到标准输出，而标准输出导入到/dev/null；即日志信息直接/dev/null。如果特别需要的话，可指定到文件。   
这里还有一个注意的地方就是> /dev/null 2 > &1的顺序，否则标准错误不能导入到标准输出，而还是输入clientmqueue目录下   

问题原因基本就是如此，所以写crontab计划任务还是需多注意，以避免此类问题重现。那么就是已有的文件删除问题了，但几点需注意：1、不要直接删除rm -rf /var/spool/clientmqueue整个目录，这个目录还是蛮重要的，很可能会引起系统问题;2、删除文件本来很简单，但是一下子删除几十万上百万个文件如：rm * 很可能会同我一样遇到报错说Argument list too long，这个个人理解可能应该是跟最大打开的文件柄数有关。   

简单的解决办法：
    a、借助xargs。xargs其实就是把传给它的列表一个一个的传给它的命令去一个一个的执行。具体的命令为ls |xargs rm -f    
    b、利用find -delete。具体命令为find /var/spool/clientmqueue/ -type f -delete

