---
title: 使用OpenCV和TensorFlow实现猫主子识别(-)——提取猫脸图片
date: 2018-05-10 09:09:13
tags: 机器学习
---

最近某端公司部门技术分享想做一个老板检测软件，即一旦老板出现则马上报警-_-!!!。
我也大概想了一下实现思路，而且搜到了OpenCV自带了猫脸检测的分类器，突发奇想干脆给我家五只猫主子做一个动态识别器吧。

```
实现分为几个步骤：
1.随机截取五只猫主子的猫脸图片，并进行保存
2.对猫脸图片进行分类标注
3.使用TensorFlow对分类标注结果进行训练
4.对监测时截取的猫脸图片进行识别，看是哪位猫主子飘过
```

今天实现第一步：随机截取五只猫主子的猫脸图片，并进行保存。

猫脸检测分类器在[GitHub](https://github.com/opencv/opencv/tree/master/data/haarcascades)上可以下载到，其中的`haarcascade_frontalcatface.xml`和`haarcascade_frontalcatface_extended.xml`即为猫脸检测的分类器。

OpenCV使用的检测方法是`detectMultiScale()`，此方法的作用是，在输入图像中检测不同尺寸的对象，返回包含对象的矩形框。它接收的参数：

```
1.image——输入图像
2.scaleFactor——表示每轮检测图像齿轮减少的比例
3.minNeighbors——指明对象要至少被检测到几次才能判定对象确实存在
4.minSize——检测对象的最小尺寸
5.maxSize——检测对象的最大尺寸
```

完整实现代码如下：
```python
#coding=utf-8
#作用：从摄像头中提取识别出来的猫脸图片，保存并准备进行下一阶段训练使用

#引用相关库
import cv2
import datetime

#使用OpenCV打开摄像头
cap = cv2.VideoCapture(0)
#分类器初始化
cascade_path = 'haarcascade_frontalcatface_extended.xml'
cascade = cv2.CascadeClassifier(cascade_path)
#循环读取摄像头中的每一帧画面
while cap.isOpened():
    #读取一帧画面
    _, frame = cap.read()
    #灰度处理
    frame_gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    #猫脸检测
    facerect = cascade.detectMultiScale(frame_gray, scaleFactor=1.1, minNeighbors=5, minSize=(5, 5))
    #如果检测到结果
    if len(facerect) > 0:
        #输出日志
        print('face detected')
        #获取当前时间，用来保存该帧画面及猫脸图片
        t = datetime.datetime.now()
        #循环遍历检测到的猫脸区域
        for (i, (x, y, w, h)) in enumerate(facerect):
            #在猫脸区域画出方框
            cv2.rectangle(frame, (x, y), (x + w, y + h), (255, 255, 255), thickness=2)
            #使用ROI获取猫脸区域图像
            roiImg = frame[y:y + h, x:x + w]
            #将猫脸区域图像保存成文件
            cv2.imwrite('save/' + str(t) + '-' + str(i) + '.jpg', roiImg)
        #将标识出猫脸区域的当前帧保存，方便以后标注
        cv2.imwrite('save/' + str(t) + '.jpg', frame)
    #未检测到结果
    else:
        #输出日志
        print('face not detected')
    #显示标识出猫脸区域的当前帧画面
    cv2.imshow("capture", frame)
    #检测ESC按钮，按下则退出
    k = cv2.waitKey(100)
    if k == 27:
        break
#释放资源
cap.release()
cv2.destroyAllWindows()
```

实现效果：
![实现效果](opencv-catface\result.png)
