---
title: eclipse中的Jni开发流程（二）——JNI简便的开发流程
layout: post
date: 2016-12-17 22:52:58
comments: true
categories:
  - Android
  - NDK、JNI
tags: [eclipse配置,JNI]
keywords: eclipse配置,JNI
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：商业转载请联系我获得授权，非商业转载请在醒目位置注明出处。


[1.eclipse中的Jni开发流程（一）](http://blog.csdn.net/wo_ha/article/details/53687903)
[2.eclipse中的Jni开发流程（二）](http://blog.csdn.net/wo_ha/article/details/53715936)
[3.Android Studio配置CMake开发NDK](http://blog.csdn.net/wo_ha/article/details/78131635)
上一篇我们讲了JNI在eclipse中的基本开发流程，觉得有点繁杂，且没有代码提示，我们这篇讲个简单的
① 写java代码 使用native 声明本地方法
-------------------------

② 添加本地支持
--------

 右键单击项目->andorid tools->add native surport--->点击Finish（此时会自动生成jni文件夹且在文件夹下自动生成  .cpp和Android.mk文件）如果发现 finish不能点击需要给工作空间配置ndk目录的位置：window->preferences->左侧选择android->ndk 把ndk解压的目录指定进来

③ 如果写的是.c的文件 先修改一下生成的.cpp文件的扩展名 ，同时相应修改Android.mk文件中LOCAL_SRC_FILES的值
---------------------------------------------------------------------

④ 使用javah命令生成头文件将里面的方法拷贝到刚才的.C文件中，然后删除生成的头文件
--------------------------------------------

⑤ 此时发现报错，解决CDT插件报错的问题
---------------------

：右键单击项目选择 properties 选测 c/c++ general->paths and symbols->include选项卡下->点击add..->file system 选择ndk目录下 platforms文件夹 对应平台下(项目支持的最小版本)
usr 目录下 arch-arm -> include 确定后 会解决代码提示和报错的问题
![这里写图片描述](http://img.blog.csdn.net/20161217225703034?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29faGE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20161217225728987?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd29faGE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


⑥编写C函数 如果需要单独编译一下c代码就在c/c++视图中找到小锤子 如果想直接运行到模拟器上 就不用锤子了
-------------------------------------------------------

⑦ java代码的 static{ System.loadlibrary(".......") ; }
---------------------------------------------------