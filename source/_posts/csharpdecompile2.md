---
title: 试水C#成果反编译(二)
date: 2018-06-08 09:49:49
tags: C#
---

上一篇介绍了使用ILSpy和Reflexil插件对简单程序的反编译和破解。现在用一个真实软件为例，对其进行破解。

这里破解的软件是代码转换工具[Converters](https://www.tangiblesoftwaresolutions.com/product_details/products.html)，可以转换C#、Java、C++和VB.Net四种语言，功能比较强大，转换之后的代码可用度比较高。这个软件免费版功能受限，高级版要上百刀。在网上看到了这篇[参考文章](http://www.cnblogs.com/yiyan127/p/CSharp_CrackJava2CSharpConverter.html)，所以也想试试对其进行破解。

首先用ILSpy打开CSharp to Java Converter软件
![打开软件](csharpdecompile2/1.png)
可以看到，这个成果是做过混淆的，里面的类名称、函数名称和变量名称都变成了意义不明的代号。

因为ILSpy中的搜索不是很好用，所以需要将代码保存之后，用VS打开查找和定位相关代码。
![保存代码](csharpdecompile2/2.png)
![保存代码](csharpdecompile2/3.png)

我们想破解这个软件对用户授权的判断，就需要找一个突破口。在这里我以程序标题提示Free Edition为突破口，来查找授权判断代码。
![突破口](csharpdecompile2/4.png)

全局搜索Free Edition，找到拼接程序标题的代码：
![定位代码](csharpdecompile2/5.png)
![定位代码](csharpdecompile2/6.png)
可以看出标题是否显示FreeEdition是由App._g13变量控制的，现在只需要找到App._g13变量在哪里赋值即可：
![定位代码](csharpdecompile2/7.png)
可以看出在程序启动后，通过App.Copyright_Tangible_Software_Solutions_g30方法对App._g13进行赋值，定位Copyright_Tangible_Software_Solutions_g30方法：
![定位代码](csharpdecompile2/8.png)
也就是说只需要把这个方法的返回值直接改为true即可。

返回ILSpy软件，搜索Copyright_Tangible_Software_Solutions_g30：
![定位代码](csharpdecompile2/9.png)
打开Reflexil插件，先用Replace All with Code方法修改：
![修改代码](csharpdecompile2/10.png)
![修改代码](csharpdecompile2/11.png)
用这种方法修改的代码编译报错，所以这种方法行不通，考虑修改IL源码。

对于修改IL源码不是特别清楚的小伙伴，可以新建一个简单的工程，将写好后的代码编译并在ILSpy中打开，照着原样拷贝到这里即可。
![修改代码](csharpdecompile2/12.png)
就是这里的6行代码。

清除所有
![修改代码](csharpdecompile2/13.png)
依次创建
![修改代码](csharpdecompile2/14.png)
![修改代码](csharpdecompile2/15.png)
![修改代码](csharpdecompile2/16.png)
由于第4行是一个跳转指令，所以最后创建
![修改代码](csharpdecompile2/17.png)
![修改代码](csharpdecompile2/18.png)
添加跳转指令
![修改代码](csharpdecompile2/19.png)
默认添加是在最后一行，需要拖拽到相应位置
![修改代码](csharpdecompile2/20.png)
![修改代码](csharpdecompile2/21.png)

修改之后启动程序并没有达到想要的效果，使用ILSpy打开破解之后的程序，发现这里的代码不并是return true，有些不是很理解，在一个简单程序上做如下修改，却是正常的。
![修改代码](csharpdecompile2/22.png)

憋了很久，打算硬着头皮试一下，猜想这6行代码里，ldc.i4.1是创建了一个数值为1的int变量，ret是返回，是不是只保留这两行就可以了呢？修改并验证：
![修改代码](csharpdecompile2/23.png)
![修改代码](csharpdecompile2/24.png)
发现代码正常了，启动软件测试：
![验证结果](csharpdecompile2/25.png)
发现程序标题中的Free Edition已经没有了，原来Unlock the Premium Edition按钮也没有了，测试转换代码也没有行数之类的限制，破解生效了。

俗话说，知己知彼百战不殆，了解反编译和破解过程，是为了更好的保护自主知识版权，希望大家多多支持正版软件。