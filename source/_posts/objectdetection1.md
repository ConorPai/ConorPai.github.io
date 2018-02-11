---
title: 使用TensorFlow进行目标识别(一)——数据处理
date: 2018-02-11 13:37:57
tags: 机器学习,tensorflow,object detection
---

最近支付宝集福卡活动和抖音小年抢红包活动都是应用了手掌识别技术，所以我也想使用TensorFlow的object detection实现一个自己的手掌识别。

1.下载手掌图片
从[Hand Images Databases](https://www.mutah.edu.jo/biometrix/hand-images-databases.html)中下载手掌图片。
这里面有两种手掌图片，一种是mobile hand images database (MOHI)，即通过手机摄像头拍摄的图片，清晰度比较高，图片大小比较大；另一种是webcam hand images database (WEHI)，即通过网络摄 像头拍摄的图片，清晰度一般，图片大小比较小。

因为总的图片比较多，所以这里进行了拆分，两类图片都有3个Session，每个Session有4个Group，每个Group里面有50个人的手掌图片，每个人有5张图片。

将下载后的图片解压到Images目录下。
2.数据集标注
因为TensorFlow的object detection训练使用的数据集为TFRecord格式，需要对数据集进行标注及处理。
首先使用[LabelImg](https://github.com/tzutalin/labelImg)工具对图片中手掌的具体位置进行标注。下载Linux版LabelImg标注工具，解压后直接运行labelImg
![](objectdetection1/1.png)
打开Images目录
![](objectdetection1/2.png)
使用Create RectBox功能拉框标注手掌所在区域
![](objectdetection1/3.png)
编辑标注
![](objectdetection1/4.png)
如果对框选区域不满意还可以进行编辑
![](objectdetection1/5.png)
将标注结果xml保存到Images/xmls目录下，注意保存路径中不能包含中文，否则保存不成功
![](objectdetection1/6.png)

待续...