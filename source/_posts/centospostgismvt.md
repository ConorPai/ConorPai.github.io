---
title: CentOS下使用PostGIS的ST_AsMVT函数
date: 2019-01-22 14:55:10
tags: 
	- CentOS
	- PostGIS
	- PostGISMVT
	- Vector Tiles
---

最近准备在生产环境(CentOS系统)上试用矢量瓦片，所有都部署妥当之后请求矢量瓦片报错，Missing  libprotobuf-c。使用yum安装，包括源码编译protobuf-c之后，还是报这个错，后来看了PostGIS的源码，判断是由于CentOS里使用yum安装的PostGIS版本，编译时没有protobuf-c环境，所以如果需要用ST_AsMVT函数，需要配置好protobuf-c之后，重新编译PostGIS才可以。
![源码](centospostgismvt/1.png)
![源码](centospostgismvt/2.png)

#### 首先编译protobuf和protobuf-c
从Git上下载[protobuf](https://github.com/protocolbuffers/protobuf)和[protobuf-c](https://github.com/protobuf-c/protobuf-c)代码，并依次编译。

这两个编译比较简单，略过。。

#### 编译PostGIS
下载2.5.1版本PostGIS源码，然后使用`./autogen.sh`脚本初始化。
![初始化](centospostgismvt/3.png)
初始化完成之后，使用`./configure`检查编译环境，我这里报了几个错，依次处理就可以了。

指向了旧版本Postgresql上：
![环境检查](centospostgismvt/4.png)
Postgresql版本是通过pg_config命令获取的，所以只需要查找到10版本的pg_config命令位置，做为configure参数传入即可。
![环境检查](centospostgismvt/5.png)
![环境检查](centospostgismvt/6.png)

安装各个组件的devel开发版本，包括`postgresql10-devel`、`geos37-devel`、`proj-devel`。

configure顺利完成：
![环境检查](centospostgismvt/7.png)

可以看到PROTOBUFC的检查状态是yes，就可以使用make和make install进行编译部署了。
![环境检查](centospostgismvt/8.png)