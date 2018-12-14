---
title: 解决CentOS服务器使用Yum连接源失败的问题
date: 2018-12-14 15:00:08
tags: CentOS
---

今天又协调到一台服务器，CentOS操作系统的，准备部署Node.js服务。

上来第一步就用yum update进行更新，更新时发现有很多源连接失败，提示`404 Not Found`之类的。仍然能搜索到更新，但是安装失败。

由于使用`yum install`仍然可用，所以暂时没理这个问题。所有环境都部署好了，需要使用git下截代码的时候，提示yum install无法安装git，这个时候其实也可以下载代码zip，然后到服务器解压，但是想到后期如果有代码更新可能会比较麻烦，所以考虑解决这个问题。

到网上搜到了这个[解决方案](http://blog.51cto.com/17532/2298743)，照着做竟然解决了。

1.首先备份/etc/yum.repos.d/CentOS-Base.repo
```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

2.到[这里](http://mirrors.163.com/.help/centos.html)下载相应版本的repo文件

3.将下载后的repo文件改名为`CentOS-Base.repo`，并使用rz上传到`/etc/yum.repos.d/`目录下。

4.清理并重置缓存
```bash
yum clean all
yum makecache
```

然后`yum update`和`yum install git`都可以正常使用了，问题解决！