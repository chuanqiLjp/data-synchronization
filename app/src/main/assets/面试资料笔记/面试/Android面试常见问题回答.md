# 非技术
- 你对未来三到五年的职业规划。
- 项目中遇到哪些难题，最终你是如何解决的？
- 自我介绍
- 为什么从上一家公司离职
- 我的问题问完了，你还有什么要问的
> 对这次面试做个总结和对我评价(其实就看也看出是否有意向);根据面试，您觉得我的能力是否能够胜任贵公司的工作; 您觉得我哪方面知识需要深入学习或者我的不足在那些方面，今后我该注意什么; 这些问题不仅能帮助你，还能对这次面试做到心中有数。

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 开源框架
- 常用哪些开源项目，说说最熟悉的一个？
- 图片加载：Glide、Picasso、Fresco；photoview（图片缩放）、circleimageview（圆形图片）、
- 网络请求：OKhttp、OkhttpUtils、Retrofit
- XML的解析：DOM、Pull、SAX
- JSON的解析：GSOn、FastJson
- 数据库：Greendao、ActiveAndroid、
- RecycleView：EasyRecycleView、
- 下拉刷新：SwipeToLoadLayout、
- Activity滑动销毁：SwipebackLayout、
- 网页解析：Jsoup、
- EventBus、BufferKnife、RxJava、组件开发跳转alibaba-ARouter
- 广告条或启动页：banner
- 左滑删除：SwipeLayout

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 第三方接入
- 支付接入：微信、支付宝、Mob第三方支付
- 地图：百度、高德的定位和轨迹追踪，LBS应用：使用百度或高德地图的SDK遇到过的问题（国内外限制、精确度等）？
- 分享：ShareSDK、极光分享、Umeng社会化分享登录
- 第三方登录： ShareSDK、Umeng社会化分享登录
- 用户统计：友盟统计、极光统计
- 短信验证码：Mob的短信验证码（免费）、极光短信（收费）、
- 热修复、热更新、插件开发：微信-Tinker，阿里-AndFix，美团-Robust，QQ空间超级补丁-QZone,
- 消息推送：极光推送、友盟、小米推送、华为推送、个推、百度云推、
- 视频直播：网易云、融云直播、七牛直播云
- 常见API：mobAPI、聚合数据、百度apistore、
- 视频录制：mob的ShareREC进行游戏视频录制、
- 后台数据：Bmob后端云、七牛云存储（免费）
- 广告集成：有米、指点、安智、点金
- 云测试：蒲公英测试、百度云测
- 即时通讯：环信、融云、极光im、网易云信
- 语音识别：讯飞
- 相机：Camera360美容、锐动天地的短视频编辑、美摄（录制的视频带有美颜等特效）、趣拍（收费了）

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

# 实战编程
- 手写一段代码，如何找出一段字符串中，出现最多的汉字是哪个。

--------------------------------------------------------------------------------------------------------------------------------

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 四大组件
> 如果一个进程中没有四大组件在运行那么该进程会很快被系统杀死，比较好的方法是将后台工作放在Service中从而保证进程有一定的优先级


****

<h2 id="AndroidManifest.xml的里有什么和作用">
AndroidManifest.xml的里有什么和作用
</h2>

