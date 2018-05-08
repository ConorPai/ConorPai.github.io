---
title: C#垃圾回收机制新的理解
date: 2018-05-08 10:49:31
tags: C#
---

最近程序中报了一个十分诡异的异常。调试状态下不出现，直接运行EXE时100%报错。根据已往的经验肯定是垃圾回收机制的锅，因为在调试状态下变量保存的时间会比较长，直接运行回收比较频繁，只要变量被回收就会报错。

这里说明一下为什么C#变量被回收了会报错，是因为我们底层使用C++开发，上层封装成C#类，虽然上层看起来不再使用这个变量了，但是底层可能会用到，所以一旦被销毁底层就会报错。

以往出现这种问题主要有两种解决方案：1.看上层代码调用是否有问题；2.在底层加日志反馈具体的报错信息。但是看了一遍代码并没有发现有什么问题，在底层加了比较全的日志反而不报错了，对底层日志做了删减发现在某一处加了日志之后不报错，不加就报错，但是这个位置也没有看出来什么问题，都是正常的调用。

头疼了很久也没有找出来解决方案，感觉有可能是内存错乱导致的崩溃，这就比较麻烦了。所以决定完全重写上层代码，用最简模式减少上层干扰，代码重写完成后发现了一个新的问题，代码大概如下：
```csharp
public void funtion1()
{
    //打开数据源
    DataSource ds = IWorkspaceFactory.Open();
    
    //调用方法进行大量运算、频繁申请内存等操作
    StaticClass.StaticFuction(ds);
    
    //其它无关代码
}
```

就是在StaticClass.StaticFuction执行过程中，发现数据源被释放了，导致报错。
但是根据我以往的经验引用型变量在出作用域之后才会被垃圾回收，怎么提前回收了？看了半天发现之后的代码没有再用到数据源，难到是CLR分析到变量不再使用而提前释放了？
为了验证这个问题，我在StaticClass.StaticFuction之后调用了数据源的一个无关紧要的方法，大概如下：
```csharp
public void funtion1()
{
    //打开数据源
    DataSource ds = IWorkspaceFactory.Open();
    
    //调用方法进行大量运算、频繁申请内存等操作
    StaticClass.StaticFuction(ds);
    
    //调用数据源方法，验证数据源是否得以保留
    ds.GetTableCount();
    
    //其它无关代码
}
```

验证结果是不再报错正常运行，即上面的结论是正确的。

反观之前报错部分的上层代码，也是同样问题，修改后可以正常运行。
```csharp
//大量循环、查询、空间分析等操作
foreach (KeyValuePair<string, List<int>> pVar in pDicUnValue)
{
    //创建查询条件
    IMZQueryDef pQuerydef = new mzQueryDef();
    pQuerydef.QueryType = mzQueryType.mzQueryGeneral;
    
    //查询(在底层执行查询过程中，上层回收了查询条件，导致底层再用查询条件时报错)
    IMZFeatureCursor mzFeatuCur = m_sourceLayer.DatasetVector.Query(pQuerydef);
	
    //无关紧要代码，防止查询条件被释放
    string s = pQuerydef.QueryFilter;
    
    //其它无关代码
}
```
上面的代码是我们比较常见的空间查询代码，一直没有出现过类似问题，看来是由于我们进行了大量的空间查询、内存申请等操作，导致CLR尽可能多的回收内存空间以保证程序正常运行，才出现这样的问题。