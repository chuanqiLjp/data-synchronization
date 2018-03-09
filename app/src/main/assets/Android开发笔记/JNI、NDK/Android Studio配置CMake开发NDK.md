---
title: Android Studio配置CMake开发NDK
layout: post
date: 2017-09-29 10:40:58
comments: true
categories:
  - Android
  - NDK、JNI
tags: [AndroidStudio配置,CMake,NDK,JNI]
keywords: AndroidStudio配置,CMake,NDK,JNI
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
# 1.在SDK Tools中勾选安装CMake、LLDB、NDK

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-e9531961f5c65186.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 2.配置一些快捷方式
###参数讲解
```
    javah   用于生成头文件
    Program：$JDKPath$/bin/javah
    Parameters：-d ../jni -jni $FileClass$
    Working directory：$SourcepathEntry$\..\java
    ndk-build   用于构建so包
    注意：MAC/Linux用ndk-build，没有.cmd后缀
    Program：D:\adt\sdk\ndk-bundle\ndk-build.cmd
    Parameters：什么都不用填
    Working directory：$ModuleFileDir$\src\main
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-5a1438d28dbe723c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-2a6c6bcc5c069069.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 3.在工程的local.properties文件中配置NDK的目录
```
sdk.dir=C\:\\Users\\yuxue\\AppData\\Local\\Android\\sdk
ndk.dir=C\:\\Users\\yuxue\\AppData\\Local\\Android\\sdk\\ndk-bundle
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-15b2ce962dd442aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

也可以使用图形界面，单击模块选择Open Moude Setting，选择好NDK的路径

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-f57aac9b51c62945.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 4.编译时如果检查NDK过时了可以在gradle.properties文件中增加“android.useDeprecatedNdk=true”使它可以使用过时的NDK
```
android.useDeprecatedNdk=true
```
# 5.创建CMakeLists.txt文件并放在模块的的根目录
```
# 设置构建本地库所需的最小版本的cbuild。
cmake_minimum_required(VERSION 3.4.1)
# 创建并命名一个库，将其设置为静态
#  或者共享，并提供其源代码的相对路径。
# 您可以定义多个库，而cbuild为您构建它们。
#  Gradle自动将共享库与你的APK打包。
add_library( hello-lib  #设置库的名称。即SO文件的名称，生产的so文件为“libhello-lib.so”,在加载的时候“System.loadLibrary("hello-lib");”
                SHARED  # 将库设置为共享库。
                src/main/jni/hello.cpp    # 提供一个源文件的相对路径
                src/main/jni/helloJni.cpp    # 提供同一个SO文件中的另一个源文件的相对路径
              )
#搜索指定的预构建库，并将该路径存储为一个变量。因为cbuild默认包含了搜索路径中的系统库，所以您只需要指定您想要添加的公共NDK库的名称。cbuild在完成构建之前验证这个库是否存在。
find_library(log-lib  # 设置path变量的名称。
              log   #  指定NDK库的名称 你想让CMake来定位。
               )
#指定库的库应该链接到你的目标库。您可以链接多个库，比如在这个构建脚本中定义的库、预构建的第三方库或系统库。
target_link_libraries( hello-lib     #指定目标库中。与 add_library的库名称一定要相同
                       ${log-lib}    # 将目标库链接到日志库包含在NDK。
                        )
#如果需要生产多个SO文件的话，写法如下
add_library( natave-lib  #设置库的名称。另一个so文件的名称
                SHARED  # 将库设置为共享库。
                src/main/jni/nataveJni.cpp    # 提供一个源文件的相对路径
              )
target_link_libraries( natave-lib     #指定目标库中。与 add_library的库名称一定要相同
                       ${log-lib}    # 将目标库链接到日志库包含在NDK。
                        )
```
# 6.在模块的build.gradle文件中添加
```
android {
    compileSdkVersion 26
    buildToolsVersion "26.0.0"
    defaultConfig {
        externalNativeBuild {
            cmake {
                cppFlags ""
            }
        }
    }
    externalNativeBuild {
        cmake {
            path "CMakeLists.txt"
        }
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-630ad87d7a0c9440.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 7.编写Java中的Native方法
```
    public native  String getStr();
    public native  String gethelloJniStr();