[AndroidManifest--你真的理解了吗？](https://www.jianshu.com/p/6ed30112d4a4)
[返回目录](#目录)

1. AndroidManifest里有包名、权限、资源命名空间、版本号、四大组件；

2. 当Android启动一个应用程序组件之前，它必须知道哪些个组件是存在的，所以开发人员在开发过程中，必须将应用程序中出现的组件一一在AndroidManifest.xml文件中申明，最终这个AndroidManifest.xml文件也会被一起打包到.apk文件中去。常用于自定义权限和注册并声明四大组件的属性；


****

<h2 id="Activity的生命周期">

</h2>

[返回目录](#目录)

1. onCreate()--->onStart()--->onResume()--->onPause()--->onStop()--->onDestory(), onRestart();
2. 从生到死：onCreate()和onDestory()；从可见到不可见：onStart()和onStop()；从可交互到不可交互：onResume()和onPause()；
3. 旧的Activity一定先会调用onPause方法然后新的Activity再启动，所以onPause不能做重量级的操作，
4. Activity异常终止时：调用onSaveInstanceState(调用时机在onStop之前)保存状态，重建Activity实例时调用onRestoreInstanceState(调用时机在onStart之后)恢复数据

****

<h2 id="屏幕旋转时的生命周期">
屏幕旋转时的生命周期
</h2>

[返回目录](#目录)

1. 不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次;
2. 设置Activity的android:configChanges="orientation"时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次;
3. 设置Activity的android:configChanges="orientation|keyboardHidden"时，切屏不会重新调用各个生命周期，只会执行 onConfigurationChanged 方法。
4. 建议配置android:configChanges="orientation|screenSize|keyboardHidden",



****

<h2 id="Activity异常时数据的保存">
Activity异常时数据的保存
</h2>

[返回目录](#目录)

系统会调用onSaveInstanceState()来保存Activity的状态，onSaveInstanceState在onStop之前调用正常情况下不会被调用并会把所保存的Bundle传递给onCreate和onRestoreInstanceState方法，当Activity被重建后会调用onRestoreInstanceState，onRestoreInstanceState调用时间在onStart之后，通常建议使用onRestoreInstanceState去恢复数据因为不用判NULL，onSaveInstanceState和onRestoreInstanceState方法只有Activity异常终止时才会被调用，其他情况不会触发，

****

<h2 id="Activity的启动模式">
Activity的启动模式
</h2>

[返回目录](#目录)

1. standard：标准模式，系统默认的模式，每次启动一个Activity都会创建一个新的实例，不管该实例是否已经存在，该模式下，谁启动了这个Activity就把这个Activity压入启动它的Activity所在的栈中，那个具有任务栈的Context启动Activity就会把Activity实例压入到那个任务栈中，非Activity类型的Context（如：Application、Service）并没有所谓的任务栈，若需要启动Activity需添加FLAG_ACTIVITY_NEW_TASK标记位，这样启动时会创建一个新的任务栈，待启动的Activity实际是以singleTask模式启动了；

2. singleTop：栈顶复用模式，如果在任务的栈顶正好存在该Activity的实例，就重用该实例( 会调用实例的 onNewIntent() )，否则就会创建新的实例并放入栈顶，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。使用场景如新闻类或者阅读类App的内容页面。

3. singleTask：栈内复用模式，一种单实例模式，如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的 onNewIntent() )。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。使用场景如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

4. singleInstance：单实例模式，一种加强的singleTask，在一个新栈中创建该Activity的实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例( 会调用实例的 onNewIntent() )。其效果相当于多个应用共享一个应用，不管谁激活该 Activity 都会进入同一个应用中。使用场景如闹铃提醒，将闹铃提醒与闹铃设置分离。singleInstance不要用于中间页面，如果用于中间页面，跳转会有问题，比如：A -> B (singleInstance) -> C，完全退出后，在此启动，首先打开的是B。

> singleTask和singleInstance标记的B，若A以startActivityForResult启动B，则A的onActivityResult会立即回调，而不会等待返回，因为不同Task之间默认是不能传递数据的【A或B设置了singleTask/singleInstance都是这种情况】；

****

## 若A是singleTask/singleInstance，使用startActivityForResult启动B，需要获取B回传的数据，如何操作？

解决方案：若在A中重写onActivityResult去接收数据，会接收不到，因为启动B以后会立即回调onActivityResult方法而不会等待返回，此时可以在B中使用startActivity传递Intent，然后在A中使用onNewIntent方法接收传递回来的数据；

****

<h2 id="如何判断Activity是否在运行？">
如何判断Activity是否在运行？
</h2>

[返回目录](#目录)

> 从Activity A 启动一个线程去进行网络上传操作，在A中设立一个回调函数，当上传操作完成以后，在A的这个回调函数中会弹出一个对话框，用来显示回调信息。可是当上传的过程还在进行的时候，我按下back键，A的activity 被销毁了，可是那个上传的线程还在进行，当那个线程结束后，本来应该在A中弹出一个对话框，可是由于A已经不存在了，系统就会报错提示，“将对话框显示在不存在的页面上”，然后程序就挂掉了。我看到过很多人用Handler来充当上面所提到的“回调函数”，即工作线程完成工作后，通过主线程的Handler处理UI更新，如弹出提示Dialog。可能有些人没有弄明白，Activity都被系统销毁了，工作线程怎么还能调它的变量呢？其实所谓的Activity销毁只是不再受系统的AMS控制，但Activity这个对象的实例还是存在于内存中的，具体什么时候真正把这个对象实例也销毁（回收）了，就要看内存回收机制了，哪怕是这个实例没有可达的引用了也不一定会马上回收。

```
activity == null || activity.isDestroyed() || activity.isFinishing()
```

****

<h2 id="两个Activity之间如何传递参数？">
两个Activity之间如何传递参数？
</h2>

[返回目录](#目录)

启动Activity时使用Intent传递数据，若需要新的Activity回传数据，则使用startActivityForResult启动Activity，在onActivityResult中接收回传的数据，通过setResult设置回传的数据；



****

<h2 id="Service的启动方式及生命周期">
Service的启动方式及生命周期
</h2>

[返回目录](#目录)

1. **通过start方式启动Service：** context.startService()   ---> onCreate() ---> onStartCommand() ---> Service running  --->  调用context.stopService()   ---> onDestroy()，第一次 启动服务时，运行 onCreate -->onStartCommand，后面在启动服务时，服务只执行onStartCommand，不再执行OnCreate
2. **通过bind方式启动Service:** context.bindService()  --->  onCreate()  --->  onBind()  --->  Service running  ---> 所有客户端被销毁或客户端调用了unbindService()  --->  onUnbind()   --->  onDestroy()，第一次绑定时会调用onCreate->onBind()。随后无论哪个组件再绑定几次该Service。服务A的onCreate()和onBind()只调用一次。

****

<h2 id="注册广播有几种方式，有何优缺点">
注册广播有几种方式，有何优缺点
</h2>

[Android四大组件——BroadcastReceiver普通广播、有序广播、拦截广播、本地广播、Sticky广播、系统广播](https://blog.csdn.net/qq_30379689/article/details/53341313)

[返回目录](#目录)

1. 普通广播（自定义广播）：普通广播是完全异步的，通过Context的sendBroadcast()方法来发送，消息传递效率比较高，但所有receivers（接收器）的执行顺序不确定。缺点是：接收者不能将处理结果传递给下一个接收者，并且无法终止广播Intent的传播，直到没有与之匹配的广播接收器为止。
    1. 代码：注册广播：registerReceiver(mReceiver,intentFilter);反注册：unregisterReceiver(receiver);
    2. 清单文件：在与Activity同级声明广播接收者名称和intent-filter过滤器
    3. 发送广播：sendBroadcast(intent);intent要设置好相关的匹配；
2. 有序广播：有序广播通过Context.sendOrderedBroadcast()来发送，所有的广播接收器优先级依次执行，广播接收器的优先级通过receiver的intent-filter中的android:priority属性来设置，数值越大优先级越高。当广播接收器接收到广播后，可以使用setResult()函数来结果传给下一个广播接收器接收，然后通过getResult()函数来取得上个广播接收器接收返回的结果。当广播接收器接收到广播后，也可以用abortBroadcast()函数来让系统拦截下来该广播，并将该广播丢弃，使该广播不再传送到别的广播接收器接收，
    1. 注册广播：这里的注册方式和普通广播是一样的，这里的区别在于priority属性，确定了他们之间的优先级，
    2. 发送广播：sendOrderedBroadcast(intent,null);这里需要发送的是有序广播，否则在接收者中通过setResult()和getResult()方法会报错，因为只有有序广播才能传递结果
    3. 拦截广播：abortBroadcast();
    4. 有序广播、拦截广播的拓展——终结广播：现在有这样的一个应用场景，按照上面的程序走，只能在第一个广播中被拦截住了，后面的广播则不执行。如果这个时候我们需要一个不管有没有被拦截都必须执行的广播，我们称为终结广播，那应该怎么办。同样的，发送有序广播也考虑到这一点，通过以下代码来发送广播，并指定我们不管有没有被拦截都必须执行的终结广播，sendOrderedBroadcast(intent, null, new PriorityBroadcastReceiver.LowPriority(),new Handler(), 0, null, null);
3. 本地广播：在API21的Support v4包中新增本地广播，也就是LocalBroadcastManager。由于之前的广播都是全局的，所有应用程序都可以接收到，这样就会带来安全隐患，所以我们使用LocalBroadcastManager只发送给自己应用内的信息广播，限制在进程内使用,它的用法很简单，只需要把调用context的sendBroadcast、registerReceiver、unregisterReceiver的地方换为LocalBroadcastManager.getInstance(Context context)中对应的函数即可。这里创建广播的过程和普通广播是一样的过程
    1. 注册：LocalBroadcastManager.getInstance(ReceiverActivity.this).registerReceiver(receiver,new IntentFilter("com.handsome.hensen"));
    2. 反注册：LocalBroadcastManager.getInstance(ReceiverActivity.this).unregisterReceiver(receiver);
    3. 发送异步广播：LocalBroadcastManager.getInstance(ReceiverActivity.this).sendBroadcast(intent);
    4. 发送同步广播：LocalBroadcastManager.getInstance(ReceiverActivity.this).sendBroadcastSync(intent);
4. Sticky广播：sticky广播通过Context.sendStickyBroadcast()函数来发送，用此函数发送的广播会一直滞留，当有匹配此广播的广播接收器被注册后，该广播接收器就会收到此条信息。使用此函数需要发送广播时，需要获得BROADCAST_STICKY权限，sendStickyBroadcast只保留最后一条广播，并且一直保留下去，这样即使已经有广播接收器处理了该广播，当再有匹配的广播接收器被注册时，此广播仍会被接收。如果你只想处理一遍该广播，可以通过removeStickyBroadcast()函数来实现。这里创建广播的过程和普通广播是一样的过程；
5. 系统广播：
> 屏幕被关闭之后的广播：Intent.ACTION_SCREEN_OFF；屏幕被打开之后的广播：Intent.ACTION_SCREEN_ON；充电状态，或者电池的电量发生变化：Intent.ACTION_BATTERY_CHANGED；关闭或打开飞行模式时的广播：Intent.ACTION_AIRPLANE_MODE_CHANGED；表示电池电量低：Intent.ACTION_BATTERY_LOW；表示电池电量充足，即电池电量饱满时会发出广播：Intent.ACTION_BATTERY_OKAY；按下照相时的拍照按键(硬件按键)时发出的广播：Intent.ACTION_CAMERA_BUTTON

****

<h2 id="说说Android为什么要设计ContentProvider这个组件吗">
说说Android为什么要设计ContentProvider这个组件吗
</h2>

[返回目录](#目录)

1. 封装。对数据进行封装，提供统一的接口，使用者完全不必关心这些数据是在DB，XML、Preferences或者网络请求来的。当项目需求要改变数据来源时，使用我们的地方完全不需要修改。
2. 提供一种跨进程数据共享的方式
3. ContentResolver接口的notifyChange函数来通知那些注册了监控特定URI的ContentObserver对象，使得它们可以相应地执行一些处理。ContentObserver可以通过registerContentObserver进行注册。

> 每个ContentProvider的操作是在哪个线程中运行的呢（其实我们关心的是UI线程和工作线程）？比如我们在UI线程调用getContentResolver().query查询数据，而当数据量很大时（或者需要进行较长时间的计算）会不会阻塞UI线程呢？

1. ContentProvider和调用者在同一个进程，则ContentProvider的方法（query/insert/update/delete等）和调用者是处在同一线程中的；
2. ContentProvider和调用者在不同的进程，则ContentProvider的方法会运行在它自身所在进程的一个Binder线程中。
3. 以上两种方式在ContentProvider的方法没有执行完成前都会阻塞调用者，因此会阻塞UI线程



****

<h2 id="Fragment的生命周期">
Fragment的生命周期
</h2>

[返回目录](#目录)

1. onAttach(Activity)：连接宿主Activity；
2. onCreate(Bundle)：创建Fragment；
3. onCreateView(LayoutInflater,ViewGroup,Bundle)：创建Fragment的视图；
4. onActivityCreated(Bundle)：当宿主Activity的onCreate执行完之后调用；
5. onStart()：可见时调用
6. onResume()：获取焦点可交互时调用
7. onPause()：失去焦点不可交互时调用
8. onStop()：不可见时调用
9. onDestoryView()：销毁Fragment视图，与onCreateView对应；
10. onDestory()：销毁Fragment，与onCreate对应；
11. onDetach()：与宿主Activity断开连接，与onAttach对应；


****

<h2 id="两个Fragment之间如何进行通信？">
两个Fragment之间如何进行通信？
</h2>

[返回目录](#目录)

1. 可以通过getSupportFragmentManager()拿到FragmentManager，然后通过FragmentManager的findFragmentByTag或者findFragmentById拿到我们需要通信的Fragment（比如说在下面的DEMO中我们用的是FragmentTabHost，所以就使用findFragmentByTag拿到Fragment，如果Fragment是直接在XML中定义的，那么就使用findFragmentById拿到Fragment），然后就可以对拿到的Fragment进行各种操作了。
2. 可以先向宿主activity传值，再在宿主activity中向Fragment传值；


****

<h2 id="如何解决Fragment的重影问题">
如何解决Fragment的重影问题
</h2>

[返回目录](#目录)

> 原因：依附的Activity被销毁，短生命周期持有常生命周期的引用，因为使用了Fragment的状态保存，当系统内存不足，Fragment的宿主Activity回收的时候，Fragment的实例并没有随之被回收。Activity被系统回收时，会主动调用onSaveInstance()方法来保存视图层（View Hierarchy），所以当Activity通过导航再次被重建时，之前被实例化过的Fragment依然会出现在Activity中，在onCreate中又再次重建了新的Fragment，综上这些因素导致了多个Fragment重叠在一起。

解决方法：
1. 通过 FragmentManager().findFragmentByTag解决：我们add fragment的时候添加一个tag，当系统异常重新启动activity和fragment的时候 我们通过判断onCreateView(inflater, container, savedInstanceState)中的savedInstanceState！=null来找到系统帮我恢复的fragment并赋值给你的变量中制定的fragment。进行判空操作.如果为空,创建对象,add上去;如果不为空,直接show出来(注意不要使用remove方法移除,hide即可)；
2. 简单的做法,就是给每层fragment加上背景色,重叠后也就看不见了，实际上上次的Fragment没有被回收，存在消耗资源；
3. 注释掉Activity中onSaveInstanceState()里super.onSaveInstanceState(outState)代码，但是此时注意Activity异常回收时不会保存状态需要自己保存；


****

<h2 id="Activity与Fragment的通信">
Activity与Fragment的通信
</h2>

[返回目录](#目录)

1. **Activity向Fragment传值：** 在Activity中通过fragment.setArguments(Bundle)传值，这种方式只适用还没有显示的Fragment（注：Fragment只能新建），在Fragment中，通过Fragment.getArguments()获取Bundle，获取后先判空
2. **Fragment向Activity传值：** 在Activity中定义一个公共的方法，参数设置为要传值的数据类型，在Fragment中通过getActivity()可以获取到宿主的activity对象【MainActivity mActivity=(MainActivity) getActivity()】，在使用这个对象调用那个公共的方法，把数据传给方法的参数


- Activity多次调用Fragment的setArguments方法会崩溃？现在测试还没有

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 性能优化
- 布局优化
- 绘制优化
- 内存泄露优化（MAT分析和LeakCanary分析检测内存泄露）
- ListView的优化,ListView如何优化，复用的原理，为什么会图片错位，如何解决，分页的思想是什么。
- Bitmap的优化
- 数据库的优化
- OOM与内存泄漏

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### UI
- 说说Android事件分发机制，ViewGroup和View上的分发有什么不同？
- View的绘制流程
- 如何实现一个自定义View，自定义View的流程
- SurfaceView的介绍
- Touch事件的传递机制
- 自定义View的状态是如何保存的？
- 通过new创建的View实例它的onSaveStateInstance会被调用吗？
- 有遇到过哪些屏幕和资源适配问题？
- Android中的动画有哪些有什么区别
- 如何解决ScrollView嵌套中一个ListView的滑动冲突？
- Menu菜单常见的方法及作用

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 线程
- HandlerThread常规使用步骤
- 说说AsyncTask原理
- 如何处理线程同步的问题？几种线程池
- IntentService
- Android子线程与主线程交互方式，原理以及各自的优缺点。

****

<h2 id="对静态方法加锁和对普通方法加锁有什么不同">
对静态方法加锁和对普通方法加锁有什么不同
</h2>

[返回目录](#目录)

1. synchronized static是某个类的范围，synchronized static cSync{}防止多个线程同时访问这个类中的synchronized static 方法。它可以对类的所有对象实例起作用。
2. synchronized 是某实例的范围，synchronized isSync(){}防止多个线程同时访问这个实例中的synchronized 方法。
3. synchronized方法与synchronized代码快的区别：synchronizedmethods(){}与 synchronized（this）{}之间没有什么区别，只是 synchronized methods(){} 便于阅读理解，而 synchronized （ this）{}可以更精确的控制冲突限制访问区域，有时候表现更高效率。
4. synchronized关键字是不能继承的也就是说，基类的方法synchronized f(){} 在继承类中并不自动是synchronized f(){}，而是变成了f(){}。继承类需要你显式的指定它的某个方法为synchronized方法；

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 高级进阶
- 设计模式：单例模式（会手写）、观察者模式、建造者模式、工厂模式
- 观察者模式与回调函数的区别
- 设计架构：MVC、MVP、MVVM、DataBinding的使用
- Handler的运行机制(原理)，线程间是如何通信的？
- Message、Handler、MessageQueue、Looper之间的关系
- Android中跨进程通讯（IPC）有几种方式？
- AIDL的全称是什么？如何工作？能处理哪些类型的数据？
- Binder的原理（机制），Binder是什么？它是如何实现跨进程通信的？
- Binder的线程数
- 如何解决65535方法数的问题
- Activity、View、Window三者之间的关系
- 为什么Dialog不能用Application的Context
- 安全：数据加密（DES、3DES、SM4、SM2、RSA），代码混淆，WebView/Js调用，https、代码混淆、APK加固
- JNI、NDK的开发：是否接触过JNI/NDK，java如何调用C语言的方法
- 数据结构、算法：冒泡排序、
- 如何选择第三方，从那些方面考虑
- 从那些角度可以减少APK体积的
- 如何实现进程保活
- 说下冷启动与热启动是什么，区别，如何优化，使用场景等。
- session与cookie的区别 、
- Android四大组件启动机制之Activity启动过程
- 无障碍服务AccessibilityService


******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

### 其他
- XML的有哪几种解析区别是什么？
- JSON的解析有哪些？
- JSON与XML的区别，为啥使用JSON传输网络数据
- Android的数据存储方式
- ANR是什么？怎么避免
- 什么情况会导致ForceClose，如何避免
- Android的系统架构
- 如何实现应用内多语言切换？语言的国际化
- 如何对SQLite数据库中进行大量的数据插入？
- Android中Java和JavaScript如何交互？
- 知道什么是ART吗？它和Dalvik有什么区别？
- 如何对SQLite数据库中进行大量的数据插入？
- 知道什么是ART吗？它和Dalvik有什么区别？
- 运行时权限、Design风格、
- Android6.0/7.0/8.0特性，kotlin语言，I/O大会
- 你在公司中用的什么代码管理，如何解决git冲突；你说用的代码管理工具什么，为什么会产生代码冲突，该如何解决
- TCP/UDP有什么异同
- OOM，性能优化，ListView的滑动优化，

******************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************

# Java基础
- StringBuilder与StringBuffer的区别
- Java内存模型和垃圾回收机制
- Java的值传递和引用传递问题
- java高级知识，注解，反射，泛型的理解与作用
- 说下LinkedList与ArrayList，HashTable与HashMap的区别与存储过程与遍历方式。
- 说下java虚拟机的理解，回收机制，JVM是如何回收对象的，有哪些方法等
- 排序算法：冒泡排序、选择排序、插入排序、希尔排序、堆排序、快速排序、归并排序、基数排序
- HashMap、HashSet、HashTable、LinkedHashMap、LinkedHashSet、ArrayList、LinkedList的源码
- JVM虚拟机的内存回收策略，JVM的GC原理
- Java的引用类型：强引用、软引用、弱引用、虚引用
- 集合的树结构，Set与List的区别，数据的排序，sort方法，让类实现Comparable这个接口重写里面的compareTo()方法

List集合:特点:有序，可以重复
Set集合:特点:无序，不可以重复  常见子类：HashSet、TreeSet  （在java中带有Tree的就有排序的功能）
无序:
Set集合没有拓展方法------->简单
HansCode值的来历
   HashCode值默认的是当前对象的内存地址
hashSet的存储原理
   1:当我们调用hashSet的add方法的时候，实际上首先会调用当前对象的HashCode值来进行比较,比较的结果有两种
                1:HashCode值不等的时候
                  他会把当前的hashCode值通过一系列的移位运算计算出当前对象应该存储的地点,然后进行存储
                2:HashCode值相等的时候
                  然后再比较当前对象的equals方法是否返回true
                         1:返回true说明当前对象确实存在那么就不存储当前的数据/false
                         2:当他返回fasle的时候那么就存储当前的对象
TreeSet：
    能干什么?  能够对加入集合的数据进行排序
TreeSet的方法
     E ceiling(E e) 返回此 set 中大于等于给定元素的最小元素；如果不存在这样的元素，则返回 null。
     first() 返回此 set 中当前第一个（最低）元素
     floor(E e) 返回此 set 中小于等于给定元素的最大元素；如果不存在这样的元素，则返回 null。
如何定义比较器
  第一种方法:
     第一步:让当前对象实现于Comparable这个接口重写里面的compareTo（）方法
     第二步:自定义比较规则
             比较谁就用谁来做减法
                减法的结果三个
                如果减法的结果是正数说明前面比后面打
                如果减法的结果是0说明两个一样大
                如果减法的结果是负数说明后面比前面大
 第二种方法:
            编写一个类实现于Comparator重写里面那个方法
            定义哈那个比较规则
![image.png](https://upload-images.jianshu.io/upload_images/4143664-7ce532fabd3ec2b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- "=="与equles的区别