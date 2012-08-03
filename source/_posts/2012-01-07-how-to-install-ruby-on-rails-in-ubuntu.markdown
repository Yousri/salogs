---
layout: post
title: "Ubuntu 11.10安装配置Ruby on Rails开发环境"
date: 2012-01-07 09:46
comments: true
categories: [Linux,Rails] 
---
1.初始系统安装开发包
    $ sudo apt-get install build-essential
2.安装RVM
    $ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)
测试查看是否已经安装成功:
    $ rvm -v
    rvm 1.10.1 by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.beginrescueend.com/]
修改用户bash相应配置文件~/.bashrc，确保每次用户登录时RVM脚本都会载入
    $ vim ~/.bashrc
    [[ -s "$HOME/.rvm/scripts/rvm" ]] && . "$HOME/.rvm/scripts/rvm" #增加此行
    $ source ~/.bashrc  
<!-- more -->

3.借助RVM安装Ruby环境
    $ rvm notes
可以列出标准版的Ruby所需的依赖包：
    $ sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev
安装ruby和rubygems：
    $ rvm list known  #列出哪些版本的Ruby可安装
    $ rvm install 1.9.2
    $ rvm --default use 1.9.2
检查确认下Ruby和RubyGems的版本：
    $ ruby -v
    $ gem -v
4.安装Rails环境
Rails 全部都打包在 Rails Gem 中。安装它是这个教程中最容易的部分。使用 RubyGems 来安装它，即 gem 命令。安装完成后，检查其版本来确保正确安装。
    $ sudo gem install bundler rails
    $ rails -v 
    注:必要时需修改下系统环境变量PATH：export /usr/local/bin/ruby:$PATH

