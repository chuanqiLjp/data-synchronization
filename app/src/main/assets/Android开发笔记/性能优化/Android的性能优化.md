---
title: Android的性能优化
layout: post
date: 2018-03-12 20:39:00
updated: 2018-03-12 20:39:00
comments: true
categories:
  - Android
  - 性能优化
tags: [布局优化,绘制优化,内存泄露优化,ListView优化,Bitmap优化,数据库的优化]
keywords: 布局优化,绘制优化,内存泄露优化,ListView优化,Bitmap优化,数据库的优化
description: 布局优化,绘制优化,内存泄露优化,ListView优化,Bitmap优化,数据库的优化
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

## 版权声明：商业转载请联系我获得授权，非商业转载请在醒目位置注明出处。

## 1、布局优化
1. 使用Lint（AS -> Analyze -> Inspect code） — 查看你的view 层级哪些地方可以优化；
2. 删除布局中无用的控件和层级；
3. 使用include标签重用布局文件；
4. 尽量减少内嵌的层级—>可考虑使用merge标签【删减多余的层级】；
5. 使用ViewStub标签按需加载所需的布局文件；

## 2、绘制优化
1. 在onDraw方法中不要创建新的局部变量；
2. 在onDraw方法不做耗时操作和避免循环；

## 3、内存泄露优化（MAT分析和LeakCanary分析检测内存泄露）
1. 单例模式导致的内存泄露 —> 不要持有Activity或Fragment的引用改用Application的Context；
2. 属性动画导致的内存泄露：开启一个重复的动画没有在onDestroy中停止播放；
3. 非静态内部内的静态实例，非静态内部类会维持一个到外部类实例的引用，如果非静态内部类的实例是静态的，就会间接长期维持着外部类的引用，阻止被回收掉，可以使用静态内部类和WeakReference代替。
4. 资源对象未关闭，资源性对象如Cursor、File、Socket，应该在使用后及时关闭。未在finally中关闭；
5. 注册对象未反注册，未反注册会导致观察者列表里维持着对象的引用，阻止垃圾回收。在必要的地方及时反注册，如广播，EventBus；
6. Handler临时性内存泄露，一般将Handler定义为静态的，推荐使用静态内部类+弱引用 WeakReference 这种方式，但要注意每次使用前判空
7. 避免Bitmap的浪费，临时bitmap的主动回收Bitmap，bitmap.recycle();bitmap=null;
8. 使用软引用保存对象，当内存紧张时会释放，使用弱引用保存对象，当发生GC操作时释放对象
9. 对象的复用：复用系统的资源，ListView的ConvertView复用，避免在onDraw方法里执行对象的创建
10. 类的静态变量持有大数据对象，不使用时及时置为null；
11. Try catch某些大内存的分配的操作；

## 4、ListView优化
1. 复用convertView
2. 缓存item条目的引用，减少findViewbyId—>ViewHolder
3. 数据的 分页/分批 加载：对大量的数据进行分页展示，对不同的滚动状态进行分别处理，在快速滑动状态不加载数据
4. 图片的缓存，需要解决图片错位问题—>推荐使用成熟框架Glide或Picasso
5. 根据列表的滑动状态来控制任务的执行频率（在快速滑动时不要加载图片）
6. 可以开启硬件加速使ListView更加流畅（android:hardwareAccelerated="true"）
7. 将ListView的scrollingCache和animateCache这两个属性设置为false（默认是true）;
8. 避免GC（可以从LOGCAT查看有无GC的LOG）；
9. 尽可能减少List Item的Layout层次（如可以使用RelativeLayout替换LinearLayout，或使用自定的View代替组合嵌套使用的Layout）；

## 5、Bitmap优化
1. 避免Bitmap的浪费，临时bitmap的主动回收Bitmap，bitmap.recycle();bitmap=null;
2. 使用三级缓存，内存-sd卡-网络,将大图片用BitmapFactory压缩采样处理(使用inSampleSize参数)再放到内存中；

## 6、数据库的优化
1. 尽量利用原生的SQL语句，原生的SQL省去了拼接sql语句的步骤，要比SqliteDatabase提供的insert、query、 update、delete等函数效率高。当数据库越大，差别也越大；
2. 当操作条数较多时，利用事务进行批处理，这样SQLite将把全部要执行的SQL语句先缓存在内存当中，然后等到COMMIT的时候一次性的写入数据库，这样数据库文件只被打开关闭了一次，效率自然大大的提高；

## 7、其他优化
1. 响应速度优化并避免ANR，分析ANR文件（/data/anr/traces.txt）；
2. 尽量避免使用枚举（枚举占用空间大）；
3. 线程优化：采用线程池避免线程的创建和销毁所带来的性能开销；