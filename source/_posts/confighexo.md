---
title: 使用Hexo搭建个人博客的详细教程
date: 2017-10-31 22:18:44
tags: hexo
---

## 1.基础环境配置

**1.1 申请GitHub帐号**
到GitHub[官网](https://github.com/)注册个人帐号。

**1.2 配置node.js**
1)到Node.js[官网](https://nodejs.org/)下载相应平台的最新版本安装，或者使用系统软件包管理器(`Homebrew`、`apt-get`等)进行安装(MacOS上的小坑：使用brew和安装包同时安装Node.js会冲突，目前我的电脑上无法全局安装模块，还没来得及找解决方案)。

2)配置淘宝数据源，加快npm模块安装速度
``` bash
$ npm config set registry https://registry.npm.taobao.org
```
**1.3 配置git**
1)安装git
2)全局配置
``` bash
$ git config --global user.email baiguangnan@163.com
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

**2.1 命令行进入博客文件存放目录**
``` bash
$ mkdir myblog
$ cd myblog
```
**2.2 初始化**
``` bash
$ hexo init
```
**2.3 安装依赖模块**
``` bash
$ npm install
```
**2.4 创建新的文章**
``` bash
$ hexo new "MyNewPost"
```
**2.5 生成博客页面**
``` bash
$ hexo clean 	#清理之前的缓存
$ hexo g		#生成博客页面
$ hexo s		#发布本地服务，访问http://localhost:4000/即可查看
```
**2.6 修改博客主题**
目前本博客使用的是[yilia](https://github.com/litten/hexo-theme-yilia)主题，配置方式如下：
1）下载主题
``` bash
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```
2）配置主题
修改`themes/yilia/_config.yml`中的各项内容，具体配置可以参考[yilia作者博客主题配置](https://github.com/litten/BlogBackup/blob/master/_config.yml)及[本博主题配置](https://github.com/ConorPai/ConorPai.github.io/blob/backup/themes/yilia/_config.yml)
3）应用主题
修改`_config.yml`中的`theme: landscape`改为`theme: yilia`
4）清理并重新生成
``` bash
$ hexo clean
$ hexo g
```
## 3.编写博客
TODO
## 4.发布到Github Pages
TODO
## 5.绑定域名
TODO
## 6.博客内容多端同步
TODO