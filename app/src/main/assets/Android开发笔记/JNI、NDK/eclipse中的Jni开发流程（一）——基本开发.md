---
title: eclipse中的Jni开发流程（一）——基本开发
layout: post
date: 2016-12-16 08:30:58
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
1、声明java的本地方法，使用native关键字 本地方法不用去实现
-----------------------------------

```

public class MainActivity extends Activity {

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	}

	public void HelloWord(View view){
		Toast.makeText(this, hellofromC(), 0).show();
	}
	public native String hellofromC();
}

```

2、在项目的根目录创建jni文件夹
-----------------

3、在jni文件夹下创建xxxx.c文件（导入三个必要的头文件《stdlib.h、stdio.h、jni.h》）
--------------------------------------------------------

①本地函数命名规则: Java_包名_类名_本地方法名（可以使用javah命令去生成）
②JNIENV* env JNIEnv 是JniNativeInterface这个结构体的一级指针
③JniNativeInterface这个结构体定义了大量的函数指针
④env 就是结构体JniNativeInterface这个结构体的二级指针
⑤(*env)->调用结构体中的函数指针
⑥第二个参数jobject 调用本地函数的java对象就是这个jobject


```
#include<stdlib.h>
#include<stdio.h>
#include<jni.h>
JNIEXPORT jstring JNICALL Java_com_example_hellojni_MainActivity_hellofromC
  (JNIEnv * env, jobject  jo){
	char* hello="Hello World From JNI !";
	return (*env)->NewStringUTF(env, hello);
}

```

4、在jni文件夹下创建Android.mk文件 makefile 告诉编译器.c的源文件在什么地方,要生成的编译对象的名字是什么
-----------------------------------------------------------------

```
Android.mk的文件内容（复制即可）
LOCAL_PATH := $(call my-dir)
include $(CLEAR_VARS)
LOCAL_MODULE := hello #指定了生成的动态链接库的名字，加载的时候就是它的名字
LOCAL_SRC_FILES := hello.c #指定了C的源文件叫什么名字
include $(BUILD_SHARED_LIBRARY)
```

5、在项目的根目录下调用ndk-build命令编译C代码，生成动态链接库libxxx.so文件 文件的位置 lib->armeabi->libxxxx.so
------------------------------------------------------------------------

6、在Java代码需要调用的地方的类中使用static代码块加载动态链接库（ System.loadlibrary("动态链接库的名字"); Android.mk的LOCAL_MODULE所指定的名字）
------------------------------------------------------------------------

```
在MainActivity中
static{
		System.loadLibrary("hello");
	}
```
至此，就可以将我们的程序跑起来了，你会了吗？下一篇我会讲Jni在eclipse中简便开发流程，欢迎继续关注！