---
title: Eclipse和Android-Studio使用 LeakCanary
layout: post
date: 2017-02-15 15:56:32
updated: 2017-02-15 15:56:32
comments: true
categories:
  - Android
  - 性能优化
tags: [LeakCanary,内存泄露检测]
keywords: LeakCanary,内存泄露检测
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

## 版权声明：商业转载请联系我获得授权，非商业转载请在醒目位置注明出处。

# LeakCanary简介

我们经常被OOM所困扰，引起OOM往往都是内存泄漏长期没有解决造成的，如果在对象的生命周期本该结束的时候，这个对象还被一系列的引用，这就会导致内存泄漏，随着泄漏的累积，app将消耗完内存，直到OOM，[LeakCanary](https://github.com/square/leakcanary) 是一个开源的在debug版本中检测内存泄漏的java库。下面介绍其使用方法：
# 在Eclipse中使用
1. 下载为Eclipse优化的LeakCanary，下载链接  http://download.csdn.net/detail/wo_ha/9755042；
2. 将项目导入Eclipse中；
3. 将LeakCanary作为自己项目的依赖库（右键单击自己的项目----->Properties----->Android----->在Libary选择Add----->选择导入的Leakcanary项目----->Apply----->OK），若出现V4包报错，请选择其中一个项目的V4包去替换另一个项目的V4包，参考[http://blog.csdn.net/jackrex/article/details/8984033](http://blog.csdn.net/jackrex/article/details/8984033)；
4. 在自己项目的AndroidManifest中添加权限和相关的Activity、Service；
```
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <activity
            android:name="com.squareup.leakcanary.internal.DisplayLeakActivity"
            android:enabled="false"
            android:icon="@drawable/__leak_canary_icon"
            android:label="@string/__leak_canary_display_activity_label"
            android:taskAffinity="com.squareup.leakcanary"
            android:theme="@style/__LeakCanary.Base" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <service
            android:name="com.squareup.leakcanary.internal.HeapAnalyzerService"
            android:enabled="false"
            android:process=":leakcanary" />
        <service
            android:name="com.squareup.leakcanary.DisplayLeakService"
            android:enabled="false" />
```

5. 自定义一个 Application；
```
public class ExampleApplication extends Application {
    public static RefWatcher getRefWatcher(Context context) {
        ExampleApplication application = (ExampleApplication) context.getApplicationContext();
        return application.refWatcher;
    }
    private RefWatcher refWatcher;
    @Override public void onCreate() {
        super.onCreate();
        refWatcher = LeakCanary.install(this);
    }
}
别忘在AndroidManifest的Application节点添加name哦
```
6. 在需要观察的Activity的Destory方法添加如下代码；
 ```
    @Override
    protected void onDestroy() {
        super.onDestroy();
        RefWatcher refWatcher = ExampleApplication.getRefWatcher(this);
        refWatcher.watch(this);
    }
```
好啦，把LeakCanary集成到我们的Eclipse项目中就完成了，如果有内存泄漏如下图，本讲解例子的源码：http://download.csdn.net/detail/wo_ha/9755057

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-451ba18e6c8186c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 在Android Studio中使用
这可比在Eclipse中使用简单多了，只需要在需要的Mode的gradle中添加如下代码在同步下就可以了，使用的方法都是一样的，我就不贴代码了
```
dependencies {
    .......
    debugCompile 'com.squareup.leakcanary:leakcanary-android:1.5'
    releaseCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.5'
    testCompile 'com.squareup.leakcanary:leakcanary-android-no-op:1.5'
}
```
