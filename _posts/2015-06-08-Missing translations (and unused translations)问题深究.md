---
layout: post
title:  "Missing translations (and unused translations)问题深究"
date:   2015-06-08 20:49:28
categories: Android
---

* content
{:toc}

今天在打包apk文件时，Android Studio出现以下错误：

![http://7xk2i5.com1.z0.glb.clouddn.com/er.png](http://7xk2i5.com1.z0.glb.clouddn.com/er.png)

当然，第一次遇到这种问题我也没啥头绪，打开错误所在文件发现没啥问题。便立即查找解决方法。  
百度不大靠谱，在stackoverflow上有相关解决方法，[见此](http://stackoverflow.com/questions/21118725/error-app-name-is-not-translated-in-af)。

当然前几个答案都是针对Eclipse的，有参考意义，对Android Studio依然无解。  
但在最后一个答案中给出一种很好的解决方法：

![http://7xk2i5.com1.z0.glb.clouddn.com/solution.png](http://7xk2i5.com1.z0.glb.clouddn.com/solution.png)

果然，在`build.gradle`的`android{ }`块中添加这两行代码可以顺利解决问题。

但解决问题，不代表懂！继续上网查找相关资料，留意到解决方法中的`lintOptions`方法，更换搜索词。

在[这篇文章](http://blog.csdn.net/hudashi/article/details/8333349)中介绍了Lint，也很好的解释了这个问题。

这里指出Android Lint是Android工程源代码进行扫描和检查，发现潜在问题的工具。  

其中检查的错误包括Missing translations (and unused translations)没有翻译的文本。即此问题报错类型。

详细仍需参考[这篇文章](http://blog.csdn.net/hudashi/article/details/8333349)，及其参考文章：[英文原文](http://tools.android.com/tips/lint)、[参照文章](http://blog.csdn.net/thl789/article/details/8037473)。

