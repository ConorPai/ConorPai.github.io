---
title: 利用无人机拼接正射影像图(二)
date: 2018-06-04 15:29:46
tags: 无人机
---

#### 6.内业：照片拼接成正射影像图
经过外业照片采集之后，通过内业软件进行拼接，形成正射影像图。

##### 内业正射影像图拼接软件有以下几种：
1.OpenDroneMap
2.PhotoScan
3.Pix4dmapper
4.ENVI OneButton

###### 1.[OpenDroneMap](http://opendronemap.org)
OpenDroneMap是一款开源软件，可以对无人机照片进行拼接处理，生成正射影像图、DEM、纹理、点云等，实验证明拼接的效果不是很理想，而且没有UI操作界面用起来难度比较大。具体的拼接流程可以参考[这篇文章](https://mp.weixin.qq.com/s?__biz=MzUzNDc0NjQ5NA==&mid=2247484330&amp;idx=1&amp;sn=2fd17bbb9d9117344c3c0769a6a8bd21&source=41#wechat_redirect)。

##### 2.PhotoScan
PhotoScan可以使用GPU加速大大提高拼接效率，GPU加速设置在菜单Tools-Perferences中：
![GPU加速设置](dronephotoscreatedom2/9.png)
下面说一下具体的拼接流程：
1）新建Workspace
点击Workspace组件中工具栏第一个按钮新建Workspace。
![新建Workspace](dronephotoscreatedom2/1.png)
2）添加照片
点击菜单Workflow-Add Photos添加照片。
![添加照片](dronephotoscreatedom2/1.1.png)
3）排列照片
点击菜单Workflow-Align Photos。
![排列照片](dronephotoscreatedom2/2.png)
4）生成密集点云
点击菜单Workflow-Build Dense Cloud。由于有GPU加速，想试一下最高级的效果，不开GPU加速需要2天时间，开了只要20分钟，但是最高级效果在接下来生成三角网过程中报错，因为没有足够大的内存，所以这里选中等级别。
![生成密集点云](dronephotoscreatedom2/3.png)
5）生成三角网
点击菜单Workflow-Build Mesh。
![生成三角网](dronephotoscreatedom2/4.png)
6）生成纹理
点击菜单Workflow-Build Texture。
![生成纹理](dronephotoscreatedom2/5.png)
7）生成正射影像图
点击菜单Workflow-Build Orthomosaic，生成正射影像图前，需要将Workspace生成的成果保存。
![生成正射影像图](dronephotoscreatedom2/6.png)
8）导出正射影像图
点击菜单File-Export-Export Orthomosaic-Export JPEG/TIFF/PNG。
![导出正射影像图](dronephotoscreatedom2/7.png)
![导出正射影像图](dronephotoscreatedom2/8.png)

##### 3.Pix4dmapper
Pix4dmapper在前期设置之后，整个处理过程是完整的，而且会生成报告，体验比较好。
下面说一下具体的拼接流程：
1）新建项目
![新建项目](dronephotoscreatedom2/12.png)
2）添加照片
![添加照片](dronephotoscreatedom2/13.png)
![添加照片](dronephotoscreatedom2/14.png)
3）选择输出坐标系
这里没有找到4326的WGS84 经纬度坐标系，使用的是3857的WGS84 Web墨卡托投影。
![选择输出坐标系](dronephotoscreatedom2/15.png)
4）选择处理模板
![选择处理模板](dronephotoscreatedom2/16.png)
5）新建项目完成
![新建项目完成](dronephotoscreatedom2/17.png)
6）项目设置
点击左下角“处理选项”即可对项目参数进行设置，这里包括设置GPU加速。
![项目设置](dronephotoscreatedom2/18.png)
7）开始处理
![开始处理](dronephotoscreatedom2/18.1.jpg)
8）处理结果
处理完成之后，在项目目录下就保存了输出成果。
![处理结果](dronephotoscreatedom2/19.png)
正射影像图在如下位置：
![处理结果](dronephotoscreatedom2/20.png)

##### 4.ENVI OneButton
我申请了该软件的试用许可，但是还没有收到回复，等收到了试用许可再实验效果。

##### 测试正射影像图效果
目前来看OpenDroneMap成果不理想，PhotoScan和Pix4dmapper成果差不多，Pix4dmapper略好些。
和GoogleEarth影像叠加效果：
![添加照片](dronephotoscreatedom2/10.png)
放大之后和GoogleEarth影像对比效果：
![添加照片](dronephotoscreatedom2/11.png)

可以看出，还是有一些偏差的，如果需要提高精度，需要进行下一步：影像校正。

#### 7.内业：正射影像图校正
使用我们的软件或ArcMap都可以进行影像校正，在这里以ArcGIS为例演示校正效果。
1）在ArcGIS工具栏上添加“地理配准”工具条
2）为了方便校准，创建矢量图层，在GoogleEarth影像中找到比较明确的地物点，勾绘成矢量成果。
3）将正射影像图和矢量成果加载到ArcMap中，使用地理配准工具条，向地图中添加影像校正配准点：
![影像校正](dronephotoscreatedom2/jz1.png)
所有配准点：
![影像校正](dronephotoscreatedom2/jz2.png)
4）导出校正后的影像

可以对比一下校正前后的效果：
![校正前](dronephotoscreatedom2/11.png)
![校正后](dronephotoscreatedom2/jz3.png)

