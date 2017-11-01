---
title: 使用Hexo搭建个人博客的详细教程
date: 2017-10-31 22:18:44
tags: hexo
---

## 1.基础环境配置

**1.1 申请GitHub帐号**
到GitHub[官网](https://github.com/)注册个人帐号。

**1.2 配置node.js**
1)到Node.js[官网](https://nodejs.org/)下载相应平台的最新版本安装，或者使用系统软件包管理器(Homebrew、apt-get等)进行安装(MacOS上的小坑：使用brew和安装包同时安装Node.js会冲突，目前我的电脑上无法全局安装模块，还没来得及找解决方案)。


2)配置淘宝数据源，加快npm模块安装速度
``` bash
$ npm config set registry https://registry.npm.taobao.org
```
**1.3 配置git**
1)安装git
2)全局配置
``` bash
$ git config --global baiguangnan@163.com
$ git config --global user.name ConorPai
```
3)生成密钥，密码设置为空
``` bash
$ ssh-keygen -t rsa -C baiguangnan@163.com
```

**1.4 安装Hexo**
``` bash
$ npm install -g hexo
```

## 2.配置Hexo目录
TODO