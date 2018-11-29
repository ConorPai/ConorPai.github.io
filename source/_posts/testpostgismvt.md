---
title: 试用TileStrata-PostGISMVT插件
date: 2018-11-29 23:03:47
tags: 
	- GIS
	- PostGISMVT
	- TileStrata
	- Vector Tiles
---

之前试用过TileStrata-vtile插件，但是使用过程中发现其空间参考只支持Web墨卡托坐标系，而我们主要用的坐标系是经纬度，如果要修改需要改动node-mapnik的c++代码，比较麻烦。幸好TileStrata提供了另外一个矢量瓦片的插件：[PostGISMVT插件](https://github.com/Stezii/tilestrata-postgismvt)。

大概了解了一下源码，其实现主要是通过PostGIS2.4版本以上提供的ST_AsMVT函数和ST_AsMVTGeom实现的，而我竟然发现我们装的PostGIS在函数列表里找不到ST_AsMVT函数。。

在网上找了一些资料发现原来ST_AsMVT函数需要依赖ProtoBuf库，所以安装了下面几个库：

ubuntu：
```bash
sudo apt-get install libprotobuf-c1 libprotobuf-c-dev libprotobuf-dev protobuf-compiler protobuf-c-compiler
```

安装完成之后ST_AsMVT调用不再报错，然而还是在函数列表里找不到。

待续。。。
