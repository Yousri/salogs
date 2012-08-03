---
layout: post
title: "SSH密钥代理管理应用"
date: 2012-01-05 16:15
comments: true
categories: [linux,系统管理] 
---
对于ssh公密钥认证免密码设置想必基本都比较熟悉不够，详见：[SSH Without a Password](http://www.csua.berkeley.edu/~ranga/notes/ssh_nopass.html)     
但这里想记录的是关于日常使用过程中遇到些小麻烦所延伸的问题，因为通常会设置此认证方式原因莫过于一、安全考虑;二、服务器量多管理操作方便等原因。情况大致是，本地机器local，分别有服务器：s1和s2等等，私钥放在local，公钥分别放在两台服务器，那local登录到两台服务器都可以不再需要密码认证。但s1与s2直接通信的话还是需要输入密码（如果限制了密码方式登录那不是更麻烦了?）

因此通过搜索引擎了解到，可以通过ssh代理转发功能及代理认证解决，其中用到ssh-agent 就是一个管理私钥的代理，受管理的私钥通过 ssh-add 来添加，所有 ssh-agent 的客户端都可以共享使用这些私钥;以及开启代理转发ForwardAgent yes或者ssh -A选项开启。   
其实：ssh-agent起到的效果有2点：一、不用每次重复输入密码;二、私钥只需部署于本地同样可在远程服务器下调用。   

大致配置操作：
    1.vim /etc/ssh/ssh\_config 或~/.ssh/config 修改配置ForwardAgent yes (若没开启需每次连接时使用ssh -A选项进行开启转发代理)
    2.启动ssh-agent
    3.使用ssh-add将私钥添加到管理私钥的代理---ssh-agent下，
        ssh-add ~/.ssh/id\_rsa #添加
        ssh-add -l   #查看

如此一来，不管s1-->s2还是s2-->s1都可以使用本地的私钥进行验证而无需输入密码。

<!-- more -->
但是，解决的还不够彻底，在同样情况下的延伸中(所有服务器上都有公钥，从local连入s1，再从s1使用SSH命令连扩s2，再从s2使用SSH命令连入s3)这时候，s3依然要要求输入密码进行验证。其实关于此问题，由刚前面对于ForwardAgent选项的配置便很好理解：即连接端需存在私钥或由上个连接端的私钥代理的转发层层传递下来的，否则连接时需在前个连接时候就使用ssh -A参数临时开启。故解决措施有二：
    1.在s1连接s2的时候，ssh命令临时使用附加参数：-A 开启允许转发认证代理的连接，这样s2连接s3或其他（同样道理要想层层皆可那就需循环每每连接时都使用附加参数"-A"选项使得下次连接可用）
    2. 直接一次性修改配置文件~/.ssh/config中的ForwardAgent yes发布到各个服务器用户配置目录下。
很显然第二种方法较可取，省得每次连接还得多加"-A"参数。   

现在只要服务器已加了公钥且开启了ForwardAgent后，从本地local连接出去后再连接任何一台都通畅方便啊。   

以上是所测试本地local系统环境是在Linux(ubuntu11.10)终端下的，对于Win系统环境可以借助私钥管理工具[pageant](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)及ssh客户端工具Putty/SecureCRT，详细配置见：[SSH免密码认证进阶使用](http://xiaobin.net/201106/ssh-key-auth-and-ssh-agent/) 