```
 # 8.生成C的头文件

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-82edca6bbcdda37c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 生成后

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-be6f2c0e939d8996.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# 9.编写C函数
```
#include "stdio.h"
#include "jni.h"
#include "string"
extern  "C"
JNIEXPORT jstring JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_getStr
        (JNIEnv *env,
jobject jobject1){
    return env->NewStringUTF("hello world from cpp");
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-431666dd67d04c1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 至此，这个项目就可以运行了

# 更多的学习

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-72bdd9f8f072b8d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
(1) .externalNativeBuild文件夹：用于存放cmake编译好的文件，包括支持的各种硬件等信息，有点类似于build.gradle文件明确Gradle如何编译APP；
(2) cpp文件夹：存放C/C++代码文件，native-lib.cpp文件默认生成的；
(3) CMakeLists.txt：cmake脚本配置文件，cmake会根据该脚本文件中的指令去编译相关的C/C++源文件，并将编译后产物生成共享库或静态块，然后Gradle将其打包到APK中。
```
CMakeLists.txt文件解析如下：
```
# 指定cmke版本
cmake_minimum_required(VERSION3.4.1)
# add_library()命令用于向CMake添加依赖源文件或库
# 指令需传入三个参数（函数库名称、库类型、依赖源文件相对路径）
add_library(  # 生成函数库的名称，即libnative-lib.so或libnative-lib.a(lib和.so/.a默认缺省)
             native-lib
             # 生成库类型：动态库为SHARED，静态库为STATIC
             SHARED
             # 依赖的c/cpp文件(相对路径)
             src/main/cpp/native-lib.cpp )
# find_library()命令用于定位NDK中的库
# 需传入两个参数(path变量、ndk库名称)
find_library(  # 设置path变量的名称，这里为NDK中的日志库
              log-lib
                            #指定cmake查询库的名称
                            #即在ndk开发包中查询liblog.so函数库，将其路径赋值给log-lib
              log )
#target_link_libraries()命令用于指定要关联到的原生库的库
target_link_libraries(# 指定目标库，与上面指定的函数库名一致
                  native-lib
                  # 链接的库，根据log-lib变量对应liblog.so函数库
                  ${log-lib} )
```
###### 通过查看native-lib.cpp方法，stringFromJNI目的是向Java层返回一个字符串。如果要在native-lib.cpp文件中添加新的方法，必须添加在extern"C" { } 中，或者在每个方法前加extern"C", 否则会报找不到方法。如果源文件为C，则须将extern“C”部分去掉，因为extern "C"的作用就是告诉编译器以C方式编译。

### JNI开发打印日志
```
#include <android/log.h>
#define LOG_TAG "System.out.c"
#define LOGD(...) __android_log_print(ANDROID_LOG_DEBUG, LOG_TAG, __VA_ARGS__)
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, LOG_TAG, __VA_ARGS__)
#define LOGW(...) __android_log_print(ANDROID_LOG_WARN, LOG_TAG, __VA_ARGS__)
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, LOG_TAG, __VA_ARGS__)

LOG的使用
    LOGD("TAGD,a=%d,b=%d",a,b);
    LOGI("TAGI,a=%d,b=%d",a,b);
    LOGW("TAGW,a=%d,b=%d",a,b);
    LOGE("TAGE,a=%d,b=%d",a,b);
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-652ac51a570a032d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Android系统目前支持的CPU架构
```
ARMv5，ARMv7 (从2010年起)
x86 (从2011年起)
MIPS (从2012年起)
ARMv8，MIPS64和x86_64 (从2014年起)
每一个CPU架构对应一个ABI
CPU架构           ABI
ARMv5   --->    armeabi
ARMv7   --->    armeabi-v7a
x86     --->    x86
MIPS    --->    mips
ARMv8   --->    arm64-v8a
MIPS64  --->    mips64
x86_64  --->    x86_64
armeabi：默认选项，将创建以基于ARM* v5TE 的设备为目标的库。 具有这种目标
的浮点运算使用软件浮点运算。 使用此ABI（二进制接口）创建的二进制代码将可以
在所有 ARM*设备上运行。所以armeabi通用性很强。但是速度慢
armeabi-v7a：创建支持基于ARM* v7 的设备的库，并将使用硬件FPU指令。
armeabi-v7a是针对有浮点运算或高级扩展功能的arm v7 cpu。
mips：MIPS是世界上很流行的一种RISC处理器。MIPS的意思是“无内部互锁流水级
的微处理器”(Microprocessor without interlocked piped stages)，其机
制是尽量利用软件办法避免流水线中的数据相关问题。
x86：支持基于硬件的浮点运算的IA-32 指令集。x86是可以兼容armeabi平台运行
的，无论是armeabi-v7a还是armeabi，同时带来的也是性能上的损耗，另外需要
指出的是，打包出的x86的so，总会比armeabi平台的体积更小。
总结
如果项目只包含了 armeabi，那么在所有Android设备都可以运行；
如果项目只包含了 armeabi-v7a，除armeabi架构的设备外都可以运行；
如果项目只包含了 x86，那么armeabi架构和armeabi-v7a的Android设备是无法
运行的；
如果同时包含了 armeabi，armeabi-v7a和x86，所有设备都可以运行，程序在运
行的时候去加载不同平台对应的so，这是较为完美的一种解决方案，同时也会导致
包变大。
```
我的CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/78131635