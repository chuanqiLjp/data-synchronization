---
title: Android面试一天一题
layout: post
date: 2018-03-27 17:52:58
comments: true
categories:
  - Android
  - 面试
tags: [2018面试,Android面试]
keywords: 2018面试,Android面试
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/


<h2 id="目录">
目录
</h2>

* [知道Service吗，它有几种启动方式？](#知道Service吗，它有几种启动方式？)

* [用广播来更新UI界面好吗？](#用广播来更新UI界面好吗？)

* [怎么理解Activity的生命周期？](#怎么理解Activity的生命周期？)

* [如何判断Activity是否在运行？](#如何判断Activity是否在运行？)

* [自定义View的状态是如何保存的？](#自定义View的状态是如何保存的？)

* [通过new创建的View实例它的onSaveStateInstance会被调用吗？](#通过new创建的View实例它的onSaveStateInstance会被调用吗？)

* [Java的值传递和引用传递问题](#Java的值传递和引用传递问题)

* [能讲讲Android的Handler机制吗？](#能讲讲Android的Handler机制吗？)

* [HandlerThread常规使用步骤](#HandlerThread常规使用步骤)

* [两个Activity之间如何传递参数？](#两个Activity之间如何传递参数？)

* [如何理解Android中的Context，它有什么用？](#如何理解Android中的Context，它有什么用？)

* [如何优化ListView的性能？](#如何优化ListView的性能？)

* [如何实现应用内多语言切换？](#如何实现应用内多语言切换？)

* [在项目中使用AsyncTask会有什么问题吗](#在项目中使用AsyncTask会有什么问题吗)

* [修改SharedPreferences后两种提交方式有什么区别？](#修改SharedPreferences后两种提交方式有什么区别？)

* [有使用过ContentProvider码？能说说Android为什么要设计ContentProvider这个组件吗？](#有使用过ContentProvider码？能说说Android为什么要设计ContentProvider这个组件吗？)

* [如何处理线程同步的问题？](#如何处理线程同步的问题？)

* [如何准备自我介绍](#如何准备自我介绍)

* [如何对SQLite数据库中进行大量的数据插入？](#如何对SQLite数据库中进行大量的数据插入？)

* [Activity的启动模式（launchMode）有哪些，有什么区别？](#Activity的启动模式（launchMode）有哪些，有什么区别？)

* [Android资源目录的读取顺序？](#Android资源目录的读取顺序？)

* [有没有遇到OOM的问题？如何优化图片占用的内存空间？](#有没有遇到OOM的问题？如何优化图片占用的内存空间？)

* [Android中Java和JavaScript如何交互？](#Android中Java和JavaScript如何交互？)

* [两个Fragment之间如何进行通信？](#两个Fragment之间如何进行通信？)

* [如何理解Android应用的进程？](#如何理解Android应用的进程？)

* [如何解决ScrollView嵌套中一个ListView的滑动冲突？](#如何解决ScrollView嵌套中一个ListView的滑动冲突？)

* [知道什么是ART吗？它和Dalvik有什么区别？](#知道什么是ART吗？它和Dalvik有什么区别？)

* [如何检测内存泄露，如何进行内存优化？](#如何检测内存泄露，如何进行内存优化？)

* [如何准备和Boss（或经理）的面试](#如何准备和Boss（或经理）的面试)

* [你在Android开发中遇到的技术难题是什么，你是怎么解决的？](#你在Android开发中遇到的技术难题是什么，你是怎么解决的？)

* [谈谈你使用过的Android开源库，是否有遇到过什么问题？](#谈谈你使用过的Android开源库，是否有遇到过什么问题？)

* [谈谈MVP和MVVM模式，你有在自己的项目中使用过吗？](#谈谈MVP和MVVM模式，你有在自己的项目中使用过吗？)

* [如何快速突击Android面试](#如何快速突击Android面试)

* [介绍一下你经常浏览的Android技术网站](#介绍一下你经常浏览的Android技术网站)

* [Binder是什么？它是如何实现跨进程通信的？](#Binder是什么？它是如何实现跨进程通信的？)

* [一套高级工程师的面试题](#一套高级工程师的面试题)

* [达到如下要求的简历可以认为是好的简历。](#达到如下要求的简历可以认为是好的简历。)

* [你有写博客或者其他的输出吗？如果有，谈谈你的经历或者看法。](#你有写博客或者其他的输出吗？如果有，谈谈你的经历或者看法。)

* [如何系统学习Android开发？](#如何系统学习Android开发？)

* [你有使用过Kotlin来开发Android应用吗？说说Kotlin和Java有什么区别？](#你有使用过Kotlin来开发Android应用吗？说说Kotlin和Java有什么区别？)

* [你是如何解决Android的布局嵌套问题的？](#你是如何解决Android的布局嵌套问题的？)

* [设计模式](#设计模式)

* [离面试不到24小时，该准备什么？](#离面试不到24小时，该准备什么？)

* [Java内存模型](#Java内存模型)

* [Binder的线程数](#Binder的线程数)

* [我的问题问完了，你有什么要问的吗？](#我的问题问完了，你有什么要问的吗？)

[返回目录](#目录)


****

<h2 id="知道Service吗，它有几种启动方式？">
知道Service吗，它有几种启动方式？
</h2>

[返回目录](#目录)

> Service是一个专门在后台处理长时间任务的Android组件，它没有UI。它有两种启动方式，startService和bindService。

这两种启动方式的区别。

> **startService** 只是启动Service，启动它的组件（如Activity）和Service并没有关联，只有当Service调用stopSelf或者其他组件调用stopService服务才会终止。

>**bindService** 方法启动Service，其他组件可以通过回调获取Service的代理对象和Service交互，而这两方也进行了绑定，当启动方销毁时，Service也会自动进行unBind操作，当发现所有绑定都进行了unBind时才会销毁Service。

![image.png](https://upload-images.jianshu.io/upload_images/4143664-01f4027a3cc793a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Service的onCreate回调函数可以做耗时的操作吗？
如果需要做耗时的操作，你会怎么做？
是否知道IntentService，在什么场景下使用IntentService？
如果一个应用要从网络上下载MP3文件，并在Activity上展示进度条，这个Activity要求是可以转屏的。那么在转屏时Actvitiy会重启，如何保证下载的进度条能正确展示进度呢？
```
没有Service概念的人，一般想出来的方案如下：

在转屏前将进度缓存，转屏后再读出来。
使用android:configChanges设置，让转屏时Activity不销毁和重建。
针对第1个方案，我会继续问他将进度值存在哪里？ 转屏的过程中，我们知道Activity的重建算是比较耗时的，会可能会有几百毫秒以上，那么这时候下载线程仍然在工作，进度肯定和保存时的进度不一致了，如何处理这个问题呢？

第2个方案，大家可以自己展开思考，实际的项目中可能会需要额外做一些事情来处理ContentView的横竖布局的问题。

如果使用Service来解决这个问题，看似是比较完美的，不过就会涉及Activity（UI）和Service的交互问题
```
当我们知道了Service的用途，心中有一个Service相关的概念时，针对实际的场景还是要做具体的分析再决定是否使用Service。因为Service仍然是在主线程中调用，还是要开线程才能处理长时间的工作，Service和UI的交互也让这个方式变得不那么简便。如果你只需要在当前界面去做一些耗时操作，界面退出或改变时，工作也要停止，那么这时直接使用Thread（或者AsyncTask, ThreadHandler）会更合适你。
链接：https://www.jianshu.com/p/7a7db9f8692d

****

<h2 id="用广播来更新UI界面好吗？">
用广播来更新UI界面好吗？
</h2>

[返回目录](#目录)

> Normal broadcasts无序广播，会异步的发送给所有的Receiver，接收到广播的顺序是不确定的，有可能是同时。
> Ordered broadcasts有序广播，广播会先发送给优先级高(android:priority)的Receiver，而且这个Receiver有权决定是继续发送到下一个Receiver或者是直接终止广播。

除了上面的两种广播外，还有其他类型的广播吗？
> 可以使用sendStickyBroadcast发送Sticky类型的广播。Sticky简单说就是，在发送广播时Reciever还没有被注册，但它注册后还是可以收到在它之前发送的那条广播。

有时候基于数据安全考虑，我们想发送广播只有自己（本进程）能接收到，那么该如何去做呢？
```
在我不知道有新的API或者框架时我常常喜欢用自己现有的知识去想方案，最后再Google一下看是否有更好的。这个问题，我会先想到权限，发送广播时限定有权限（receiverPermission）的接收者才能收到。但是我们知道APK太容易被反编译，注册广播的权限也只是一个字符串，并不安全。
然后可能使用Handler，没错，往主线程的消息池（Message Queue）发送消息，只有主线程的Handler可以分发处理它，广播发送的内容是一个Intent对象，我们可以直接用Message封装一下，留一个和sendBroadcast一样的接口。在handleMessage时把Intent对象传递给已注册的Receiver。
后来在看项目组的其他同事写代码时，发现还有一个LocalBroadcastManager类，查了一下官方文档是Support V4包里的一个类，其实现方式也是使用Handler，思路也是一样的。
```

BroadcastReceiver的生命周期？
> 有些人并不态清楚Receiver也是有生命周期的，而且很短，当它的onReceive方法执行完成后，它的生命周期就结束了。这时BroadcastReceiver已经不处于active状态，被系统杀掉的概率极高，也就是说如果你在onReceive去开线程进行异步操作或者打开Dialog都有可能在没达到你要的结果时进程就被系统杀掉。因为这个进程可能只有这个Receiver这个组件在运行，当Receiver也执行完后就是一个empty进程，是最容易被系统杀掉的。替代的方案是用Notificaiton或者Service（这种情况当然不能用bindService）。

更新界面也分很多种情况，如果不是频繁地刷新，使用广播来做也是可以的。但对于较频繁地刷新动作，建议还是不要使用这种方式。广播的发送和接收是有一定的代价的，它的传输是通过Binder进程间通信机制来实现的（细心人会发现Intent是实现了Parcelable接口的），那么系统定会为了广播能顺利传递做一些进程间通信的准备。
我们要先了解Android的ActivityManagerService有一个专门的消息队列来接收发送出来的广播，sendBroadcast执行完后就立即返回，但这时发送来的广播只是被放入到队列，并不一定马上被处理。当处理到当前广播时，又会把这个广播分发给注册的广播接收分发器ReceiverDispatcher，ReceiverDispatcher最后又把广播交给接Receiver所在的线程的消息队列去处理（就是你熟悉的UI线程的Message Queue）。整个过程从发送--ActivityManagerService--ReceiverDispatcher进行了两次Binder进程间通信，最后还要交到UI的消息队列，如果基中有一个消息的处理阻塞了UI，当然也会延迟你的onReceive的执行。

链接：https://www.jianshu.com/p/df7af437e766

****

<h2 id="怎么理解Activity的生命周期？">
怎么理解Activity的生命周期？
</h2>

[返回目录](#目录)

![image.png](https://upload-images.jianshu.io/upload_images/4143664-cbe2256120660b75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果一个Activity在用户可见时才处理某个广播，不可见时注销掉，那么应该在哪两个生命周期的回调方法去注册和注销BroadcastReceiver呢？
> Activity 的可见生命周期发生在 onStart调用与 onStop调用之间。在这段时间，用户可以在屏幕上看到 Activity 并与其交互。我们可以在 onStart中注册一个 BroadcastReceiver以监控影响 UI 的变化，并在用户无法再看到您显示的内容时在 onStop中将其取消注册,如果对方回答是在onResume和onPause方法中，那么你可以去引导对方看看在这两个方法有什么不好的地方。

如果有一些数据在Activity跳转时（或者离开时）要保存到数据库，那么你认为是在onPause好还是在onStop执行这个操作好呢？
> 这题的要点和上一题是一样的，onPause较容易被触发，所以我们在做BroadcastReceiver注销时放在onStop要好些。onPause时Activity界面仍然是可见的，如弹出一个Dialog时。但在保存数据时，放在onPause去做可以保证数据存储的有效性，如果放在onStop去做，在某些情况下Activity走完onPause后有可能还没顺利走到onStop就被系统回收了。但要注意在onPause中要非常迅速地执行完所需操作，不然会影响到下一个Activity的生命周期函数的调用。

简单说一下Activity A启动Activity B时，两个Activity生命周期的变化。
> 当一个 Activity 启动另一个 Activity 时，它们都会发生生命周期转变。第一个 Activity 暂停然后停止（但如果它在后台仍然可见，则不会停止，比如B是半透明的），系统会创建另一个 Activity。 如果这两个Activity 共用保存数据到文件或者数据库，必须要注意，在创建第二个 Activity 前，第一个 Activity 不会完全停止。更确切地说，启动第二个 Activity 的过程与停止第一个 Activity 的过程存在重叠。以下是当 Activity A 启动 Activity B 时一系列操作的发生顺序：Activity A 的 onPause方法执行。Activity B 的 onCreate、onStart和 onResume方法依次执行。然后，如果 Activity A 在屏幕上不再可见，则其 onStop方法执行。您可以利用这种可预测的生命周期回调顺序管理从一个 Activity 到另一个 Activity 的信息转变。 例如，如果您必须在第一个 Activity 停止时向数据库写入数据，以便下一个 Activity 能够读取该数据，则应在 onPause而不是 onStop执行期间向数据库写入数据。

链接：https://www.jianshu.com/p/ae6e1d93cc8e

****

<h2 id="如何判断Activity是否在运行？">
如何判断Activity是否在运行？
</h2>

[返回目录](#目录)

从Activity A 启动一个线程去进行网络上传操作，在A中设立一个回调函数，当上传操作完成以后，在A的这个回调函数中会弹出一个对话框，用来显示回调信息。可是当上传的过程还在进行的时候，我按下back键，A的activity 被销毁了，可是那个上传的线程还在进行，当那个线程结束后，本来应该在A中弹出一个对话框，可是由于A已经不存在了，系统就会报错提示，“将对话框显示在不存在的页面上”，然后程序就挂掉了。
我看到过很多人用Handler来充当上面所提到的“回调函数”，即工作线程完成工作后，通过主线程的Handler处理UI更新，如弹出提示Dialog。可能有些人没有弄明白，Activity都被系统销毁了，工作线程怎么还能调它的变量呢？其实所谓的Activity销毁只是不再受系统的AMS控制，但Activity这个对象的实例还是存在于内存中的，具体什么时候真正把这个对象实例也销毁（回收）了，就要看内存回收机制了，哪怕是这个实例没有可达的引用了也不一定会马上回收。

```
activity == null || activity.isDestroyed() || activity.isFinishing()
```
链接： https://www.jianshu.com/p/f8a0c43b3dfe

****

<h2 id="自定义View的状态是如何保存的？">
自定义View的状态是如何保存的？
</h2>

[返回目录](#目录)

![image.png](https://upload-images.jianshu.io/upload_images/4143664-803f5f56ca59b10c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> Activity类的onSaveInstanceState默认实现会恢复Activity的状态，默认实现会为布局中的每个View调用相应的 onSaveInstanceState方法，让每个View都能保存自身的信息。这里需要注意一个细节：想要保存View的状态，需要在XML布局文件中提供一个唯一的ID（android:id），如果没有设置这个ID的话，View控件的onSaveInstanceState是不会被调用的。

无法保证系统会在销毁Activity前一定调用onSaveInstanceState，例如用户使用“返回” 按退出 Activity 时，因为用户的行为是在显式关闭 Activity，所以不会调用onSaveInstanceState。

如果系统调用onSaveInstanceState，那么它是在onStop还是在onPause之前执行呢？
> 可以肯定的是它会在调用 onStop之前，但是是不是在onPuase之前就不能确认了，要看情况，官方文档在说明这个执行顺序时用了“可能”这个词。

在自定义View内实现一个静态内部类SavedState，并继承自BaseSavedState，这样就能得到一个Parcelable对象了。当然，你也可以直接写一个类实现Parcelable接口来保存状态,
```
@Override
protected Parcelable onSaveInstanceState() {
    Parcelable parcelable = super.onSaveInstanceState();
    SavedState ss = new SavedState(parcelable);
    //把当前的位置保存进SavedState
    ss.currentPosition = mCurrentPosition;
    return ss;
}
static class SavedState extends BaseSavedState{
    //当前的ViewPager的位置
    int currentPosition;
    public SavedState(Parcel source) {
        super(source);
        currentPosition = source.readInt();
    }
    public SavedState(Parcelable superState) {
        super(superState);
    }
    @Override
    public void writeToParcel(Parcel out, int flags) {
        super.writeToParcel(out, flags);
        out.writeInt(currentPosition);
    }
    public static final Parcelable.Creator<SavedState> CREATOR = new Parcelable.Creator<SavedState>(){

        @Override
        public SavedState createFromParcel(Parcel source) {
            return new SavedState(source);
        }
        @Override
        public SavedState[] newArray(int size) {
            return new SavedState[size];
        }
    };
}
```

自定义View的状态恢复

上面对自定义View的状态进行了保存，接下来我们就要恢复这些状态。既然有了已经保存的状态，那么恢复状态也很简单了，因为在onRestoreInstanceState(Parcelable)方法内，根据传递进来的Parcelable参数，我们可以拿到我们之前保存的数据，再根据需要进行赋值或者调用某些方法来恢复状态就行了。比如说，CheckBox，之前保存的是是否选择了这个CheckBox的状态，那么恢复就应该对CheckBox重新选中或不选中。同样以BannerViewPager为例，重写onRestoreInstanceState(Parcelable)方法如下：
```
@Override
protected void onRestoreInstanceState(Parcelable state) {
    SavedState ss = (SavedState) state;
    super.onRestoreInstanceState(ss.getSuperState());
    //调用别的方法，把保存的数据重新赋值给当前的自定义View
    mViewPager.setCurrentItem(ss.currentPosition);
}
```
> 以后如果我们的自定义View要嵌套在另一个自定义View内作为子View，必须注意该父容器有没有重写了dispatchSaveInstanceState方法，或者说有没有把保存-恢复事件传递给子View


[Android 如何保存与恢复自定义View的状态？](http://blog.csdn.net/a553181867/article/details/54633151)
链接：https://www.jianshu.com/p/1071b9c48f1e

****

<h2 id="通过new创建的View实例它的onSaveStateInstance会被调用吗？">
通过new创建的View实例它的onSaveStateInstance会被调用吗？
</h2>

[返回目录](#目录)
自定义View控件的状态被保存需要满足两个条件：
1. View有唯一的ID；
2. View的初始化时要调用 setSaveEnabled(true) ；

链接：https://www.jianshu.com/p/4f482548de59

****

<h2 id="Java的值传递和引用传递问题">
Java的值传递和引用传递问题
</h2>

[返回目录](#目录)

> “在Java里面参数传递都是按值传递”这句话的意思是：按值传递是传递的值的拷贝，按引用传递其实传递的是引用的地址值，所以统称按值传递。简单的说，基本类型是按值传递的，方法的实参是一个原值的复本。类对象是按对象的引用地址（内存地址）传递地址的值，那么在方法内对这个对象进行修改是会直接反应在原对象上的（或者说这两个引用指向同一内存地址）。不过要注意String这个类型

有很多人并不重视基础的问题，总认为不知道也无防，用的时候有问题自然会报出来，到时候再解决就好了，你知道的也没比我多能耐。我只能说，知道的话确实不比别人多能耐，只是多了一份从容。

链接： https://www.jianshu.com/p/c0c5e0540928

****

<h2 id="能讲讲Android的Handler机制吗？">
能讲讲Android的Handler机制吗？
</h2>

[返回目录](#目录)

> Message：消息分为硬件产生的消息(如按钮、触摸)和软件生成的消息；

>  MessageQueue：消息队列的主要功能向消息池投递消息(MessageQueue.enqueueMessage)和取走消息池的消息(MessageQueue.next)；

>  Handler：消息辅助类，主要功能向消息池发送各种消息事件(Handler.sendMessage)和处理相应消息事件(Handler.handleMessage)；

>  Looper：不断循环执行(Looper.loop)，按分发机制将消息分发给目标处理者。

链接：https://www.jianshu.com/p/108db0240a34

****

<h2 id="HandlerThread常规使用步骤">
HandlerThread常规使用步骤
</h2>

[返回目录](#目录)

1. 创建实例对象 :HandlerThread handlerThread = new HandlerThread("downloadImage");
2. 启动HandlerThread线程  :handlerThread.start();//必须先开启线程
3. 构建循环消息处理机制
```
  /**
     * 该callback运行于子线程
     */
    class ChildCallback implements Handler.Callback {
        @Override
        public boolean handleMessage(Message msg) {
            //在子线程中进行相应的网络请求

            //通知主线程去更新UI
            mUIHandler.sendMessage(msg1);
            return false;
        }
    }
```
4. 构建异步handler :Handler childHandler = new Handler(handlerThread.getLooper(),new ChildCallback());//子线程Handler

> 当调用quitSafely方法时，其内部调用的是Looper的quitSafely方法而最终执行的是MessageQueue中的removeAllFutureMessagesLocked方法，该方法只会清空MessageQueue消息池中所有的延迟消息，并将消息池中所有的非延迟消息派发出去让Handler去处理完成后才停止Looper循环，quitSafely相比于quit方法安全的原因在于清空消息之前会派发所有的非延迟消息。最后需要注意的是Looper的quit方法是基于API 1，而Looper的quitSafely方法则是基于API 18的;

HandlerThread的特点
```
1、HandlerThread将loop转到子线程中处理，说白了就是将分担MainLooper的工作量，降低了主线程的压力，使主界面更流畅。
2、开启一个线程起到多个线程的作用。处理任务是串行执行，按消息发送顺序进行处理。HandlerThread本质是一个线程，在线程内部，代码是串行处理的。
3、但是由于每一个任务都将以队列的方式逐个被执行到，一旦队列中有某个任务执行时间过长，那么就会导致后续的任务都会被延迟处理。
4、HandlerThread拥有自己的消息队列，它不会干扰或阻塞UI线程。
5、对于网络IO操作，HandlerThread并不适合，因为它只有一个线程，还得排队一个一个等着。
```
链接： http://blog.csdn.net/javazejian/article/details/52426353

****

<h2 id="两个Activity之间如何传递参数？">
两个Activity之间如何传递参数？
</h2>

[返回目录](#目录)

> 使用Intent的Bundle协带参数，就是我们常用的Intent.putExtra方法。

除了传递基本类型外，如何传递自定义的对象呢？
> 这个问题就是想引出Android的Parcelable。一般很多面试者都有用过传递实现了Serializable接口的自定义对象的经验，因为这个很简单，加句代码就搞定了。而Parcelable的实现要多一些代码

这Parcelable和Serializable的区别：
> Serializalbe会使用反射，序列化和反序列化过程需要大量I/O操作，Parcelable自已实现封送和解封（marshalled &unmarshalled）操作不需要用反射，数据也存放在Native内存中，效率要快很多。

Parcelable和Parcle这两者之间的关系。
> Parcelable 接口定义在封送/解封送过程中混合和分解对象的契约。Parcelable接口的底层是Parcel容器对象。Parcel类是一种最快的序列化/反序列化机制，专为Android中的进程间通信而设计。该类提供了一些方法来将成员容纳到容器中，以及从容器展开成员。

现在我们知道了如何传递自定义的对象，那么在两个Activity之前传递对象还要注意什么呢？
> 一定要要注意对象的大小，Intent中的Bundle是在使用Binder机制进行数据传递的，能使用的Binder的缓冲区是有大小限制的（有些手机是2M），而一个进程默认有16个binder线程，所以一个线程能占用的缓冲区就更小了（以前做过测试，大约一个线程可以占用128KB）。所以当你看到“The Binder transaction failed because it was too large.”这类TransactionTooLargeException异常时，你应该知道怎么解决了。因此，使用Intent在Activity之间传递List和Bitmap对象是有风险的。

>还有一个要注意的：因为android不同版本Parcelable可能不同，所以不推荐使用Parcelable进行数据持久化。之前我有过一次，将Android的PackageInfo进行持久化到数据库，结果用户升级Android系统后，再从数据库解封PackageInfo时应用就Crash了。

Parcelable与Serializable的性能比较
```
首先Parcelable的性能要强于Serializable的原因我需要简单的阐述一下
1）. 在内存的使用中,前者在性能方面要强于后者
2）. 后者在序列化操作的时候会产生大量的临时变量,(原因是使用了反射机制)从而导致GC的频繁调用,因此在性能上会稍微逊色
3）. Parcelable是以Ibinder作为信息载体的.在内存上的开销比较小,因此在内存之间进行数据传递的时候,Android推荐使用Parcelable,既然是内存方面比价有优势,那么自然就要优先选择.
4）. 在读写数据的时候,Parcelable是在内存中直接进行读写,而Serializable是通过使用IO流的形式将数据读写入在硬盘上.
但是：虽然Parcelable的性能要强于Serializable,但是仍然有特殊的情况需要使用Serializable,而不去使用Parcelable,因为Parcelable无法将数据进行持久化,因此在将数据保存在磁盘的时候,仍然需要使用后者,因为前者无法很好的将数据进行持久化.(原因是在不同的Android版本当中,Parcelable可能会不同,因此数据的持久化方面仍然是使用Serializable)
```
 
链接：https://www.jianshu.com/p/be593134eeae

****

<h2 id="如何理解Android中的Context，它有什么用？">
如何理解Android中的Context，它有什么用？
</h2>

[返回目录](#目录)

> Context提供了一个应用的运行环境，通过这个上下文应用才可以访问资源，才能完成和其他组件、服务的交互。它就是一个调用者和具体实现的桥接。

![image.png](https://upload-images.jianshu.io/upload_images/4143664-dfc1508499dbd2c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Application（或者Service）和Activity都可以调用Context的startActivity方法，那么在这两个地方调用startActivity有区别吗？
> 在Application（或者Service）需要给Intent设置Intent.FLAG_ACTIVITY_NEW_TASK才能正常启动Activity

Context的实例是什么时候创建的？一个应用里面会有几个Context的实例？

链接：https://www.jianshu.com/p/0754e65a5744

****

<h2 id="如何优化ListView的性能？">
如何优化ListView的性能？
</h2>

[返回目录](#目录)

1. 我们设置或者优化ListView的性能很多时候都是在getView中完成的，反过来说就是很多性能问题都是由于没有正确使用getView造成的。
2. 所以我在们设置Listener进就要注意，使用convertView时需要重新设置一个Listener，保括一些数据也需要重设置，不然可能会显示之前那个ItemView在回收前的状态。
3. 在绘制ListView前往往要计算它的高度，所以一个ListView界面上可以看到6个ItemView，但是getView的执行次数却有可能是12次，多出的次数用来计算高度（这个可以通过设置ListView的height为0来避免）。所以要避免在getView中进行逻辑运算，两次计算同一逻辑完全是浪费。
> 1秒之内屏幕可以完成30帧的绘制，人才能看到它比较流畅（苹果是接近60帧，高于60之后人眼也无法分辨）。每帧可使用的时间：1000ms/30 = 33.33 ms;每个ListView一般要显示6个ListItem，加上1个重用convertView：33.33ms/7 = 4.76ms;即是说，每个getView要在4.76ms内完成工作才会较流畅，但是事实上，每个getView间的调用也会有一定的间隔（有可能是由于handler在处理别的消息），UI的handler处理不好的话，这个间隔也可难会很大（0ms-200ms）。结论就是，留给getView使用的时间应该在4ms之内，如果不能控制在这之内的话，ListView的滑动就会有卡顿的现象。

```
1. 复用convertView
2. 缓存item条目的引用，减少findViewbyId—>ViewHolder
3. 数据的 分页/分批 加载：对大量的数据进行分页展示，对不同的滚动状态进行分别处理，在快速滑动状态不加载数据
4. 图片的缓存，需要解决图片错位问题—>推荐使用成熟框架Glide或Picasso
5. 根据列表的滑动状态来控制任务的执行频率（在快速滑动时不要加载图片）
6. 可以开启硬件加速使ListView更加流畅（android:hardwareAccelerated="true"）
7. 将ListView的scrollingCache和animateCache这两个属性设置为false（默认是true）;
8. 避免GC（可以从LOGCAT查看有无GC的LOG）；
9. 尽可能减少List Item的Layout层次（如可以使用RelativeLayout替换LinearLayout，或使用自定的View代替组合嵌套使用的Layout）；
```

链接：https://www.jianshu.com/p/8fd5fa90ee6c

****

<h2 id="如何实现应用内多语言切换？">
如何实现应用内多语言切换？
</h2>

[返回目录](#目录)

> 在不同的res/value-xx下放置不同语言的strings.xml实现字符的本地化，而这个value-xx目录的选择是根据Resource中的Configuration.Locale这项的值来决定的。如zh中文，就会选择value-zh目录，如果没有匹配到（即APK中没有value-zh目录）就使用默认的value目录中的字符资源。然而，我们还是会有一些业务场景需要不根据Android系统的Locale配置就改变应用的语言。实现的方式也很简单，直接调用Android开放的接口Resources.changeSystemLanguage(Context context, String language)

阻止app的文字随手机字体的调整而调整。比如你把手机字体调整到最大，那么你app内的文字也会变到最大，然后app就面目全非
```
    //在BaseActivity中重写此方法
    public Resources getResources() {
        Resources res = super.getResources();
        Configuration config = new Configuration();
        config.setToDefaults();
        res.updateConfiguration(config, res.getDisplayMetrics());
        return res;
    }
```
链接：https://www.jianshu.com/p/024f46834485

****

<h2 id="在项目中使用AsyncTask会有什么问题吗">
在项目中使用AsyncTask会有什么问题吗
</h2>

[返回目录](#目录)

1. 提供了onCancelled()方法，在主线程中执行，当异步任务被取消时，onCancelled()方法会被调用，这个时候onPostExecute方法不会被调用；
2. AsyncTask的类必须在主线程中加载，即第一次访问AsyncTask必须发生在主线程中，但这个过程在Android4.1及以上版本系统自动完成；
3. AsyncTask的对象必须在主线程中创建；
4. execute方法必须在UI线程中调用；
5. 不能在程序中直接调用onPreExecute()、onPostExecute()、doInBackground和onProgressUpdate方法；
6. 一个AsyncTask对象只能执行一次，即只能调用一次execute方法；
7. AsyncTask采用一个线程来串行的执行任务，但可以通过AsyncTask的executeOnExecutor(Executor exec,Params... params)方法来并行的执行任务；

线程池可以同时执行多少个TASK？

你在项目中，会用什么方案来替换AsyncTask呢？


链接：https://www.jianshu.com/p/c925b3ea1444

****

<h2 id="修改SharedPreferences后两种提交方式有什么区别？">
修改SharedPreferences后两种提交方式有什么区别？
</h2>

[返回目录](#目录)

1. commit这种方式很常用，在比较早的SDK版本中就有了，这种提交修改的方式是同步的，会阻塞调用它的线程，并且这个方法会返回boolean值告知保存是否成功（如果不成功，可以做一些补救措施）。
2. 而apply是异步的提交方式，目前Android Studio也会提示大家使用这种方式。
3. SharedPreferences还提供一个监听接口可以监听SharedPreferences的键值变化，需要监控键值变化的可以用registerOnSharedPreferenceChangeListener添加监听器。
4. Sharedpreferences：系统对它的读写有一定的缓存策略，即在内存中会有一份Sharedpreferences文件的缓存，因此多进程模式下系统对它的读写就不可靠；不建议在多进程中使用（可考虑ContentProvider），开源的替代方案[tray](https://github.com/grandcentrix/tray)


链接：https://www.jianshu.com/p/4dd53e1be5ba

****

<h2 id="有使用过ContentProvider码？能说说Android为什么要设计ContentProvider这个组件吗？">
有使用过ContentProvider码？能说说Android为什么要设计ContentProvider这个组件吗？
</h2>

[返回目录](#目录)

> ContentProvider应用程序间非常通用的共享数据的一种方式，也是Android官方推荐的方式。Android中许多系统应用都使用该方式实现数据共享，比如通讯录、短信等。但我遇到很多做Android开发的人都不怎么使用它，觉得直接读取数据库会更简单方便。

>android:exported属性非常重要。这个属性用于指示该服务是否能够被其他应用程序组件调用或跟它交互。如果设置为true，则能够被调用或交互，否则不能。设置为false时，只有同一个应用程序的组件或带有相同用户ID的应用程序才能启动或绑定该服务。

其设计用意在于：
1. 封装。对数据进行封装，提供统一的接口，使用者完全不必关心这些数据是在DB，XML、Preferences或者网络请求来的。当项目需求要改变数据来源时，使用我们的地方完全不需要修改。
2. 提供一种跨进程数据共享的方式
3. ContentResolver接口的notifyChange函数来通知那些注册了监控特定URI的ContentObserver对象，使得它们可以相应地执行一些处理。ContentObserver可以通过registerContentObserver进行注册。

每个ContentProvider的操作是在哪个线程中运行的呢（其实我们关心的是UI线程和工作线程）？比如我们在UI线程调用getContentResolver().query查询数据，而当数据量很大时（或者需要进行较长时间的计算）会不会阻塞UI线程呢？
1. ContentProvider和调用者在同一个进程，则ContentProvider的方法（query/insert/update/delete等）和调用者是处在同一线程中的；
2. ContentProvider和调用者在不同的进程，则ContentProvider的方法会运行在它自身所在进程的一个Binder线程中。
3. 以上两种方式在ContentProvider的方法没有执行完成前都会阻塞调用者，因此会阻塞UI线程

[Android系统匿名共享内存Ashmem（Anonymous Shared Memory）简要介绍和学习计划](http://blog.csdn.net/luoshengyang/article/details/6651971)

链接：https://www.jianshu.com/p/380231307070

****

<h2 id="如何处理线程同步的问题？">
如何处理线程同步的问题？
</h2>

[返回目录](#目录)

Object的wait和notify/notifyAll如何实现线程同步？
> 在Object.java中，定义了wait(), notify()和notifyAll()等接口。wait()的作用是让当前线程进入等待状态，同时，wait()也会让当前线程释放它所持有的锁。而notify()和notifyAll()的作用，则是唤醒当前对象上的等待线程；notify()是唤醒单个线程，而notifyAll()是唤醒所有的线程。

wait和yield（或sleep）的区别？
> wait()是让线程由“运行状态”进入到“等待(阻塞)状态”，而yield()是让线程由“运行状态”进入到“就绪状态”，从而让其它具有相同优先级的等待线程获取执行权；但是，并不能保证在当前线程调用yield()之后，其它具有相同优先级的线程就一定能获得执行权。

> wait()是会线程释放它所持有对象的同步锁，而yield()方法不会释放锁。


链接：https://www.jianshu.com/p/fd70d652f9e3

****

<h2 id="如何准备自我介绍">
如何准备自我介绍
</h2>

[返回目录](#目录)

1. 将自我介绍控制在200-500字。
> 主要概括一下自己的供职过的公司和参加过的项目经历；如果刚毕业的，可以简单介绍一下自己的专业和兴趣，或之前的实践项目经历。可以找同学或者朋友、同事听听，注意他们的反馈。对于项目经历，可以找一些不懂这个项目的人，尝试给他们用简单的一句话说明白。并且自我介绍也不能一成不变，对不同的公司和职位，要有不同的侧重点。好比我招Android应用开发，肯定希望面试者多讲讲自己在Android开发方面的经验和看法。

2. 不要主动介绍自己不好的地方。
> 就算你很谦虚，也没有必要逢人就说自己的缺点在哪里。任何时候，你都应该表现自己积极主动的一面，你应该做一个传播正能量的人，为什么？因为大多数人都喜欢和正能量的人在一起。遇到太多，所以我更相信概率（很多人都无法胜任）。因为很多人都是“用一个经验工作了很多年”，并不是真有多年工作经验。一个有多年工作经验的人，他有能力让自己在面试时就能达到新平台的开发要求，如果现在不能，那么以后能的可能性并不大。这从一个方面说明了他的理解和学习能力，以及自身的执行力。这三者兼具又有多年工作经验的人，他只需要很短的时间就可以达到一般项目中对Android开发的要求。如果短时间（2到6个月）无法达到，给他一两年，他仍然达不到。

3. 诚实的对待自己。
> 不行的地方就是不行，把自己的预期降低，用几年的时光努力换取一个满意真实地自我介绍。你可能会损失眼前的工作，也可能只能拿到更低的工资。不要觉得几年是很长的时间，这几年的努力对你的长远来说意义非凡。做到这一点，你会更容易明白什么更值钱。

> 总结下来就是，你需要用简洁的语言概括出你工作和项目经历，并侧重于突出和此次面试相关的经历，并且在平时要培训自己成为一个正面、积极的人。

链接：https://www.jianshu.com/p/3ca780defc93

****

<h2 id="如何对SQLite数据库中进行大量的数据插入？">
如何对SQLite数据库中进行大量的数据插入？
</h2>

[返回目录](#目录)

插入的优化可以通过“SQLiteStatement+事务”的方式显著提高效率。
```
直接使用SQL语句进行插入
直接使用SQL语句插入，添加事务
使用ContentValues方式，添加事务
使用SQLiteStatement方式，添加事务
```

> 查询方面的优化一般可以通过建立索引。建立索引会对插入和更新的操作性能产生影响，使用索引需要考虑实际情况进行利弊权衡，对于查询操作量级较大，业务对要求查询要求较高的，还是推荐使用索引。所以这会有一个取舍问题，看你的项目是查询频繁还是插入和修改频繁。当然还有一些小的优化细节，如果面试官问到也可以说几点（如limit）。

>SQLite的同步锁精确到数据库级，粒度比较大，不像别的数据库有表锁，行锁。同一个时间只允许一个连接进行写入操作。

**我们如果开一个工作线程去操作SQLite数据库，如批量地插入可能需要30秒钟，而这个时间UI线程也要从数据库读取一下数据展示给用户，那么这个时候UI线程能读取到这个数据库吗？**
> 关于楼主那个一边插入一边读取显示那个问题，我有如下解决方法，望楼主评价一下如何： 首先先根据项目需求，看这个一边读取的数据量有多少，可以采用预插入一个小数量级的数据，然后再读出来（假设数据获取和UI显示在两个不同的进程），满足UI需求，然后后续再用户无法感知的情况下，再完成后续的大量数据插入。这相当于lazy吧，因为移动端的确资源有限，动不动就想并发不太实际，楼主觉得呢？

链接：https://www.jianshu.com/p/2398aad3bd61

****

<h2 id="Activity的启动模式（launchMode）有哪些，有什么区别？">
Activity的启动模式（launchMode）有哪些，有什么区别？
</h2>

[返回目录](#目录)

> Launcher这个应用的Home界面（一般是指Launcher.java）用的是哪种模试。我自信的回答用singleInstance，要不是面试官早有准备，估计他都要被我的自信弄得要开始怀疑人生。我的相法很简单，认为它全局只有一个实例而且应该只有一个实例，用singleInstance最好。当我回来查询Launcher的源代码时发现使用的是SingleTask模式。

很多人在使用startActivityForResult启动一个Activity时，会发现还没有开始界面跳转本身的onActivityResult马上就被执行了，这是为什么呢？
> 左边第1列代表MainActivity的启动模式，第一行代表SecondActivity（即要startActivityForResult启动的Activity）的启动模式，打叉代表在这种组合下 onActivityResult 会被马上调用，好在幸运的是，Android在5.0及以后的版本修改了这个限制。也就是说上面x的地方全部变成了√，那么在Android 5.0后，还会有这个问题吗？还是会的。如在Intent中设置了FLAG_ACTIVITY_NEW_TASK（intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);）再startActivityForResult，即使是标准的启动模式仍然会有这个问题。

|onActivityResult立即被调用的组合|stand|singleTop| singleTask | singleInstance|
|----|:------:|:----:|:----:|:----:|
|stand|√|√|x| x|
|singleTop|√|√| x|x|
|singleTask|√|√| x |x|
|singleInstance|x|x|x|x|

> 在项目中常遇到一个需求就是在通知栏中使用PendingIntent跳转到相关的Activity。但这个Activity往往是根据通知的内容的具体的Activity，通知来的时候有可能应用已经被KILL掉了，这时跳转这个具体内容的DetailActivity后，我们希望按Back键能回退到应用的主界面（MailActivity），你会怎么做呢？在DetailActivity中onBackPressed做判断？如果没有很好的解决方案的话，大家可以看看：TaskStackBuilder。

> 关于程序员的“35岁坎”（业界就是这样流传的），我们都需要各自好好思考一下。如果一样东西我们学过、使用过，但当我们不在这个行业时，这个当年费尽辛苦学来的东西还能记住什么呢？会不会也只是很少的一部份，只是当年让我们为难的痛点呢？当有一天Android被淘汰了，或者自己转行了，那么从Android这个平台上我们学习到了什么呢？什么是会变的，什么又是不变的呢？我想不变的东西，才是值得我们仔细思考的

链接：https://www.jianshu.com/p/e466b6390a7c
链接：https://www.jianshu.com/p/cb5c4e5598ed

****

<h2 id="Android资源目录的读取顺序？">
Android资源目录的读取顺序？
</h2>

[返回目录](#目录)

![image.png](https://upload-images.jianshu.io/upload_images/4143664-37b06a06b0625135.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 在查找时会先去掉有冲突的资源目录（上图第1步），然后再按MCC、MNC、语言等指定的优先级进行查找，直到确认一个匹配资源。根据屏幕尺寸限定符选择资源时，如果没有更好的匹配资源，则系统将使用专为小于当前屏幕的屏幕而设计的资源。

图片放错目录会产生的问题吗？
把同一个图片资源放在不同的dpi目录，会发现它们使用的内存是不一样的。简单说就是高密度（density）的系统去使用低密度目录下的图片资源时，会将图片长宽自动放大以去适应高密度的精度，当然图片占用的内存会更大。所以如果能提各种dpi的对应资源那是最好，可以达到较好内存使用效果。如果提供的图片资源有限，那么图片资源应该尽量放在高密度文件夹下，这样可以节省图片的内存开支。

drawable文件夹是存放一些xml(如selector)和图片，Android会根据设备的屏幕密度(density)自动去对应的drawable文件夹匹配资源文件。Android对放在mipmap目录的图标会忽略屏幕密度，会去尽量匹配大一点的，然后系统自动对图片进行缩放，从而优化显示和节省资源（使用上面说的mipmap技术）。就目前的版本来说，mipmap也没有完全取代drawable的意思，为了更好的显示效果，官方建议如下类型的图片资源可以放到mipmap目录(Launcher icons、Action bar and tab icons、Notification icons)。

**drawable-nodpi文件夹** 这个文件夹是一个密度无关的文件夹，放在这里的图片系统就不会对它进行自动缩放，原图片是多大就会实际展示多大。但是要注意一个加载的顺序，drawable-nodpi文件夹是在匹配密度文件夹和更高密度文件夹都找不到的情况下才会去这里查找图片的，因此放在drawable-nodpi文件夹里的图片通常情况下不建议再放到别的文件夹里面。

**res/raw和assets的区别**  这两个目录下的文件都会被打包进APK，并且不经过任何的压缩处理。assets与res/raw不同点在于，assets支持任意深度的子目录，这些文件不会生成任何资源ID，只能使用AssetManager按相对的路径读取文件。如需访问原始文件名和文件层次结构，则可以考虑将某些资源保存在assets目录下。

UI设计师对照参考图

|密度|	建议尺寸|
|:----|:----|
| mipmap-mdpi  |	48 * 48|
|  mipmap-hdpi  |	72 * 72  |
|  mipmap-xhdpi  |	96 * 96  |
|  mipmap-xxhdpi  |	144 * 144  |
|  mipmap-xxxhdpi  |	192 * 192  |

面试者分做两类。一类是打下手的开发，做一些功能或业务模块实现，修修BUG保证应用正常运行；另一类是主创型开发，也可以叫主力开发，他们对应用开发的各个模块都有了解，可以熟练地从零开始搭建出整个项目框架，主力选手除了经验的积累外更重要的是思维方式的改变，注重接口的设计胜过功能代码的实现

链接：https://www.jianshu.com/p/46ce37b8553c

****

<h2 id="有没有遇到OOM的问题？如何优化图片占用的内存空间？">
有没有遇到OOM的问题？如何优化图片占用的内存空间？
</h2>

[返回目录](#目录)

1. 3.0版本的还增加了一个inBitmap属性（BitmapFactory.Options.inBitmap）。如果设置了这个属性则会重用这个Bitmap的内存从而提升性能。但是这个重用是有条件的，在Android4.4之前只能重用相同大小的Bitmap，Android4.4+则只要比重用Bitmap小即可
2. 使用采样率（inSampleSize），如果最终要压缩图片，如显示缩列图，我们并不需要加载完整的图片数据，只需要按一定的比例加载即可；使用Matrix变形等，比如使用Matrix进行放大，虽然图像大了，但并没有占用更多的内存。
3. 如果图片很大，比如他们的占用内存算下来就直接OOM了，那么我们肯定不能直接加载它。解决主法还是有很多的，系统也给我们提供了一个类BitmapRegionDecoder，可以用来分块加载图片

链接：https://www.jianshu.com/p/3c597baa39e5

****

<h2 id="Android中Java和JavaScript如何交互？">
Android中Java和JavaScript如何交互？
</h2>

[返回目录](#目录)

> JavaScript 是一种脚本语言，个人认为他比Java更面像对象，它没有编译、链接等操作，在运行时才动态的进行词法、语法分析，生成抽象语法树和字节码，然后由解释器负责执行或者使用 JIT 将字节码转化为机器码再执行。整个流程由 JavaScript 引擎负责完成，在Android手机上，这个JavaScript 引擎就是WebView的实现内核，在Android 4.4版本中，原本基于Android WebKit的WebView实现被换成基于Chromium的WebView实现。

1、Java调用WebView加载的网页上的JavaScript

```
webView.loadUrl(“javascript:methodName(parameterValues)”)//无法接受返回值

Android 4.4之后WebView增加了evaluateJavascript接口可以供我们通过ValueCallback这个回调处理返回值。
webView.evaluateJavascript("getString()", new ValueCallback<String>() {//必须在UI线程（主线程）调用
  @Override
  public void onReceiveValue(String value) {//接受到的返回值
      Log.i(LOGTAG, "onReceiveValue value=" + value);
  }});

```
2、JavaScript调用本地的Java对像方法
```
WebView.getSettings().setJavaScriptEnabled(true);
WebView.addJavascriptInterface(new JsClient(), "JsClientTest");
    public class JsClient {
        public JsClient() {
        }

        @JavascriptInterface
        public void submit(String str) {
            Toast.makeText(HtmlActivity.this, str, Toast.LENGTH_SHORT).show();
            presenter.submit(str);
        }
    }
    //JS的调用
    JsClientTest.submit(jsonStr);//传值
```

React Native
> React Native的目的是构建真正native的应用。而不是构建在Webview里运行的混合模式的应用，开发完全由JavaScript和React来完成。简单说它的原理就是，开发用JavaScript开发Web的方式进行开发，最终在移动端会使用一个JavascriptCore解释器引擎来解析JS相关文件成相应的原生控件再进行渲染，性能上得到了很大的提升,FaceBook开发React的思维是：Learn once，Write anywhere!

链接：https://www.jianshu.com/p/4bb5672ff88d


针对没有工作（或者项目）经验的毕业生来说，比较看重如下几点
1. 沟通和语言表达能力:如果面试官是正常的（不难沟通），那对方能和面试官进行良好不冷场的对话，那么沟通能力就是有保证的,语言的表达能力很重要，它代表你的思维方式和抽象概括的能力。理查德D. 费曼关于知识管理有一个“费曼技巧”：通过向别人清楚地解说一件事，来确认自己真的弄懂了这件事。其实通俗点说，就是你能用自己的话说出来了才说明你会了。而要用自己的话说出来，你的抽象概括就显得尤为重要。
2. 理解和学习能力；
3. 是否极积乐观：极积和乐观的人对工作是很有帮助的，至少和他们在一起工作也会觉得很开心。工作肯定会有压力，乐观的人总是比较容易化解这些压力，不至于压力一来就在烦躁的骂天骂地骂人
4. 对未来的规划：这点应该是很多人的弊端，正如那句话“我们都没有去看过世界，哪来的世界观”。我们总想着，未来我们成为软件行业的高手、大神，追随者无数，人见人爱。但是却忘了，做一个规划。每天都能过且过的上班，然后一直抱怨是公司或者社会没有给自己机会成为大神，所以我认为有一个对自己未来（三到五年）的规划是很重要的，特别是对应届毕业生，你没有过往的经验，那就只能来说说你是如何憧憬未来，最重要的还是要看到你打算如何实现它的方案。这一点还涉及到眼界方面的问题，也许我们现在定的未来很可笑和幼稚，但有规划总比漫无目的的好吧。

****

<h2 id="两个Fragment之间如何进行通信？">
两个Fragment之间如何进行通信？
</h2>

[返回目录](#目录)

> 尽管Fragment是作为独立于Activity的对象实现，并且可在多个Activity内使用，但片段的给定实例会直接绑定到包含它的 Activity。具体地说，片段可以通过getActivity()访问Activity实例。同样地，Activity也可以使用findFragmentById()或findFragmentByTag()，通过从FragmentManager获取对Fragment的引用来调用片段中的方法。知道这些基础，你应该很清楚怎么进行通信了。不过我们建议的方式是面向接口编程，在Fragment中getActivity获取到的Activity实例应该是实现了我们某个接口的实例，即在Fragment的代码中不应该出现某个具体的Activity类

如何管理Fragment回退栈?

FragmentTransaction中remove和detach的区别 ?

****

<h2 id="如何理解Android应用的进程？">
如何理解Android应用的进程？
</h2>

[返回目录](#目录)

> 当系统内存不足时，Android系统会选择终止掉一部份进程，回收其所占用的内存空间。 为了确定保留或终止哪些进程，系统会根据进程中正在运行的组件以及这些组件的状态，将每个进程放入“重要性层次结构”中。 必要时，系统会首先消除重要性最低的进程，然后是重要性略逊的进程，依此类推，以回收系统资源。重要性从高到低:前台进程->可见进程->服务进程->后台进程->空进程；

对于一般提高进程优先级的方法
1. 进程要运行一些组件，不要成为空进程。
2. 远行一个Service，并设置为前台运行方式（startForeground）。
```
private void keepAlive() {
        try {
            Notification notification = new Notification();
            notification.flags |= Notification.FLAG_NO_CLEAR;
            notification.flags |= Notification.FLAG_ONGOING_EVENT;
            startForeground(0, notification); // 设置为前台服务避免kill，Android4.3及以上需要设置id为0时通知栏才不显示该通知；
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }
```
3. AndroidManifest.xml中配置persistent属性（persistent的App会被优先照顾，进程优先级设置为PERSISTENT_PROC_ADJ=-12）

> 线程是CPU调度的基本单元，一个应用都有一个主线程负责处理消息。一个应用启动后，至少会有3个线程，一个主线程（UI线程）和2个Binder线程。Zygote进程（APK所在的进程也是由Zygote进程Fork出来的）还会产生有一些Daemon线程如：ReferenceQueueDaemon、FinalizerDaemon、FinalizerWatchdogDaemon、HeapTaskDaemon

![image.png](https://upload-images.jianshu.io/upload_images/4143664-85960f850b74268e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**线程安全** 就是多线程访问时，采用了加锁机制，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染。(Vector,HashTab;le)
**线程不安全** 就是不提供数据访问保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据。（ArrayList，LinkedList，HashMap等）

链接：https://www.jianshu.com/p/999650fda571

****

<h2 id="如何解决ScrollView嵌套中一个ListView的滑动冲突？">
如何解决ScrollView嵌套中一个ListView的滑动冲突？
</h2>

[返回目录](#目录)

解决ListView只显示一个Item和解决ListView与ScrollView的滑动冲突问题的代码？
```
    /**
     * 重写ListView的onMeasure方法，达到使ListView适应ScrollView的效果，解决ListView只显示一个Item
     */
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2,
        MeasureSpec.AT_MOST);
        super.onMeasure(widthMeasureSpec, expandSpec);
    }
    @Override//重写ListView的onInterceptTouchEvent方法，解决ListView与ScrollView的滑动冲突问题
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        switch (ev.getAction()) {
            case MotionEvent.ACTION_DOWN:
            case MotionEvent.ACTION_MOVE:
                getParent().requestDisallowInterceptTouchEvent(true);
                break;
            case MotionEvent.ACTION_UP:
            case MotionEvent.ACTION_CANCEL:
                getParent().requestDisallowInterceptTouchEvent(false);
                break;
        }
        return super.onInterceptTouchEvent(ev);
    }

```
ScrollView和ListView都是上下滑动的，嵌套在一起后ScrollView中的ListView就没法上下滑动了，事件被ScrollView响应了。


事件的分发中我们较关注的三个方法：

**分发事件：dispatchTouchEvent**在这里进行事件的分发，onInterceptTouchEvent和onTouchEvent都是由dispatchTouchEvent负责调度的。

**拦截事件：onInterceptTouchEvent**只有ViewGroup才有这个方法。拦截了的话，ViewGroup就不会把事件继续分发给子View了，即子View的dispatchTouchEvent和onTouchEvent这两个方法都不会被调用。返回true时，表示ViewGroup会拦截事件。

**消费事件：onTouchEvent**onTouchEvent 返回true时，表示事件被消费掉了。一旦事件被消费掉了，其他父元素的onTouchEvent方法都不会被调用。

![image.png](https://upload-images.jianshu.io/upload_images/4143664-501daf8083e483a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

链接：https://www.jianshu.com/p/9abf6a874feb

****

<h2 id="知道什么是ART吗？它和Dalvik有什么区别？">
知道什么是ART吗？它和Dalvik有什么区别？
</h2>

[返回目录](#目录)

**什么是Dalvik：** Dalvik是Google公司自己设计用于Android平台的Java虚拟机。Dalvik虚拟机是Google等厂商合作开发的Android移动设备平台的核心组成部分之一，它可以支持已转换为.dex(即Dalvik Executable)格式的Java应用程序的运行，.dex格式是专为Dalvik应用设计的一种压缩格式，适合内存和处理器速度有限的系统。Dalvik经过优化，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个Dalvik应用作为独立的Linux进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。

**什么是ART:** Android操作系统已经成熟，Google的Android团队开始将注意力转向一些底层组件，其中之一是负责应用程序运行的Dalvik运行时。Google开发者已经花了两年时间开发更快执行效率更高更省电的替代ART运行时。ART代表Android Runtime,其处理应用程序执行的方式完全不同于Dalvik，Dalvik是依靠一个Just-In-Time(JIT)编译器去解释字节码。开发者编译后的应用代码需要通过一个解释器在用户的设备上运行，这一机制并不高效，但让应用能更容易在不同硬件和架构上运行。ART则完全改变了这套做法，在应用安装的时候就预编译字节码到机器语言，这一机制叫Ahead-Of-Time(AOT)编译。在移除解释代码这一过程后，应用程序执行将更有效率，启动更快。

ART优点：
1. 系统性能的显著提升
2. 应用启动更快、运行更快、体验更流畅、触感反馈更及时
3. 更长的电池续航能力
4. 支持更低的硬件

ART缺点：
1. 更大的存储空间占用，可能会增加10%-20%
2. 更长的应用安装时间

链接：https://www.jianshu.com/p/fda3fa6ec4c4

****

<h2 id="如何检测内存泄露，如何进行内存优化？">
如何检测内存泄露，如何进行内存优化？
</h2>

[返回目录](#目录)

如何申请更多的内存？
> 因为APK应用的Java进程也是运行在Native进程之上的，而Native的内存空间是不受Dalvik虚拟机的heapsize限制，相对而言大很多（Java和Native可以通过JNI进行通信），因此我们可以用C/C++实现大内存的消耗

Android系统是基于Linux的，但Android中的进程分为两种，一种是Native进程，一种是Java进程。我们常接触的APK应用都是运行在Java进程中的。Android应用运行于Dalvik虚拟机之上的进程，由Dalvik虚拟机负责分配和管理应用的Java对象需要的内存。而对应到内存上，就存在Natvie内存和Java内存，由Dalvik虚拟机管理的就是Java内存（或者叫Java Heap内存）。
> Native进程：采用C/C++实现，不包含Dalvik实例的进程，/system/bin/目录下面的程序文件运行后都是以native 进程形式存在的，/system/bin/surfaceflinger、/system/bin/rild、procrank等就是Native进程。

链接：http://chuanqiljp.top/2018/03/12/Android%E7%9A%84%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96/
链接：https://www.jianshu.com/p/26d37babcb50
链接：https://www.jianshu.com/p/e86c58ba9343

****

<h2 id="如何准备和Boss（或经理）的面试">
如何准备和Boss（或经理）的面试
</h2>

[返回目录](#目录)



前几天和之前呆过的一家公司的同事（称他为T吧）一起吃饭，T最近准备换工作，已经过了移动下属的一家子公司的技术面试，第二天公司的负责人（应该是总经理）要再面试一下他，想听听我的看法和建议。

我觉得很简单，从专业上讲，先看你到这家公司要做的事情，你的专业技能是不是行业稀缺的？如果是（正好T就是）那么没有什么好担心的，Boss不会为难你的，多半就看一下你长什么样就OK了。

有经验的面试官都明白，很多时候招聘一个工程师是基于他编码之外的能力，也就是说往往决定你能否入职的关键并不一定是技术因素。所以一般经理级别的人物多半不会和你谈技术或者什么未来的趋势走向，而是如家常般随便聊聊。而且不是说Boss都五十多了吗？那更不会问你什么了，最多聊聊“人生和理想”。因为，Boss和我们不是在同一个级别上的人，其实并没有多少问题可以谈的。

**不过可不要小看“随便聊聊”，这个聊聊可以很容易看出你的思维能力和对事物的看法，而且这些方面是你短期很难改变的特质。**
根据T的情况，我给他总结一下：

1. 技能上是行业稀缺，并不好招到有这个专业经验的工程师，因为稀缺所以薪资也可以在自己的心理价位上再提高一些（一定要听到对方说不行，这个价太高了，不然你就叫低了）；
2. 应聘的职位只是高级工程师，并没有管理和技术负责人的角色，所以对方并不一定会太在意你的管理和领导力等一些编码之外的软实力，所以后面的面试大可以轻松应对；
3. 同事内部推荐的职位是很有优势的，如知道对方在扩展业务，需要更多的工程师，其实主动权在你的手里。
所以，和经理的面试走过场的可能性比较大。而且容我“恶意”地想一下，可能就是经理为了体现自己的存在感，所以不管招什么职位的人（哪怕他完全不懂的职位）都得经过他点头认可。

第二天T告诉我果真是这样：老板就问了一下他是否结婚、什么时候到岗之类的问题。

不过，并不是每个公司都是这样的情景，那么对于要接受经理面试的开发人员，还是给一些我认为比较适合的建议：

1. 临时准备基本上来不急，除非是外企的话要准备一段英文的自我介绍之类的；
2. 但还是要准备一下，了解一些公司的情况，态度上要不卑不亢；
3. 最好的方式是想办法做一场“预面试”，即是在经理面试你前就有机会和经理碰面或者通电话聊聊（当然要有合适的机缘，不然可能适得其反）；
**预面试**

这是很有用的一招，不过一般需要长时间的准备，临时起意去的公司一段比较难有预面试的机会。什么叫预面试呢？就是在正式的面试前，可以和面试官进行一定的交流，让面试官对你有一个感性（最好是好感）的认识。等正式面试时，面试官不由自动地在主观意识上就会偏向于你！
有一个事例：一个朋友为了进一家自已向往的公司，前半年开始就到处去打听这家公司的工程师的博客和微搏，然后只要对方发搏客（技术类的）他就去进行专业的评论（不懂就先Google，返正最后的跟帖要专业），最后这几个工程师都认识他了，还返过来也看他的搏客并和他形成了良好的互动关系。结果不说你也能猜到，只要这个公司有合适他的职位，面试基本上只是走过场。
最后还是那句话“机会都留给有准备的人”。现在开始，你至少得准备一个搏客写写文章吧！

****

<h2 id="你在Android开发中遇到的技术难题是什么，你是怎么解决的？">
你在Android开发中遇到的技术难题是什么，你是怎么解决的？
</h2>

[返回目录](#目录)

一个工作过几年的程序员肯定会有工作中遇到技术难点问题，虽然这个问题有可能对于别人不是技术难点，但只要对于当时的你是技术难点，只要让你抓耳挠腮毫无头绪就往往会在你的大脑中留下深刻的印象。
其实很多人的问题在于“他没有问题”！很多工作了多年的程序员，你问他有什么印象深刻的问题困扰过自己吗？没有！有在参与过的项目中有提供什么非常关键的解决方案吗？没有！有发现过测试没测出来但很严重的问题吗？没有！太多的“没有”了，这样的人不在少数，包括工作很多年的资深程序员。那么真的是我们没有遇到过问题吗？答案显然不是，问题是我们遇到问题时没有对问题进行过深入的思考，没有把它记录或者当成案例分享给别人，渐渐地你自然会遗忘它们，然后你还有可能遇到同一个（或相似的）问题又被它再次弄得焦头烂额。努力吧，做个有“问题”的程序员。

链接：https://www.jianshu.com/p/69d9444e2a9a

****

<h2 id="谈谈你使用过的Android开源库，是否有遇到过什么问题？">
谈谈你使用过的Android开源库，是否有遇到过什么问题？
</h2>

[返回目录](#目录)

其实很多程序员的问题是很少使用开源库或者框架，除了图片加载库是必备之外，其他的开源库很多人就不一定会使用了，自然也不会遇到什么问题。为什么我会比较看重这一点呢？因为，我觉得第三方的开源库，你不一定要在自己的项目中使用，但你一定要去学习别人是怎么写这个库的，是不是比你的方法更好，而且很多流行的开源库都是一些Android大牛写的，直接读和修改他们的代码是你接近他们最简单的方式。
可不可以在使用第三方开源库时，就是没有问题呢？当然可以，不过你可以聊一聊你为什么要选择这个库，其实就是从侧面来说明：你对这个库的优缺点的看法，以及你是否了解它的实现原理，因为你要对自己的项目负责（开源作者可不需要）

链接：https://www.jianshu.com/p/571d9a4e51cf

****

<h2 id="谈谈MVP和MVVM模式，你有在自己的项目中使用过吗？">
谈谈MVP和MVVM模式，你有在自己的项目中使用过吗？
</h2>

[返回目录](#目录)

1. **MVC** 全名是Model--View--Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。其中Model层处理数据，业务逻辑等；View层处理界面的显示结果；Controller层起到桥梁的作用，来控制View层和Model层通信以此来达到分离视图显示和业务逻辑层。
2. **MVP** 其实是MVC的一种演进版本，它更简单，将MVC中的Controller改为了Presenter，View通过接口与Presenter进行交互，降低耦合，方便进行单元测试,最明显的区别就是，MVC中是允许Model和View进行交互的，而MVP中很明显，Model与View之间的交互由Presenter完成。
```
View：负责绘制UI元素、与用户进行交互(Activity、View、Fragment都可以做为View层)；
Model：对数据的操作、对网络等的操作，和业务相关的逻辑处理；
Presenter：作为View与Model交互的中间纽带，处理与用户交互的逻辑。可以把Presenter理解为一个中间层的角色，它接受Model层的数据，并且处理之后传递给View层，还需要处理View层的用户交互等操作。
```
![image.png](https://upload-images.jianshu.io/upload_images/4143664-3162a4b311ef7081.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. **MVVM模式** （Model--View--ViewModel模式），和MVP模式相比，MVVM 模式用ViewModel替换了Presenter ，其他层基本上与 MVP 模式一致，ViewModel可以理解成是View的数据模型和Presenter的合体。MVVM采用双向绑定（data-binding）：View的变动，自动反映在ViewModel，反之亦然，这种模式实际上是框架替应用开发者做了一些工作（相当于ViewModel类是由库帮我们生成的），开发者只需要较少的代码就能实现比较复杂的交互
![image.png](https://upload-images.jianshu.io/upload_images/4143664-ebdcf46e0dd75c14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 你的项目并不一定非要用MVP/MVVM或者别的什么模式，模式都有它们自身应用的场景，有好处也就会有坏处。但了解是必需的，不然你很难扩展自己的技能范围，高级的开发应该学会判断某个项目是否合适一些现成的模式，合理的使用模式会让你的应用框架更加清晰并且易于维护和扩展

链接：https://www.jianshu.com/p/b7fb7f502ea5

****

<h2 id="如何快速突击Android面试">
如何快速突击Android面试
</h2>

[返回目录](#目录)

**Android面试的技术题准备:**
> 这个世界有一个“二八原则”在好多地方都发挥着作用，在Android开发上我认为也一样有用。做一个Android开发，你也许只会用到Android开发知识中的20%，有80%其实你学了也不一定会用。而面试官也一样，他也可能只掌握了20%的知识，而且一个面试也不会有足够多的时间给你展示你全部的知识，而往往只会注意开发中最常遇到的20%。有些公司会例外，他们可能会比较注重一些诸如算法的问题，而直接忽视Android的技能题。但大体上来说，那20%比较重要的知识点，一般是你要重视和答对的。比如：Android的四大组件，每个组件的特点、作用；Handler的机制；异步任务处理；

**Android面试的项目题准备:**
> 其实最重要做判断的根据还是你的项目经验。所以对于你从事过的项目及你在这些项目中的职责和作用，你应该有一个清晰的描叙。对于项目经验丰富但是项目的类型单一的人，如项目中清一色的“资讯”类应用，那么你应该表现你具备独立开发和处理各方面问题的能力，而且最好在平时你就要有意识的避免进入到这种境地当中。对于“一个经验用十年”的人，面试官其实也很难分辨出他在其他的方面是否也能做得一样好，如果你不能在公司层面避免陷入到这种情况，那么你还是应该尝试同一个项目中的不同方面，或者自己做一些和当前公司不相关的项目、开源库等。但其实有很多人的问题在于，项目经验并不丰富，而且有些人工作了很多年，但有可能其中的几年都在维护一个项目，简历上往往用一句话就把这几年的事情说完了。但我认为，并不是我们在这几年中没有做什么有价值的事，而是我们没有把这些事情记录和总结，并做一个深入的思考和扩展。想想吧，总会有的，把事情想到了还要对这个主题做一下扩展，你总结出来的东西才更有深意。

**个人问题的准备:**
> 对于面试者来说，往往觉得面试就是回答对面试官的问题，但从面试官的角度来看，面试其实就是要做一件事情：“如何区分面试者”。简单的说，就是把你和面试官面过的（或即将面试的）的人区分开来，并给你打上几个签标，简单点可以是“不错”、“合适”、“犹豫”、“肯定不行”。复杂点的，可能会把你的某些能力列出来，比如学习能力强、协作能力差，然后再和其他人放在一起综合考虑。每个项目都有不同的特点，所以每次的侧重能力考察也会不一样。所以，有时候你通过了一家公司的面试，也不需要太得意了，可能并不是你有多厉害，仅仅只是你正好是这个时间段里性价比较高的那个。当然，如果你被淘汰了，也不需要妄自菲薄，也许只是因为在这个时间段有个比你更高性价比的人也来这家公司面试了。那么，个人如何准备自己可能会遇到的诸如“说说你的优缺点”、“你最擅长什么”、“你在项目提供的最有价值的作用是什么”等等这类问题？其实，返过来看就很简单了，这些问题归根到底就是“你和别人的区别在哪里”。面试官的任务是要把你和别人做区别，你自己也需要把自己和别人做区别，回答“不知道、好像没什么”这样的话，其本上会给减分。每个人都有和别人不一样的地方，在面试前想想一些正面的积极的地方，然后自己总结一下，最好给你周围的同事、朋友说一下，看他们是否认同你的看法。最后你会发现给别人说事情时，最好的方式是说一些案例故事，虽然你要说的可能只是一个简单的点（比如你抗压能力强），但你也可以用讲故事的方式讲出来（在某次事件中你在怎样的压力下完成工作的）

链接：https://www.jianshu.com/p/c9091b3efb1d

****

<h2 id="介绍一下你经常浏览的Android技术网站">
介绍一下你经常浏览的Android技术网站
</h2>

[返回目录](#目录)

> 你以前常上（喜欢）什么网站，现在常上什么网站，为什么你会有这个转变？”根据面试者的回答，你可以从侧面了解他目前处于一个什么样的阶段。如果一个开发没有常浏览的网站，其实你也很容易定义他为“不爱学习，对Android的热情、兴趣不大”，意外的情况应该很少。

1. [Android Developer](https://developer.android.com/index.html):有什么网站能比官方更全面和权威呢。而且Android Developer站点上还有很多指导性的文章写得很不错，也不用担心英文不好，因为基本上官方都翻译了中文版本
2. [android-developers的blog](https://android-developers.googleblog.com/)
3. [Stackoverflow](http://stackoverflow.com/):tackoverflow是一个技术在线问答网站，几乎平常遇到的所有技术网站，在这里都能找到答案，上面有很多大牛会很热心回答
4. [Github](http://www.github.com):GitHub是一个通过Git进行版本控制的软件源代码托管服务，目前各种Android的开源项目基本上都放在Github上进行托管
5. [CodeKK](http://p.codekk.com/): 注于开源分享、源码解析、框架设计、好文推荐、Android内推，不知道要用什么框架时，看看CodeKK说不定会有意想不到的收获
6. [android-arsenal](https://android-arsenal.com/):从 2014 年开始做，囊括库最多的网站了，支持英文搜索、分类选择、显示最新开源项目
7. [Android平台源码查看](http://androidxref.com/):基于OpenGrok构建，可以快带检索Android源码的网站，通过方法名或者日志检索网站的速度非常快
8. [grepcode](http://www.grepcode.com/):可快速搜索 Android、Java任何版本及部分开源项目源码，看各版本系统代码排查问题相当方便
9. [大牛的网站Jakewharton](http://jakewharton.com/):Github：https://github.com/JakeWharton,所在公司：Square Open Source  网址：http://square.github.io/
10. [Realm官网](https://realm.io/):Realm开源库的官网也很不错，经常在它的news版块能看到一些高质量的文章或者视频演讲

链接：https://www.jianshu.com/p/bcfb00d7f491

****

<h2 id="Binder是什么？它是如何实现跨进程通信的？">
Binder是什么？它是如何实现跨进程通信的？
</h2>

[返回目录](#目录)

Binder通信的四个角色(可以把四个角色和熟悉的互联网进行类比：Server是服务器，Client是客户终端，ServiceManager是域名服务器（DNS），驱动是路由器)：
- Client进程：使用服务的进程。
- Server进程：提供服务的进程。
- ServiceManager进程：ServiceManager的作用是将字符形式的Binder名字转化成Client中对该Binder的引用，使得Client能够通过Binder名字获得对Server中Binder实体的引用。
- Binder驱动：驱动负责进程之间Binder通信的建立，Binder在进程之间的传递，Binder引用计数管理，数据包在进程之间的传递和交互等一系列底层支持。

**Binder的运行机制：** Server进程向Service Manager进程注册服务（可访问的方法接口），Client进程通过Binder驱动可以访问到Server进程提供的服务。Binder驱动管理着Binder之间的数据传递，这个数据的具体格式由Binder协议定义（可以类比为网络传输的TCP协议）。并且Binder驱动持有每个Server在内核中的Binder实体，并给Client进程提供Binder的引用。Binder跨进程传输并不是真的把一个对象传输到了另外一个进程；传输过程好像是Binder跨进程穿越的时候，它在一个进程留下了一个真身，在另外一个进程幻化出一个影子（这个影子可以很多个）；Client进程的操作其实是对于影子的操作，影子利用Binder驱动最终让真身完成操作。

**Binder的线程管理** 一个进程的Binder线程数默认最大是16，超过的请求会被阻塞等待空闲的Binder线程。理解这一点的话，你做进程间通信时处理并发问题就会有一个底，比如使用ContentProvider时（又一个使用Binder机制的组件），你就很清楚它的CRUD（创建、检索、更新和删除）方法只能同时有16个线程在跑

**AIDL是什么？你有使用过它吗，它支持哪些数据类型？**
> AIDL是Android Interface Definition Language的简写，即Android接口定义语言。我们知道Android系统为每一个应用开启一个独立的虚拟机，每个应用都运行在各自进程里（默认情况下），彼此之间相互独立，无法共享内存。当一个应用想要访问另一个应用的数据或调用其方法，就要用到Android系统提供的IPC机制。而AIDL就是Android实现IPC机制的方式之一。除了AIDL，Android还提供了Messenger来实现跨进程通信，不过Messenger是以单线程串行方式（消息队列）来处理来自不同客户端的访问的，并不适合多线程并发访问。当需要提供跨进程以及多线程并发服务时就需要AIDL上场了。

**AIDL中的接口函数有时会使用in、out或者inout的参数修饰符，它们各表示什么意思？在什么情况下要使用呢？**
> AIDL中的定向 tag 表示了在跨进程通信中数据的流向，其中 in 表示数据只能由客户端流向服务端， out 表示数据只能由服务端流向客户端，而 inout 则表示数据可在服务端与客户端之间双向流通。其中，数据流向是针对在客户端中的那个传入方法的对象而言的。in 为定向 tag 的话表现为服务端将会接收到一个那个对象的完整数据，但是客户端的那个对象不会因为服务端对传参的修改而发生变动；out 的话表现为服务端将会接收到那个对象的参数为空的对象，但是在服务端对接收到的空对象有任何修改之后客户端将会同步变动；inout 为定向 tag 的情况下，服务端将会接收到客户端传来对象的完整信息，并且客户端将会同步服务端对该对象的任何变动。

链接：https://www.jianshu.com/p/c7bcb4c96b38
链接：https://www.jianshu.com/p/467016b4487c

****

<h2 id="一套高级工程师的面试题">
一套高级工程师的面试题
</h2>

[返回目录](#目录)

**基础原理和运行机制**
1.Handler的运行机制，线程间是如何通信的？
参考：[Android面试一天一题（8 Day）](https://www.jianshu.com/p/108db0240a34)

2.说说AsyncTask原理。
参考：[Android面试一天一题（13 Day: AsyncTask）](https://www.jianshu.com/p/c925b3ea1444)

3.说说Android事件分发机制，ViewGroup和View上的分发有什么不同？
参考：[Android面试一天一题（Day 26：ScrollView嵌套ListView的事件冲突）](https://www.jianshu.com/p/9abf6a874feb)

4.说说Activity生命周期？可能会给一些情景：如Activity A启动Activity B ，A与B各自的生命周期流程（包括A被全覆盖，和半覆盖的区别）；或者具体问Activity在屏幕旋转时的生命周期。
参考：[Android面试一天一题（3 Day）](https://www.jianshu.com/p/ae6e1d93cc8e)

5.Activity的启动模式：Activity的几种LaunchMode及使用场景。
参考：[Android面试一天一题（Day 19：程序员何苦为难程序员（上））](https://www.jianshu.com/p/cb5c4e5598ed) 和 [Android面试一天一题（Day 20：程序员何苦为难程序员（下））](https://www.jianshu.com/p/e466b6390a7c)

6.Android中跨进程通讯有几种方式？
参考：[Android面试一天一题（Day 35：神秘的Binder机制）](https://www.jianshu.com/p/c7bcb4c96b38)

**开发经验**

1.两个Activity之间怎么传递数据（Serializable和Parcelable）?
参考：[Android面试一天一题（9 Day）](https://www.jianshu.com/p/be593134eeae)

2.有遇到过哪些屏幕和资源适配问题？
参考：[Android面试一天一题（Day 21：res目录－细节处见真章）](https://www.jianshu.com/p/46ce37b8553c)

3.AIDL的全称是什么？如何工作？能处理哪些类型的数据？
参考：[Android面试一天一题（Day 36：AIDL）](https://www.jianshu.com/p/467016b4487c)

4.图片的处理和优化, 图片圆角处理的方式有哪几种？
参考：[Android面试一天一题（Day 22: 图片到底是什么）](https://www.jianshu.com/p/3c597baa39e5) 和 [Android面试一天一题（Day 30：老外的自定义View面试题）](https://www.jianshu.com/p/96b9f38319c1)

5.讲讲ListView容易引起性能问题的地方，再说一下你有什么优化方案。

6.内存泄露哪几种情况？如何处理？有使用过什么相关的检测工具吗？
参考：[http://www.jianshu.com/p/26d37babcb50](https://www.jianshu.com/p/26d37babcb50) 和 [Android面试一天一题（Day 29：内存泥潭（下））](https://www.jianshu.com/p/e86c58ba9343)

**框架与模式**

1.常用哪些开源项目，说说最熟悉的一个？
参考：[Android面试一天一题（Day 32：谈谈使用过的第三方开源库）](https://www.jianshu.com/p/571d9a4e51cf)

2.说说MVP，MVC，MVVM架构的不同。
参考：[Android面试一天一题（Day 33：Android开发的套路MVP & MVVM）](https://www.jianshu.com/p/b7fb7f502ea5)

3.你常用的设计模式有哪些？

**专业领域**

1.LBS应用：使用百度或高德地图的SDK遇到过的问题（国内外限制、精确度等）？

2.插件和热修复的原理：插件和热修复框架的原理，模块化开发方式，64K的方法数限问题。

3.消息推送：国内各个消息推送的使用问题？如何保持Service长活？Http长链接实现等。

4.视频直播。

**项目经验和职业规划**

1.说说你的亮点，最值得分享的。
参考：[Android面试一天一题（吹牛题）](https://www.jianshu.com/p/a354af5cd6d1)

2.项目中遇到哪些难题，最终你是如何解决的？
参考：[Android面试一天一题（Day 31：Android技术难题解决方案）](https://www.jianshu.com/p/69d9444e2a9a)

3.你对未来三到五年的职业规划。
参考：我在简书上的[“职业生涯”](https://www.jianshu.com/nb/7675514)专题。


链接：https://www.jianshu.com/p/1d3a2227fb72

****

<h2 id="达到如下要求的简历可以认为是好的简历。">
达到如下要求的简历可以认为是好的简历。
</h2>

[返回目录](#目录)

简历的本质只有一个：向别人说明：你是谁？你擅长做什么？

1. 完整记录自己的学历、工作经历、项目经历；
2. 在简历中有7到10个相关的专业关键词；（如MediaPlayer库、XX库，XX框架、XX算法等，根自己的专业来定）
3. 每个项目中写明白自己的责任和表现；
4. 找出1到2项自己最熟悉的项目重点描述；
5. 记录1到2件自己的光辉事件；（当然，要和主题相关）
6. 简历上写的语句都是可以流畅通顺地大声朗读出来的；

有利于写出好简历的方法
> 定位清楚自己和自己的能力（如果自己做不到那么就找朋友帮忙），有案例的描述比直接陈述更有说服力；量化你所谓的“好”的标准；注意细节问题。

链接：https://www.jianshu.com/p/e0876b27f16e

****

<h2 id="你有写博客或者其他的输出吗？如果有，谈谈你的经历或者看法。">
你有写博客或者其他的输出吗？如果有，谈谈你的经历或者看法。
</h2>

[返回目录](#目录)

> 写得再好的简历，你也只能从中抽像出几个关键词，如“姓名、性别、学历、工作年限，熟练技术，项目复杂度，责任”，获得这些关键词后会预先给面试者一个定性（假设），可能是高级开发、一般的角色或者厉害的人，然后制定不同的面试题或者调整问话模式。从这个方面来说，简历只要提供了必要的关键词并没有好坏之分。通过筛选获得面试机会的面试者，接下来的面试往往是对关键词进行一个验证，如技术和项目方面是否符合预期

当你看完一本书后，往往认为自己懂了，学会了蛮多东西的，但只要让你写读后感或者分享给另一个人，你立刻就发现自己不只是一知半解，可能很多地方说出来都还有逻辑问题。没有输出的话，你并不是真懂，只是你强大的大脑进行了“脑补”让你觉得自己明白了，可以愉快地做其他事情了。而经常写作或者做分享输出的人，他们的大脑和语言逻辑会一直接受锻炼，自然说话条理会更简洁清晰，而且带有很强的逻辑性。有经验的面试官是很容易判断的

写作其实和沟通是一样的，不是为了给自己写，更重要的是要为读者写。学会从别人的视角来看待和理解问题，才能进行有效的沟通。

链接：https://www.jianshu.com/p/03a088b56753

****

<h2 id="如何系统学习Android开发？">
如何系统学习Android开发？
</h2>

[返回目录](#目录)

> 新手往往都不是特别想要学习，也不知道自己的行为是对是错，只是想实现一个立竿见影的目标。如果给新手提供一个与情境无关的规则（或者叫指命）让他们去执行，他们就会变得能干起来。新手往往不把自己看做系统的一部份，所以学习Android开发也变成了一项孤立的事件，学习的模块也变得孤立起来。虽然花时间学习了很多组件或者技巧，但是却没有理清过这些模块或问题之间的关联。

> 专家更关注情境，更关注系统，而且喜欢说“具体情况具体分析”。他们往往更关注事物之间的联系，把自己看做系统的一部份，能分清和认识到系统的边界。

我认为比较好的方式：完整地看完和练习官方指导文档。网上有太多Android开发的视频和文章，但他们都过于碎片化，只有这份官方文当是我认为最系统介绍Android开发的指南。网址：https://developer.android.com/develop/index.html ,一定要把官方文档中的“培训”和“API指南”认真的看一篇，而且花时间把相关的知识联系起来。

链接：https://www.jianshu.com/p/e883a43a5360

****

<h2 id="你有使用过Kotlin来开发Android应用吗？说说Kotlin和Java有什么区别？">
你有使用过Kotlin来开发Android应用吗？说说Kotlin和Java有什么区别？
</h2>

[返回目录](#目录)

官方网站：https://kotlinlang.org/docs/reference/
中文翻译：http://www.kotlincn.net/

- **Kotlin更安全：** 如空引用由类型系统控制，你不会再遇到NullPointerException。
- **简洁，可靠，有趣：** 如你可以用Lambda 表达式；可以减少了很多模版代码；我们的演示视频中就不需要findViewById。
- 函数式支持；
- **扩展函数：** Kotlin同C#类似，能够扩展一个类的新功能而无需继承该类或使用像装饰者这样的任何类型的设计模式。Kotlin支持扩展函数和扩展属性。
- Kotlin中没有静态成员；
- Kotlin可与Java进行100％的互操作，允许在Kotlin应用程序中使用所有现有的 Android 库 ，Kotlin的标准库更多的是对Java库的扩展。

链接：https://www.jianshu.com/p/08d709165176

****

<h2 id="你是如何解决Android的布局嵌套问题的？">
你是如何解决Android的布局嵌套问题的？
</h2>

[返回目录](#目录)

1. **merge** :merge标签的作用是合并UI布局，使用该标签能降低UI布局的嵌套层次。merge标签可用于两种情况：
- 布局顶结点是FrameLayout且不需要设置background或padding等属性，可以用merge代替，因为Activity内容试图的parent view就是个FrameLayout，所以可以用merge消除只剩一个。
- 某布局作为子布局被其他布局include时，使用merge当作该布局的顶节点，这样在被引入时顶结点会自动被忽略，而将其子节点全部合并到主布局中。
2. **ViewStub** :ViewStub标签引入的布局默认不会inflate，既不会显示也不会占用位置。 ViewStub常用来引入那些默认不会显示，只在特殊情况下显示的布局，如数据加载进度布局、出错提示布局等。
需要在使用时手动inflate:
```
ViewStub stub = (ViewStub)findViewById(R.id.error_layout);
errorView = stub.inflate();
errorView.setVisibility(View.VISIBLE);
```
ViewStub在一定的程度可以起到减少嵌套层次的作用，特别是很多时候我们的程序可能不需要走到ViewStub的界面。
3. **include** ：将可复用的组件抽取出来并通过include标签使用，但<include>标签能减少布局的层次吗？我认为不能。include主要解决的是相同布局的复用问题，它并不能减少布局的层次。
4. **用RelativeLayout代替LinearLayout**：很多人为了减少布局层次喜欢用RelativeLayout代替LinearLayout，不过可能达到的效果并不会很明显。层次是减少了，但本身RelativeLayout就会比LinearLayout性能差一点。有一些界面，比如一个图片和一个文本的布局（ListItem常见的布局方式），可以利用TextView有drawableLeft, drawableRight等属性，完全不需要RelativeLayout或者LinearLayout布局。
5. **ConstraintLayout** ：ConstraintLayout即约束布局，在2016年由Google I/O推出。ConstraintLayout和RelativeLayout有点类似，控件之间根据依赖关系而存在，但比RelativeLayout更加灵活。创建大型复杂的布局仍然可以使用扁平的层级(不用嵌套View Group)，说的简单些就是，再复杂的界面也可以只有2层层次。要使用ConstraintLayout需要在build.gradle中添加相关的support库，
6. **FlexBoxLayout** ：做过前端开发（CSS方面）的同学对FlexBox一定不会陌生，最近我在做微信小程序开发时也涉及到FlexBox。FlexBox(弹性布局)是w3c在2009年提出的一种新的布局方案，解决以前那种传统css的盒模型的局限性。Google开源了FlexboxLayout布局和前端CSS FlexBox布局具有相同的功能（肯定有不一样的地方），但已经足够在Android上改进布局的构建方式。FlexBoxLayout可以理解成一种更高级的LinearLayout，不过比LinearLayout更加强大和灵活。如果我们使用LinearLayout布局的话，那么不同的分辨率，也许我们要重新调整布局，势必会需要跟多的布局文件放在不同的资源目录。而使用FlexBoxLayout来布局的话，它可以适应各种界面的改变（所以叫响应式布局）。

链接：https://www.jianshu.com/p/c8d4a3d7fd26


****

<h2 id="设计模式">
设计模式
</h2>

[返回目录](#目录)

面试题: 回调函数和观察者模式的区别？
> “标准答案”：观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。观察者模式完美的将观察者和被观察的对象分离开，一个对象的状态发生变化时，所有依赖于它的对象都得到通知并自动刷新。回调函数其实也算是一种观察者模式的实现方式，回调函数实现的观察者和被观察者往往是一对一的依赖关系。所以最明显的区别是观察者模式是一种设计思路，而回调函数式一种具体的实现方式；另一明显区别是一对多还是多对多的依赖关系方面。

Android的单列模式如何保证一定单列的情况？
```
public class Singleton{
    private volatile static Singleton instance;
    private Singleton() {};
    public static Singleton getInstance() {
        if (instance==null) {
            synchronized(Singleton.class) {
                if (instance==null)
                    instance=new Singleton();
            }
        }
        return instance;
    }
}
```

Android较常用到的设计模式？
- 适配器模式：GridView、ListView的Adapter;
- 建造者模式：AlertDialog.Builder;
- 观察者模式：ListView的adapter.notifyDataSetChanged;
- 责任链模式：View的事件分发；（当然还有很多，列出你熟悉的就好。）

链接：https://www.jianshu.com/p/1986948a2ba4

****

<h2 id="离面试不到24小时，该准备什么？">
离面试不到24小时，该准备什么？
</h2>

[返回目录](#目录)

**必须准备的**
- 自我介绍
- 项目经验介绍： 你比较熟悉的项目？你在项目中遇到的最大困难，以及你最终是如何解决的？这个项目给你带来了什么？
- 自己的优势和弱势定位： 可能会有各种不同的问法，如“你觉得你的优点是什么？” “你比较擅长的事情是？”不过只要你对自己的有一个较清晰的定位（哪怕是错的），你就可以以不变应万变。拿出纸和笔记录下自己的优势和弱势，并附加上相应的案例（你的故事）。
- 尽可能了解面试公司招聘的真实职位： 招聘网上的职位简介往往不够准确，要获得更准确的信息，可以直接和HR确认“你们如果聘用我，会让我具体负责什么？”，最好是可以和面试官或在他们公司的朋友交流。知道对方想招什么样的人，你会更清楚自己应该怎么做。
- 了解面试公司的产品，猜测他们可能会遇到哪些问题: 对方招你是要去实现产品和解决问题的。比如大型的App应用，可能就会涉及插件方案，性能问题等；而小型的应用，可能更看中你的快速开发能力。


**有条件准备的**
- Java基础： JVM的内存模型，垃圾回收的策略；多线程同步的方式；
- Android基础： 可以自己说一遍是怎么理解Activity的生命周期的，当然这题也是很灵活的，可能面试官不会问你生命周期的流程，只挑其中的一点进行提问，如“ onSaveStateInstance什么时候被调用？”Activity的四种启动模式。

**准备了也没有用的**
一些你现在还没有掌握的技术点，准备它们的收益不是很大。既然你还未掌握，现在再看一遍还是难以理解透彻，可能还会出现你认为自己答对了，面试官却认为你南辕北辙的情况。比如：算法、设计模式、OpenGL等，这时候看并不利于你记忆和应付面试官可能换一个角度来问你。

**最后一刻**
请再看一遍自己投递给这家公司的简历，如实按简历上的回答，保证你的诚信。如果你的说法和简历上不相符，对你的影响是很大的。

链接：https://www.jianshu.com/p/db906f194bd2

****

<h2 id="Java内存模型">
Java内存模型
</h2>

[返回目录](#目录)

面试题：Java的内存模型
标准答案:Java内存模型即Java Memory Model，简称JMM。JMM定义了Java 虚拟机(JVM)在计算机内存(RAM)中的工作方式。程序中的变量存储在主内存中，每个线程拥有自己的工作内存并存放变量的拷贝，线程读写自己的工作内存，通过主内存进行变量的交互。JMM就是规定了工作内存和主内存之间变量访问的细节，通过保障原子性、有序性、可见性来实现线程的有效协同和数据的安全。

面试题：JVM如何判断一个对象实例是否应该被回收？
标准答案: 垃圾回收器会建立有向图的方式进行内存管理，通过GC Roots来往下遍历，当发现有对象处于不可达状态的时候，就会对其标记为不可达，以便于后续的GC回收。

面试题：说说JVM的垃圾回收策略。
标准答案: JVM采用分代垃圾回收。在JVM的内存空间中把堆空间分为年老代和年轻代。将大量创建了没多久就会消亡的对象存储在年轻代，而年老代中存放生命周期长久的实例对象。

链接：https://www.jianshu.com/p/7e0833df599b

****

<h2 id="Binder的线程数">
Binder的线程数
</h2>

[返回目录](#目录)

**多个进程同时调用一个ContentProvider的query获取数据，ContentPrvoider是如何反应的呢？**
> 一个content provider可以接受来自另外一个进程的数据请求。尽管ContentResolver与ContentProvider类隐藏了实现细节，但是ContentProvider所提供的query()，insert()，delete()，update()都是在ContentProvider进程的线程池中被调用执行的，而不是进程的主线程中。这个线程池是有Binder创建和维护的，其实使用的就是每个应用进程中的Binder线程池。

**你觉得Android设计ContentProvider的目的是什么呢？**
1. 隐藏数据的实现方式，对外提供统一的数据访问接口；
2. 更好的数据访问权限管理。ContentProvider可以对开发的数据进行权限设置，不同的URI可以对应不同的权限，只有符合权限要求的组件才能访问到ContentProvider的具体操作。
3. ContentProvider封装了跨进程共享的逻辑，我们只需要Uri即可访问数据。由系统来管理ContentProvider的创建、生命周期及访问的线程分配，简化我们在应用间共享数据（进程间通信）的方式。我们只管通过ContentResolver访问ContentProvider所提示的数据接口，而不需要担心它所在进程是启动还是未启动。

面试题：运行在主线程的ContentProvider为什么不会影响主线程的UI操作?
> ContentProvider的onCreate()是运行在UI线程的，而query()，insert()，delete()，update()是运行在线程池中的工作线程的，所以调用这向个方法并不会阻塞ContentProvider所在进程的主线程，但可能会阻塞调用者所在的进程的UI线程！所以，调用ContentProvider的操作仍然要放在子线程中去做。虽然直接的CRUD的操作是在工作线程的，但系统会让你的调用线程等待这个异步的操作完成，你才可以继续线程之前的工作。

> 当我聊到这个线程池中有16个线程可供使用时，面试官对这个结论提出了一些看法。一个进程创建后为了和在系统进程中的AMS（ActivityManangerService）通信，一个APK进程本身就是在Binder线程池中占用2个线程来维持Binder的通信。那么，上面提到的第二种不同进程的案例中，ContentProvider能同时使用的线程数量应该是14个才对啊。怎么我们测试的结果会是16呢？这一点我当时和面试官是有分歧的。因为我记得做过实验结果就是16个线程，但如过一个进程只维护一个Binder线程池的话，确实应该是14个线程才对。在这点上，我确实没有理解清楚Binder的线程池，所以我知道了实际效果可以同时运行16个线程（假设自己的实验没出错），但却无法解释面试官提的“为什么不是14个的问题”。我面试回来后，看一些说明和代码片段，一个初步的判断是，面试官提到的和AMS通信用到的线程并不会算在这个线程池中。

链接：https://www.jianshu.com/p/c70ae80cf64d


****

<h2 id="我的问题问完了，你有什么要问的吗？">
我的问题问完了，你有什么要问的吗？
</h2>

[返回目录](#目录)

**如果不问**
如果你也没什么问题想问，那么可以委婉的告诉面试官自己没什么问题要问。如：“通过一些朋友和渠道，其实我对贵公司的一些文化和愿景都还比较了解，所以我暂时也没有什么想问的，我也很希望能加入到这样一个环境中。”

**技术面试官：** 在提出问题前，我们要先看一下现在这个面试官是处在公司的什么位置。如果他也是一个开发人员，在对你做技术面试，那不妨聊聊团队的一些技术栈方面的问题。如：“你们的团队在采用敏捷开发的方式吗？”然后和面试官聊聊敏捷，分享一些各自的经验，方便双方进一步的了解。并不是所有的公司都会用敏捷，那我们可以问一些更开放性的问题，如：“在你们的项目中遇到技术障碍了，公司有什么机制去应对吗？”可以就此看看这个公司是否重视技术，有没有一些技术提升和交流的传统。
**管理类面试官：** 如果面试官是管理职位的，那么可以问问团队组成；假设你能加入的话，会分配在哪个team，team中有没有带你的人或让你得到进步的模式；或者了解一下他对团队目前状态的看法，是否有什么变化他想引入团队或组织的。也就是向管理类面试官提问，你可以问一些对团队现状和未来预期（目标）相关的一些问题，这些问题会让你提前知道，进入这家公司后你应该往哪个方向去努力。
**HR：** 公司文化什么的HR一般会主动向你介绍，薪酬和福利不清楚的地方也可以继续沟通。

>管理者：问战略；技术人员：问战术；HR行政人员：问后勤

链接：https://www.jianshu.com/p/4b3bb0f51d17



遇到两种“虐心”的情况：一种是投了简历后石沉大海，一种是面试时被问的体无完肤。
你只要把一次面试发现的问题都列出来，改掉其中一项最急迫需要改善的。只要你多“被虐”几次后，面试官也不需要再“虐”你了，因为他可以很容易给你下结论了。


**你是怎么知道自己技术差的？**
- 面试官问了一两个基础的技术问题后就不再问技术了；
- 别人说的技术问题自己都听不懂;
- leader总是分简单的工作给自己；
- 工作时觉得很痛苦，每个问题或者功能要很久才能处理好；

**高手注重细节**
> 可以说厉害的人，或者叫高手，可能只是比较多在意这些细节而已。在实践中的经历告诉我，很多难于解决的性能问题，并不是因为有一个影响性能的问题无法攻克，而是没有一个明显的制约因素，是有各种小问题一点一点堆积起来，最终积重难返。所以，把细节做好，或者意识到细节的地方可能引发的问题，对我们解决问题是很有帮助的，不要浪费了让你可以成长的细节。

**没有技术深度的苦恼**
> 当我们是初级工程师的时候，最希望的就是有丰富的项目经验，好把自己苍白干瘪的简历填的炫丽饱满。然而随着时间的积累，简历上的项目是挺“饱满”的了，但我们只看“外表”的行为造成了自己另一个困境：看似很资深，其实又没有做过什么有难度的事情，工作了十年可能只是1年的工作经验用了9次。正如这位去面试的读者，从简历上看确实是能看到他辉煌的项目经历，在经历之下会发现简历中没有深入的地方。有些虽然写的很有技术，但是确实只是在使用API的程度而已；有些解决问题的方式很有技巧，但还不成体系。可惜没有多走两步，没有去研究和扩展。随着年龄的增长，你原来的优势都在慢慢变成你的劣势。我们没有技术深度，最重要的原因有两个：第一是回避问题，第二是没有兴趣。链接：https://www.jianshu.com/p/40d24a1f6958

**怎么学习一个开源框架？**
> 很多时候也是基于工作需要或者自己的兴趣才会去弄懂一个开源框架的代码和架构。基于使用的话，确实不需要特别深入，能像你一样画个流程图，理一个大概，知道它的一些缺陷和优点其实就很不错了。不过熟悉了一两个框架后，如果有意识提高自己的话，往往会考虑自己能不能写了这样的框架，它倒底是怎么做到的，自己去实现会怎么做。如果没时间的话，可以先回忆一些平时使用这些框架遇到的问题及你的解决方法，如果完全没有问题，那就要对应不同的问题来和面试官交流了，其实也可以分享一些自己对它的看法或改进想法。

**你在工作中有遇到什么问题？**
> 在工作中开发时遇到一个需求：在主Activity M 中启动另一个Activity B，但是要求必须要上次的A Activity执行完onDestory()方法，怎么办？
