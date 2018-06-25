---
title: 使用Mapnik发布简单的地图服务
date: 2018-06-25 11:11:04
tags: 
	- GIS
	- Mapnik
---

[Mapnik](http://mapnik.org)是一个开放源代码的地图学工具集，提供基于客户端与服务器端的地图渲染使用，以`C++`撰写。Mapnik的原始开发者，Artem Pavlenko将目标明确设置为通过使用子像素反锯齿功能来创建美丽的地图。Mapnik现在也有Cairo渲染后端。Mapnik使用`Boost C++ Libraries`来处理如存储器管理、访问文件系统、正规表达式与解析XML等的一般软件工作。地图的外观可以使用XML文件来定义，也可以通过`C++`、`Python`与`Node.js`等编程语言来建构。

本文将要介绍在Ubuntu服务器上部署node-mapnik，通过配置好的XML发布地图服务，并使用Openlayers显示该地图服务。

```
说明：下面介绍的各个步骤可以实现上述目标，但不一定是最简、最优流程，可能存在多余的过程，就如其它教程里要求编译mapnik、boost之类的，经验证不需要这步操作。
```

#### 1.安装Node.js
如果没有安装Node.js，需要安装Node.js LTS版本(目前是8.11.3，最新版安装node-mapnik好像出问题了)。如果没有安装过可以使用命令进行安装：
```bash
apt-get install nodejs
apt-get install npm
```
安装之后验证版本：
![安装Node.js](nodemapniktest/1.jpg)
由于npm访问国外网站过慢，可以切换成淘宝npm镜像：
```bash
npm config set registry https://registry.npm.taobao.org
```
Node.js版本和npm版都不是最新的，需要进行更新，这里使用`n`模块进行升级：
```bash
npm install -g n
```

安装完成之后就可以通过命令安装latest、stable、lts和指定版本等，这里安装lts版本：
```bash
n lts
```
![安装Node.js](nodemapniktest/2.jpg)

安装完成之后发现node.js和npm版本没有发生变化，需要将原来的node删除，并创建一个软链接到n目录下的node，所在位置如下：
![安装Node.js](nodemapniktest/3.jpg)
```bash
rm /usr/bin/node
ln -s /usr/local/n/versions/node/8.11.3/bin/node /usr/bin/node
rm /usr/bin/npm
ln -s /usr/local/n/versions/node/8.11.3/bin/npm /usr/bin/npm
```
![安装Node.js](nodemapniktest/4.jpg)
npm升级到最新版本
```bash
npm i -g npm
```
![安装Node.js](nodemapniktest/5.jpg)

#### 2.安装node-mapnik
全局安装node-gyp和node-pre-gyp：
```bash
npm install -g node-gyp
npm install -g node-pre-gyp
```
node-mapnik模块我没有全局安装，安装到当前目录下。首先创建文件夹并进入：
![安装node-mapnik](nodemapniktest/6.jpg)
安装node-mapnik模块：
```bash
npm install mapnik
```
如果出现安装失败的情况，有的是因为一些资源下载失败，尝试科学上网、不断重试直到安装成功。
![安装node-mapnik](nodemapniktest/7.jpg)
![安装node-mapnik](nodemapniktest/8.jpg)

安装成功后，我们验证一下安装是否成功，我们使用[node-mapnik github](https://github.com/mapnik/node-mapnik)上的示例代码进行测试：
```javascript
var mapnik = require('mapnik');
new mapnik.Image.open('input.jpg').save('output.png');
```
使用vim创建test.js测试脚本，并写入上述代码，并保存：
![安装node-mapnik](nodemapniktest/9.jpg)
拷入input.jpg(可以使用xshell的rz，我用的是Windows10的Ubuntu子系统，直接把input.jpg拷贝到`C:\Users\PaiConor\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs\home\node-mapnik`目录下就可以了)，启动测试脚本，如果生成了output.png，则mapnik模块安装成功。
![安装node-mapnik](nodemapniktest/10.jpg)

#### 3.配置Mapnik配图样式XML

#### 4.发布地图服务

#### 5.使用Openlayers访问地图服务