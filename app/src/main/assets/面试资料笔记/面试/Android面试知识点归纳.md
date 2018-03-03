
<h1 id="index">目录</h1>

* [Android 系统架构](#Android系统架构)
* [Activity 生命周期](#Activity生命周期)
* [Activity的四种加载模式以及使用场景](#Activity的四种加载模式以及使用场景)
* [如何理解Activity,View,Window三者之间的关系？](#如何理解Activity,View,Window三者之间的关系？)
* [Fragment的生命周期](#Fragment的生命周期)
* [Activity与Fragment通信](#Activity与Fragment通信)
* [Service](#Service)
* [SQLite](#SQLite)
* [Binder机制](#Binder机制)
* [IPC——跨进程通讯](#IPC——跨进程通讯)
* [View的绘制流程](#View的绘制流程)
* [自定义View和ViewGroup](#自定义View和ViewGroup)
* [TouchEvent事件的传递机制](#TouchEvent事件的传递机制)
* [滑动冲突的解决方法](#滑动冲突的解决方法)
* [Android中的三种动画](#Android中的三种动画)
* [AIDL的使用](#AIDL的使用)
* [应用程序Activity的启动过程](#应用程序Activity的启动过程)
* [内存泄露以及优化](#内存泄露以及优化)
* [Handler原理](#Handler原理)
* [ JVM虚拟机/Dalvik/ART](#JVM虚拟机/Dalvik/ART)
* [Android开机启动流程](#Android开机启动流程)
* [热修复](#热修复)


<h1 id="Android系统架构">Android 系统架构</h1>

![image.png](http://upload-images.jianshu.io/upload_images/4143664-badb8423ded73944.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.应用程序层

Android平台不仅仅是操作系统，也包含了许多应用程序，诸如SMS短信客户端程序、电话拨号程序、图片浏览器、Web浏览器等应用程序。这些应用程序都是用Java语言编写的，并且这些应用程序都是可以被开发人员开发的其他应用程序所替换，这点不同于其他手机操作系统固化在系统内部的系统软件，更加灵活和个性化。

2.应用程序框架层

应用程序框架层是我们从事Android开发的基础，很多核心应用程序也是通过这一层来实现其核心功能的，该层简化了组件的重用，开发人员可以直接使用其提 供的组件来进行快速的应用程序开发，也可以通过继承而实现个性化的拓展。该层包括各种Manager，包括ActiviyManager，WindowManager等，还包括ContentProvider和View System。

3.系统运行库层

系统运行库层可以分成两部分，分别是系统库和Android运行时。系统库是应用程序框架的支撑，是连接应用程序框架层与Linux内核层的重要纽带。 Android应用程序时采用Java语言编写，程序在Android运行时中执行，其运行时分为核心库和Dalvik虚拟机两部分。

Dalvik和ART：

**什么是Dalvik？**
Dalvik是Google公司自己设计用于Android平台的虚拟机。
Dalvik虚拟机是Google等厂商合作开发的Android移动设备平台的核心组成部分之一。
它可以支持已转换为 .dex格式的Java应用程序的运行，.dex格式是专为Dalvik设计的一种压缩格式，适合内存和处理器速度有限的系统。
Dalvik 经过优化，允许在有限的内存中同时运行多个虚拟机的实例，并且每一个Dalvik 应用作为一个独立的Linux 进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。
很长时间以来，Dalvik虚拟机一直被用户指责为拖慢安卓系统运行速度不如IOS的根源。**2014年**，谷歌直接删除Dalvik，代替它的是传闻已久的ART。

**什么是ART？**
即Android Runtime
ART 的机制与 Dalvik 不同。在Dalvik下，应用每次运行的时候，字节码都需要通过即时编译器（just in time ，JIT）转换为机器码，这会拖慢应用的运行效率，而在ART 环境中，应用在第一次安装的时候，**字节码就会预先编译成机器码**，使其成为真正的本地应用。这个过程叫做预编译（AOT,Ahead-Of-Time）。这样的话，应用的启动(首次)和执行都会变得更加快速。

4.硬件抽象层HAL

HAL能以封闭源码的形式提供**硬件驱动模块**。其目的是把Android Framework与Linux Kernel隔开，让Android不至于过度依赖Linux Kernel，使开发人员不用了解具体设备特性，就可以通过硬件抽象层来获得各种设备的信息，以便于应用开发。

5.Linux内核层

Android是基于Linux内核，其核心系统服务如安全性、内存管理、进程管理、网路协议以及驱动模型都依赖于Linux内核。

[回到目录](#index)

<h1 id="Activity生命周期">Activity生命周期</h1>

在Activity的生命周期中，如下的方法会被系统回调：

|   方法名     |      方法备注   |
|  -----------|:-------------:|
|  onCreate(Bundle savedInstanceState)	|	Activity被创建时调用。|
|  onStart()	|	Activity已经启动，未获取到焦点，还不可以与用户进行交互。|
|  onResume()	|	当Activity可见，已获取到焦点可以与用户交互。  |
|  onPause()	|	暂停Activity时被调用，调用了该方法后，Activity变得不可交互即失去焦点但仍然可见，可恢复至onResume()。  |
|  onStop()	    |	停止Activity时被调用，Activity变得不可见,可恢复至onRestart()。  |
|  onDestroy()	|	销毁Activity时被调用。  |
|  onRestart()	|	重启Activity时被调用，当Activity从不可见重新变为可见时，就会调用该方法。  |

![image.png](http://upload-images.jianshu.io/upload_images/4143664-52195a6c5378e50a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/4143664-51aedd04cd5ab89b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**横竖屏切换时候activity的生命周期?**
A. 不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次
B. 设置Activity的android:configChanges="orientation"时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次
C. 设置Activity的android:configChanges="orientation|keyboardHidden"时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法。

[回到目录](#index)


<h1 id="Activity的四种加载模式以及使用场景">Activity的四种加载模式以及使用场景</h1>

**standard 模式**

这是默认模式，每次激活Activity时都会创建Activity实例，并放入任务栈中。使用场景：大多数Activity。

**singleTop 模式**

如果在任务的栈顶正好存在该Activity的实例，就重用该实例( 会调用实例的 onNewIntent() )，否则就会创建新的实例并放入栈顶，即使栈中已经存在该Activity的实例，只要不在栈顶，都会创建新的实例。使用场景如新闻类或者阅读类App的内容页面。

**singleTask 模式**

如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的 onNewIntent() )。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移出栈。如果栈中不存在该实例，将会创建新的实例放入栈中。使用场景如浏览器的主界面。不管从多少个应用启动浏览器，只会启动主界面一次，其余情况都会走onNewIntent，并且会清空主界面上面的其他页面。

**singleInstance 模式**

在一个新栈中创建该Activity的实例，并让多个应用共享该栈中的该Activity实例。一旦该模式的Activity实例已经存在于某个栈中，任何应用再激活该Activity时都会重用该栈中的实例( 会调用实例的 onNewIntent() )。其效果相当于多个应用共享一个应用，不管谁激活该 Activity 都会进入同一个应用中。使用场景如闹铃提醒，将闹铃提醒与闹铃设置分离。singleInstance不要用于中间页面，如果用于中间页面，跳转会有问题，比如：A -> B (singleInstance) -> C，完全退出后，在此启动，首先打开的是B。

[回到目录](#index)


<h1 id="如何理解Activity,View,Window三者之间的关系？">如何理解Activity，View，Window三者之间的关系？</h1>
打个比方。Activity像一个工匠（控制单元），Window像窗户（承载模型），View像窗花（显示视图）LayoutInflater像剪刀，Xml配置像窗花图纸。

1：Activity构造的时候会初始化一个Window，准确的说是PhoneWindow。

2：这个PhoneWindow有一个“ViewRoot”，这个“ViewRoot”是一个View或者说ViewGroup，是最初始的根视图。

3：“ViewRoot”通过addView方法来一个个的添加View。比如TextView，Button等

4：这些View的事件监听，是由WindowManagerService来接受消息，并且回调Activity函数。比如onClickListener，onKeyDown等。

[回到目录](#index)

<h1 id="Fragment的生命周期">Fragment的生命周期</h1>

![image.png](http://upload-images.jianshu.io/upload_images/4143664-10c71700762e5df8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


onAttach()：执行该方法时，Fragment与Activity已经完成绑定，该方法有一个Activity类型的参数，代表绑定的Activity，这时候可以执行诸如mActivity = activity的操作。

onCreate()：初始化Fragment。可通过参数savedInstanceState获取之前保存的值。

onCreateView()：初始化Fragment的布局，返回一个与Fragment对应的View对象。加载布局和findViewById的操作通常在此函数内完成，但是不建议执行耗时的操作，比如读取数据库数据列表。

onActivityCreated()：执行该方法时，与Fragment绑定的Activity的onCreate方法已经执行完成并返回，在该方法内可以进行与Activity交互的UI操作，所以在该方法之前Activity的onCreate方法并未执行完成，如果提前进行交互操作，会引发空指针异常。

onStart()：执行该方法时，Fragment由不可见变为可见状态。

onResume()：执行该方法时，Fragment处于活动状态，用户可与之交互。

onPause()：执行该方法时，Fragment处于暂停状态，但依然可见，用户不能与之交互。

onStop()：执行该方法时，Fragment完全不可见。

onDestroyView()：销毁与Fragment有关的视图，但未与Activity解除绑定，依然可以通过onCreateView方法重新创建视图。通常在ViewPager+Fragment的方式下会调用此方法。

onDestroy()：销毁Fragment。通常按Back键退出或者Fragment被回收时调用此方法。

onDetach()：解除与Activity的绑定。在onDestroy方法之后调用。

与Activity的声明周期进行对比：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-a9eddb3c4723147f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


[回到目录](#index)

<h1 id="Activity与Fragment通信">Activity与Fragment通信</h1>

1.Fragment从Activity获取数据

直接在Fragment中使用`getActivity.getIntent()` ，这样就能拿到Intent，从而获取Intent中携带的数据。

2.Activity从Fragment中获取数据

```java
//创建Fragment的实例
ExampleFragment fragment =(ExampleFragment)getFragmentManager().findFragmentById(R.id.example_fragment);
//调用fragment中的方法(前提是在fragment中提前定义)
fragment.setXXX()；
fragment.getXXX();
```

3.fragment之间相互通信

直接在一个Fragment中调用另外一个Fragment中的方法。如

```java
ContentFragment cf = (ContentFragment) getActivity()
                         .getFragmentManager().findFragmentById(R.id.content_fg);
cf.show(name);
```

4.使用回调接口的方式实现Activity与Fragment通信

在Fragment中定义接口：

```java
public class MyFragmnt extends Fragment{
  private onTextListener mListener;
  ***
    public interface onTextListener(){
    	public void onTest(String str);
  }
  ***
}

```

在Activity中实现接口中的方法：

```java
public class MainActivity extends Activity implements MyFragment.onTestListener {
  ****
  public void onTest(String str){
    Toast.make(this,str,Toast.LENGTH_SHORT).show();
  }

 ***
}
```



在fragment的onAttach()中：

```java
@Override
public void onAttach(Activity activity){
      super.onAttach(activity);
      try{
          mListener =(onTestListener)activity;
      }catch(ClassCastException e){
          throw new ClassCastException(activity.toString()+"must implement onTestListener");
      }
  }
```



然后fragment在合适的地方就可以调用`mListener.onTest(str)`

Fragment与Activity通信，大概归纳为：

**a、如果你Activity中包含自己管理的Fragment的引用，可以通过引用直接访问所有的Fragment的public方法**

**b、如果Activity中未保存任何Fragment的引用，那么没关系，每个Fragment都有一个唯一的TAG或者ID,可以通过getFragmentManager.findFragmentByTag()或者findFragmentById()获得任何Fragment实例，然后进行操作。**

**c、在Fragment中可以通过getActivity得到当前绑定的Activity的实例，然后进行操作。**

注：如果在Fragment中需要Context，可以通过调用getActivity(),如果该Context需要在Activity被销毁后还存在，则使用getActivity().getApplicationContext()。

[回到目录](#index)

<h1 id="Service">Service</h1>

创建自定义Service需要重写父类的如下方法：

* void onCreate()：该方法在该Service第一次被创建时调用。
* int onStartCommand(Intent intent，int flags，intstartId)：当应用程序通过startService()的方式启动Service时，会调用该方法。
* IBinder onBind(Intent intent)：当Service通过绑定的方式启动时，会调用该onBind()方法，该方法返回一个IBinder对象，应用程序可以通过IBinder对象与Service通信。
* boolean onUnbind(Intent intent)：当该Service上绑定的所有客户端都断开连接时，会触发该方法。
* void onDestroy(Intent intent)：当Service被销毁时触发该方法。

Service有两类：

**1：本地服务**， Local Service 用于应用程序内部。在Service可以调用Context.startService()启动，调用Context.stopService()结束。 在内部可以调用Service.stopSelf() 或 Service.stopSelfResult()来自己停止。无论调用了多少次startService()，都只需调用一次 stopService()来停止。

**2：远程服务**， Remote Service 用于android系统内部的应用程序之间。可以定义接口并把接口暴露出来，以便其他应用进行操作。客户端建立到服务对象的连接，并通过那个连接来调用服 务。调用Context.bindService()方法建立连接，并启动，以调用 Context.unbindService()关闭连接。多个客户端可以绑定至同一个服务。如果服务此时还没有加载，bindService()会先加 载它。
提供给可被其他应用复用，比如定义一个天气预报服务，提供与其他应用调用即可。使用远程服务需要借助AIDL来进行跨进程通讯。

Service生命周期图一：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-d3b2d957285081da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


通过start方式启动Service，则生命周期函数调用为：
context.startService() ->onCreate()- >onStartCommand()->Service running--调用context.stopService() ->onDestroy()

通过bind方式启动Service:
context.bindService()->onCreate()->onBind()->Service running--调用>onUnbind() -> onDestroy()

Service生命周期图二：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-6e1d2a11ae24320e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Service生命周期图三：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-4d309d8e66189fef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



**同一服务，多次使用start()启动时**

第一次 启动服务时，运行 onCreate -->onStartCommand

后面在启动服务时，服务只执行onStartCommand，不再执行OnCreate

**同一服务A，用任何组件多次bind服务A时**

第一次绑定时会调用onCreate->onBind()。随后无论哪个组件再绑定几次该Service。服务A的onCreate()和onBind()只调用一次。

[回到目录](#index)



<h1 id="SQLite">SQLite</h1>

1.SQLiteOpenHelper

使用SQLite通常要借助SQLiteOpenHelper，SQLiteOpenHelper是Android提供的一个管理数据库的工具类，可用于管理数据库的创建和版本更新。一般的用法是创建SQLiteOpenHelper的子类，并重写它的onCreate(SQLiteDatabase db)和onUpgrade(SQLiteDatabase db,int oldVersion,int newVersion)方法。

SQLiteOpenHelper包含以下常用方法：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-2768e2812924b333.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


一般使用SQLite的方法是，创建一个继承SQLiteOpenHelper的类，重写`onCreate`和`onUpdate` 方法，如下示例所示：

```java
public class MyDbHelper extends SLiteOpenHelper{
  private final String CREATE_TABLE = "create table student(_id Integer primary key auto increment,name ,age)";
  public MyDbHelper(Context context,String name,int version{
      super(context,name,version);
  }
  public void onCreate(SQLiteDatabase db){
      db.execSQL(CREATE_TABLE);
  }
  public void onUpdate(SQLiteDatabase db,int oldVersion,int newVersion){
      //升级时可进行修改表结构等操作
  }
}
```

`onCreate(SQLiteDatabase db)` :用于初次使用应用程序时生成数据库表。当调用getReadableDatabase()和getWritableDatabase()来获取数据库实例时，如果数据库不存在，Android系统会自动生成一个数据库，接着调用onCreate()方法，onCreate()方法只会在初次生成数据库时才被调用。

`onUpdate(SQLiteDatabase db,int oldVersion,int newVersion)` :用于升级软件时更新数据库表结构，此方法在数据库版本发生变化是会被调用，oldVersion代表之前的数据库版本，newVersion代表数据库当前的版本号。SQLiteOpenHelper的构造函数中，有一个指定版本号的参数，只要在创建SQLiteOpenHelper对象时，输入的版本号高于上一个版本号，系统就会自动触发onUpgrade()方法，从而进行表结构更新。

2.SQLiteDatabase

SQLiteDatabase代表一个数据库对象，SQLiteDatabase提供了如下静态方法来打开一个数据库文件：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-26262f5cf5cf6849.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在程序中获取了SQLiteDatabase数据库对象之后，就可以调用SQLiteDatabase的如下方法来操作数据库：

* `execSQL(String sql,Object[] bindArgs)` :执行带占位符的SQL语句。
* `execSQL(String sql )` ：执行指定SQL语句。
* `insert(String table,String nullCulumnHack,ContentValues values)` :向指定表中插入数据。ContentValues是以键值对形式存放数据的。键通常应该是列名。
* `update(String table,ContentValues values,String whereClause,String[] whereArgs)`  ：更新指定表中的特定数据。
* `delete(String table,String whereClause,String[] whereArgs)` ：删除表中满足条件的行。

3.Cursor

Cursor 是查询到的数据条目的集合，相当于JDBC中的ResultSet。Cursor使用如下方法来移动查询结果的记录指针：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-844474b9b0a9abfa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可调用SQLiteDatabase中的如下方法来获取Cursor：

```java
//执行带占位符的SQL查询语句
Cursor rawQuery(String sql,String[] selection);

//执行指定的查询操作。
Cursor query(String table,String[] columns,String whereClause,String[] whereArgs,String groupBy,String having,String orderBy);

//对指定表进行查询，limit参数控制最多查询几条记录。
Cursor query(String table, String[] columns, String selection,String[] selectionArgs, String groupBy, String having,String orderBy, String limit);

//对指定表进行查询，第一个参数用于控制是否去除重复值。
Cursor query(boolean distinct, String table, String[] columns,String selection, String[] selectionArgs, String groupBy,String having, String orderBy, String limit)：

```

4.Transaction

通常以这种形式来使用SQLite的Transaction：

```java
SQLiteDatabase db = dbHelper.getWritableDatabase();
db.beginTransaction();
db.execSQL("........");
db.execSQL("........");
db.execSQL("........");
db.setTransactionSuccessful();
db.endTransaction();
db.close();
```





[回到目录](#index)

<h1 id="Binder机制">Binder机制</h1>

[Linux](http://lib.csdn.net/base/linux)已经拥有的进程间通信IPC手段包括(Internet Process Connection)： 管道（Pipe）、信号（Signal）和跟踪（Trace）、插口（Socket）、报文队列（Message）、共享内存（Share Memory）和信号量（Semaphore）。

而Android采用的是Binder。**Binder基于Client-Server通信模式，传输过程只需一次拷贝，为发送发添加UID/PID身份，既支持实名Binder也支持匿名Binder，安全性高。**

### 1.从进程角度看IPC机制：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-5c1a75aaead4bae9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Linux中，为了保证内核安全，用户空间不能直接操作内核，从而将进程分为**用户空间和内核空间** 。对于用户空间，不同进程之间是不能彼此共享的，而对于内核空间，不同进程是可以共享的。在Binder机制中，Client进程向Server进程通信，本质上就是利用**内核空间可共享的原理** 。

### 2.Binder原理：

Binder通信采用客户端/服务端的架构，Binder定义了四个角色：Server，Client，ServiceManager（简称SMgr）以及Binder驱动。其中Server，Client，SMgr运行于用户空间，驱动运行于内核空间。

### Binder机制包括以下五个部分：

- Binder驱动

  Binder驱动的核心是**维护一个binder_proc类型的链表** 。里面记录了包括ServiceManager在内的所有Client信息，当Client去请求得到某个Service时，Binder驱动就去binder_proc中查找相应的Service返回给Client，同时增加当前Service的引用个数。

- Service Manager

  ​	Service Manager主要**负责管理Android系统中所有的服务** ，当客户端要与服务端进行通信时，首先就会通过Service Manager来查询和取得所需要交互的服务。每个服务需要向Service Manager注册自己提供的服务。

- 服务端

  ​	通常是Android的系统服务，通过Service Manager可以查询和获取到某个Server。

- 客户端

  ​	一般指Android系统上的应用程序，它可以向ServiceManager请求Server中的服务，常见的客户端是Activity。

- 服务代理

  ​	服务代理是指**在客户端应用程序中生成的Server代理** (Proxy)，从应用程序的角度看，代理对象和本地对象没有差别，都可以调用其方法，方法都是同步的，并且返回相应的结果。服务代理也是Binder机制的核心模块。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-99a8f18c1cc572da.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Binder是Android中的一个类，它实现了IBinder接口，从Android应用层来说，Binder是**客户端与服务端进行通信的媒介**(代理)，当bindService的时候，服务端就返回一个包含服务端业务的Binder对象， 通过这个Binder对象，客户端就可以获取**服务端提供的服务或者数据**，这里的服务包括普通服务和基于AIDL的服务。

### 3.Binder通讯流程

![image.png](http://upload-images.jianshu.io/upload_images/4143664-03f10a3be4e9fcc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Binder工作机制

![image.png](http://upload-images.jianshu.io/upload_images/4143664-0eb3b50ec14f27b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### Binder实例分析

一个跨进程调用系统服务的简单例子：

```java
//获取WindowManager服务
WindowManager wm = (WindowManager)getSystemService(getApplicationContext().WINDOW_SERVICE);
//使用LayoutInflater生成一个View对象
View view = LayoutInflater.from(getApplicaiton()).inflate(R.layout.view,null);
//添加iew
wm.addView(view,layoutParams);
```

这个过程分为三个步骤：

* 注册服务：在Android开机启动的过程中，Android会初始化系统地各种Service，并将这些Service向ServiceManager注册（即让ServiceManager管理）。这一步是系统自动完成的。
* 获取服务：客户端想要得到具体的Service直接向ServiceManager要即可。客户端首先向ServiceManager查询得到具体的Service引用，通常是Service引用的代理对象，对数据进行一些处理操作。在`getSystemService()` 过程中得到的wm是WindowManager对象的引用。
* 使用服务：通过这个引用向具体的服务端发送请求，服务端执行完成后就返回。对于WindowManager的`addView`函数，将触发远程调用，调用的是运行在systemServer进程中的WindowManager的addView函数。

**Binder系统架构图**:

![image.png](http://upload-images.jianshu.io/upload_images/4143664-c549f10128eeedb0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**Binder各组件之间的关系：**

![image.png](http://upload-images.jianshu.io/upload_images/4143664-663e10e0f1425df3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


****



[回到目录](#index)

<h1 id="IPC——跨进程通讯">IPC——跨进程通讯</h1>

### Serializable接口

使用方法：

serialVersionUID是用来辅助序列化和反序列化操作的，只有数据中的serialVersionUID和当前类中的serialVersionUID一样，才可以被反序列化。

```java
public class User implements Serializable{
  //声明一个serialVersionUID;
  private static final long serialVersionUID = 123L;

  public int userId;
  public String userName;
  public boolean isMale;
  ....
}
```

序列化：

```java
User user = new User(0,"jake",true);
ObjectOutPutStream out = new ObjectOutputStream(new FileOutputStream("cache.txt"));
out.writeObject(user);
out.close();
```

反序列化：

```java
ObjectInputStream in = new ObjectInputStream(new FileInputStream("cache.txt"));
User newUser = (User)in.readObject();
in.close();
```

### Parcelable接口

使用方法：

```java
public class User implements Parcelable{
  public int userId;
  public String userName;
  public boolean isMale;

  public Book book;//可序列化对象
  public User(int userId,String userName,boolean isMale){
    this.userId = userId;
    this.userName = userName;
    this.isMale = isMale;
  }
  //几乎在大部分情况下都返回0，只有当前对象中存在文件 描述符号时，该方法返回1
  public int describeContents(){
    return 0;
  }

  public void writeToParcel(Parcel out ,int flags){
    out.writeInt(userId);
    out.writeString(userName);
    out.writeInt(isMale?1:0);
    out.writeParcelable(book,0);
  }

  //反序列化
  public static final Parcelable.Creater<User> CREATOR = new Parcelable.Creator<User>() {
    public void createFromParcel(Parcel in){
      return new User(in);
    }
    public User[] newArray(int size){
      return new User[size];
    }
  }

  private User(Parcel in){
    userId = in.readInt();
    userName = in.readString();
    isMale = in.readInt() == 1;
    book = in.readParcelable(Thread.currentThread().getContextClassLoader());
  }
}


```



### 实现IPC的六种方式

简记：**A  B  C  F  S  M**

A--AIDl  B--Bundle  C--ContentProvider

F--File    S--Socket   M--Mesenger

1.使用Bundle

android的四大组件都可使用Bundle传递数据  所以如果要实现四大组件间的进程间通信 完全可以使用Bundle来实现 简单方便  。

优点：**简单易用**

缺点：**只能传输Bundle支持的数据类型**

适用场景：**用于android四大组件间的进程间通信**。

2.文件共享

这种方式在单线程读写的时候比较好用， 如果有多个线程并发读写的话需要限制线程的同步读写 。另外 SharePreference是个特例  它底层基于xml实现  但是系统对它的读写会基于缓存，也就是说再多进程模式下就变得不那么可靠了，有很大几率丢失数据。

优点：**简单易用**

缺点：**不适合高并发场景，并且无法做到进程间的及时通讯**

适用场景：**无并发访问的情形，交换简单的数据，数据实时性要求不高的场景**。

3.AIDL

优点：**功能强大，支持一对多并行通信，支持实时通信**

缺点：**使用稍微复杂，需要处理好线程同步**

适用场景：**一对多通信，远程过程调用（RPC）的场景**

4.Messenger

Messenger 可以翻译为信使，通过该对象，可以在不同的进程中传递Message对象。

客户端向服务端发送消息，可分为以下几步。

服务端

- 创建Service
- 构造Handler对象，实现handlerMessage方法。
- 通过Handler对象构造Messenger信使对象。
- 通过Service的onBind()返回信使中的Binder对象。

客户端

- 创建Actvity
- 绑定服务
- 创建ServiceConnection,监听绑定服务的回调。
- 通过onServiceConnected()方法的参数，构造客户端Messenger对象
- 通过Messenger向服务端发送消息。

实现服务端

```java
public class MessengerService extends Service {
    //构建handler 对象
    public static Handler handler = new Handler(){
        public void handleMessage(android.os.Message msg) {
            // 接受客户端发送的消息
            String msgClient = msg.getData().getString("msg");
            Log.i("messenger","接收到客户端的消息--"+msgClient);
          // 获取客户端Messenger 对象
           Messenger messengetClient = msg.replyTo;

           // 向客户端发送消息
            Message message = Message.obtain();
            Bundle data = new Bundle();
            data.putString("msg", "ccccc");
            message.setData(data);
            try {
                // 发送消息
                messengetClient.send(message);
            } catch (RemoteException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        };
    };
    // 通过handler 构建Mesenger 对象
    private final Messenger messenger = new Messenger(handler);
    @Override
    public IBinder onBind(Intent intent) {
        // 返回binder 对象
        return messenger.getBinder();
    }
}
```

实现客户端

```java
public class MessengerActivity extends AppCompatActivity {
    //Messenger 对象
    private Messenger mService;

   public static Handler handler = new Handler(){
        public void handleMessage(android.os.Message msg) {
            // 接受服务端发送的消息
            String msgService = msg.getData().getString("msg");
        };
    };

 // 通过handler 构建Mesenger 对象
    private final Messenger messengerClient = new Messenger(handler);

    private ServiceConnection conn = new ServiceConnection() {
        public void onServiceConnected(ComponentName name, IBinder service) {
            // IBinder 对象
            // 通过服务端返回的Binder 对象 构造Messenger
            mService = new Messenger(service);
            Log.i("messenger", "客户端以获取服务端Messenger对象");
        }
        public void onServiceDisconnected(ComponentName name) {
        }
    };
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_messenger);
        // 启动服务
        Intent intent = new Intent(this, MessengerService.class);
        bindService(intent, conn, BIND_AUTO_CREATE);
    }
    // 布局文件中添加了一个按钮，点击该按钮的处理方法
    public void send(View view) {
        try {
            // 向服务端发送消息
            Message message = Message.obtain();
            Bundle data = new Bundle();
            data.putString("msg", "lalala");
            message.setData(data);

            // ----- 传入Messenger 对象,这样客户端可以接受服务端传回来的消息
            message.replyTo = messengerClient;

            // 发送消息
            mService.send(message);
            Log.i("messenger","向服务端发送了消息");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

如果需要服务端和客户端能够双向通信，则在每一边都需要有Messenger和Handler，Messenger用来发送消息，在Handler中的handleMessage()中接受Message中的数据。

优点：**功能一般，支持一对多串行通信，支持实时通信**

缺点：**不能很好的处理高并发情形，只能传递Bundle能携带的数据**

适用场景：**低并发的一对多及时通信，无RPC需求**

5.ContentProvider

ContentProvider是Android中提供的，专门用于不同应用间进行数据共享的方式。

创建一个自定义的ContentProvider，继承ContentProvider，并实现六个方法：

```java
public class BookPrivider extends ContentProvider{
  public boolean onCreate(){
    *****
    //返回true则代表BookProvider已成功创建
    return true;
  }
  public String getType(Uri uri){
    ******
  }
  public Cursor query(Uri uri,String[] projection,String selection,String[] selectionArgs,String sortOrder){
    *****
  }
  public int delete(Uri uri,String selection,String[] selectionArgs){
  	****
  }
  public Uri insert(Uri uri,ContentValues values){
    ****
  }
  public int update(Uri uri,ContentValues values,String selection,String[] selectionArgs){


  }
}
```

在AndroidManifest.xml中声明ContentProvider:

android:authorities是ContentProvider的唯一标识，android:permission是要求访问者必须声明的权限，如果没有这个权限，则不能访问。

```java
<provider
  android:name="xx.xxx.BookProvider"
  android:authorities="aaa.bbb.ccc"
  android:permission="rrr.ttt.bbb"
  >
</provider>
```

使用ContentResolver访问ContentProvider共享出来的数据：

```java
***
//这个Uri就是在content后面加上authorities
Uri uri = Uri.parse("content://aaa.bbb.ccc");
ContentResolver resolver = getContentResolver();
resolver.query(uri,null,null,null,null);
****
```

优点：**在数据源访问方面功能强大，支持一对多并发数据共享**

缺点：**这相当于一种受约束的AIDL，主要提供数据源的CRUD操作**

适用场景：**一对多的进程间的数据共享**

6.Socket

服务端：

```java
*****
//使用端口为参数，创建ServerSocket对象
ServerSocket ss = new ServerSocket(12345);
*****
// 获取客户端的Socket 对象
Socket socket = ss.accept();

// 获取输入流  ---
BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
// 通过输入流读取客户端的消息
//String line = br.readLine();
// 输出流
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
// 通过输出流向客户端发送消息
//bw.write("....");

// 关闭连接
socket.close();
```

客户端：

```java
****
Socket s = new Socket("localhost", 12345);
// ----- 和服务端类似
BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
//String line = br.readLine();
// 输出流
BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
//bw.write("....");
// 关闭连接
s.close();
```

优点：**功能强大，可以通过网络传输字节流，支持一对多并发实时通信**

缺点：**实现细节有点繁琐，不支持RPC**

适用场景：**网络数据交换**

[回到目录](#index)

<h1 id="View的绘制流程">View的绘制流程</h1>

![image.png](http://upload-images.jianshu.io/upload_images/4143664-11102394b5e1a704.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




[回到目录](#index)

<h1 id="自定义View和ViewGroup">自定义View和ViewGroup</h1>

一、自定义属性的声明与获取

1.在value目录下创建自定义属性的xml，文件名随意起，比如attr.xml

```java
<?xml version="1.0" encoding="utf-8"?>
<resources>
   <declare-styleable name="CircleView">
      <attr name="circle_color" format="color"/>
  	  <attr name="circle_radius" format="dimension"/>
  	  /*****/
   </declare-styleable>
</resources>
```

2.布局文件中使用自定义属性

```java
...
xmlns:app="http://schemas.android.com/apk.res-auto"
...

<com.XXX.CircleView
    ...
    app:circle_color="@color/light_green"
    app:circle_radius="5dp"
    ...
    />
```

3.在View构造方法中获取自定义属性（使用TypedArray）

```java
int Color mColor;
int radius mRadius;
//*****
public CircleView(Context context, AttributeSet attrs, int defStyleAttr){
   super(context, attrs, defStyleAttr);
     TypedArray a = context.getTheme().obtainStyledAttributes(attrs, R.styleable.CircleRotateView, defStyleAttr, 0);
     //或使用这种方式：TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.CircleView);
  for(int i=0;i<a.getIndexCount();i++) {
     int attr = typedArray.getIndex(i);
     switch(attr){
       case R.styleable.CircleView_circleColor:
         mColor  = a.getColor(attr, Color.BLACK);
         break;
       case R.styleable.CircleView_circleWidth:
         mRadius = a.getDimensionPixelSize(attr, 10);
         break;
         /****/
     }
  }
   a.recycle();
   init();
}
```

二、测量onMeasure()

在onMeasure()中对自定义View的宽高进行测量。

MeasureSpec 代表测量规则，而它的手段则是用一个 int 数值来实现。一个 int 数值有 32 bit。MeasureSpec 将它的高 2 位用来代表测量模式 Mode，低 30 位用来代表数值大小 Size。通过`getMode()` 和 `getSize()` 可以逆向地将一个 measureSpec 数值解析出它的 Mode 和 Size。

3种测量模式：

* **MeasureSpec.EXACTLY** ：此模式说明可以给子元素一个精确的数值。当 layout_width 或者 layout_height 的取值为 **match_parent**  或者 明确的数值如 **100dp**  时，表明这个维度上的测量模式就是 MeasureSpec.EXACTLY。
* **MeasureSpec.AT_MOST** ：该模式下，子 View 希望它的宽或者高由自己决定。ViewGroup 当然要尊重它的要求，但是也有个前提，那就是你不能超过我能提供的最大值，也就是它期望宽高不能超过父类提供的建议宽高。当一个 View 的 layout_width 或者 layout_height 的取值为**wrap_content**  时，它的测量模式就是 MeasureSpec.AT_MOST。
* **MeasureSpec.UNSPECIFIED** ：此种模式表示无限制，子元素告诉父容器它希望它的宽高想要多大就要多大，你不要限制我。一般不需要处理这种情况，在 ScrollView 或者是 AdapterView 中都会处理这样的情况。所以可以在一般情形下忽视它。

```java
public void onMeasure(int widthMeasureSpec,int heightMeasureSpec){
  int resultWidth,resultHeight;
  int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        //测量得到的宽度
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        //测量的到的高度
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
  if(widthMode==MeasureSpec.EXACTLY){
    resultWidth = widthSize;
  }else{
    //这里的MeasureSpec就是MeasureSpec.AT_MOST
    //这里的widthSize就是父控件给的最大的大小，至多不能超过widthSize，这里只要把resultWidth设置为小于widthSize的值就可以。
    resultWidth = widthSize/2;
  }
  //对于resultHeight可以进行类似的处理

  setMeasuredDimension(resultWidth,resultHeight);


}
```

当需要动态改变自定义View的位置或大小时（如改变文本而引起的自定义view变化），应该调用`requestLayout()` 方法。requestLayout()方法会重新对自定义View进行测量和布局。

`requestLayout()` 和`invalidate() ` 的区别如下：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-9b1bbd82cd69d198.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


三、布局onLayout()。（只有自定义ViewGroup需要这一步。自定义View不需要）

1.决定子View的位置

```java
proceted void onLayout(boolean changed,int left,int top,int right,int bottom){
  final int childrenCount = getChildCount();
  for(int i=0;i<childrenCount;i++){
    View childView = getChildAt(i);
     cWidth = childView.getMeasuredWidth();
     cHeight = childView.getMeasuredHeight();
     cParams = (MarginLayoutParams) childView.getLayoutParams();
    if(child.getVisibility()==GONE){
      continue;
    }
    //根据情况去计算childView 的左上角x坐标。
    left = caculateChildLeft();//非原生可调函数
    top = caculateChildTop();//计算childView的左上角的y坐标
    child.layout(left,top,left+cWidth,top+cHeight);
  }


}
```



四、绘制onDraw()

在`onDraw(Canvas canvas)中` 调用canvas的一系列API来绘制View：

```
canvas.drawXXX
```

对Canvas进行变换：平移`translate` ，旋转`rotate` ，缩放`scale` ，倾斜`skew` 等等。

在运用这些变换的时候要注意使用`save()` 来保存犯罪现场，使用`restore()` 来恢复犯罪现场。

onDraw()中不建议进行`new` 操作，这样会减慢速度。

[回到目录](#index)

<h1 id="TouchEvent事件的传递机制">TouchEvent事件的传递机制</h1>

点击事件的分发与以下几个方法相关：

* `public boolean dispathchTouchEvent(MotionEvent event)`
* `public boolean onInterceptTouchEvent(MotionEvent event)`
* `public boolean onTouchEvent(MotionEvent event) `

![image.png](http://upload-images.jianshu.io/upload_images/4143664-d9e79541b3f55fe8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


只有ViewGroup才能对事件进行拦截：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-8867a04c1eab0525.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




如果Activity的所有子View都不处理事件，则最后会调用Activity的onTouchEvent():

![image.png](http://upload-images.jianshu.io/upload_images/4143664-403e54eb89afd852.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果在最底层View处理：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-6523dd3491214c42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果在中途View处理了该事件，则不会继续向下进行事件分发：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-057a4d507a2a696e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


当一个View需要处理某个事件时，如果设置了OnTouchListener，则OnTouchListener中的`onTouch()` 方法会先调用，返回true则代表处理完成，就不会再交给`onTouchEvent()` 了。如果在`onTouch()`中返回false，表示没有处理该事件，则才会调用`onTouchEvent()` ，在onTouchEvent()中，如果设置有OnClickListener，那么它的`onClick()` 会被调用。优先级：onTouchListener-->onTouchEvent()-->OnClickListener。

**同一事件序列只能被一个View拦截并消耗** 。一个事件序列是指从手指触碰到屏幕的一瞬间，到手指离开屏幕的那一段时间，在这个过程中产生的一系列事件，这个事件序列以`ACTION_DOWN` 开始，中间含有若干的`ACTION_MOVE` 事件，最终以`ACTION_UP` 事件结束。

某个View一旦决定拦截事件，那么一个事件序列都只能由它处理，并且后续它的`onInterceptTouchEvent()` 将不会被调用。因为一旦某个View决定要拦截，系统不会再次问它是否要拦截了，即`onInterceptTouchEvent()` 不再调用。

某个View一旦开始处理事件，如果他不消耗ACTION_DOWN事件(`onTouchEvent()` 返回false)，那么同一事件序列中的其他事件都不会交给它处理，而是重新交给它的父元素去处理，即父View的`onTouchEvent()` 会被调用。

事件传递过程：事件最先传递到Activity，Activity会把事件分发的具体工作交给**PhoneWindow** 来做(PhoneWindow是用来控制顶级View的外观和行为策略的)，而PhoneWindow又将事件传递给了顶级的**DecorView** （DecorView继承自FrameLayout），然后这个DecorView会将事件进行向下分发，分发给其子ViewGroup和View等，然后在子ViewGroup中会进行`dispatchTouchEvent()`依次往下分发， --> 判断是否拦截 `onInterceptTouchEvent()` -->如果拦截则处理:`onTouchEvent()` ，如果不拦截则继续往下分发。

**最底层的View没有 `dispatchTouchEvent()` 方法，它只可以选择对事件进行处理或者不处理** 。

[回到目录](#index)

<h1 id="Android中的三种动画">Android中的三种动画</h1>

### 逐帧动画

逐帧动画，把动画过程的每张静态图片都收集起来，然后由Android来控制依次展示这些静态图片，再利用人眼“视觉暂留”的原理，给用户以“动画”的感觉。实际就是一帧接着一帧的播放图片，就像放电影一样。

逐帧动画放在/drawable文件夹下，逐帧动画的语法：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-91691e2c926b2603.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


每一个`<item>` 都对应着一帧。

使用示例：

先在imageView的xml中指定逐帧动画：

```java
<ImageView
  android:id="@+id/iv"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:background="@drawable/anim.xml"/>
```

然后在java代码中使用：

```java
AnimationDrawable anim = (AnimationDrawable)imageView.getBackground();

//开始播放
anim.start();
//停止播放
anim.stop();
```

### 补间动画

补间动画就是，开发者只需指定动画开始、动画结束等“关键帧”，而动画变化的“中间帧”由系统计算并补齐。补间动画是操作某个控件让其展现出旋转、渐变、移动、缩放等转换过程。我们可以用XML形式定义动画，也可以编码实现。

补间动画放在res/anim文件夹下(需要手动创建)。

补间动画语法：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-adf99786d2adaad0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


使用示例：

```java
//先加载。my_anim是补间动画名称
Animation anim = AnimationUtils.loadAniation(context,R.anim.my_anim);
anim.setFillAfter(true);
//使用ImageView播放
imageView.startAnimation(anim);

```



### 属性动画

属性动画是增强版的补间动画，属性动画的强大可以体现在如下两个方面：1.补间动画只能定义两个关键帧在透明度，旋转，缩放，位移四个方面的变化，但属性动画可以令对象进行更多特征变化。2.补间动画只能对UI组件执行动画，但属性动画几乎可以对任何对象执行动画（不管它是否显示在屏幕上）。

1.Animator

Animator是属性动画的基类，是一个抽象类，该类中定义了许多重要方法，如下所示：

* setDuration(long duration)：设置动画总共的持续时间，以毫秒为单位。
* start()：通过start方法可以启动动画，动画启动后不一定会立即运行。如果之前通过调用setStartDelay方法设置了动画延迟时间，那么会在经过延迟时间之后再运行动画；如果没有设置过动画的延迟时间，那么动画在调用了start()方法之后会立即运行。
* setStartDelay(long startDelay)：设置动画的延迟运行时间，比如调用setStartDelay(1000)意味着动画在执行了start()方法1秒之后才真正运行。
* setInterpolator(TimeInterpolator value)：改变动画所使用的时间插值器，由于视图动画也需要使用时间插值器，所以我们可以使用android.view.animation命名空间下的一系列插值器，将其与属性动画一起工作。
* pause()：该方法可以暂停动画的执行。调用pause()方法的线程必须与调用start()方法的线程是同一个线程。
* resume()：如果动画通过调用pause()方法暂停了，那么之后可以通过调用resume()方法让动画从上次暂停的地方继续运行。
* end()：动画结束运行，直接从当前状态跳转到最终的完成状态，并将属性值分配成动画的终止值
* addListener (Animator.AnimatorListener listener)：通过addListener方法向Animator添加动画监听器，该方法接收的是AnimatorListener接口类型的参数，其具有四个方法：onAnimationStart、onAnimationCancel、onAnimationEnd、onAnimationRepeat。这四个方法分别在动画被启动，被取消，结束和重复播放时被回调。

2.ValueAnimator

ValueAnimator是属性动画的“时间引擎”，它负责计算各个帧的属性值。中有两个比较重要的属性，一个是TimeInterpolator类型的属性，另一个是TypeEvaluator类型的属性。TimeInterpolator指的就是时间插值器，TypeEvaluator表示的是ValueAnimator对哪种类型的值进行动画处理。	ValueAnimator提供了四个静态方法ofFloat()、ofInt()、ofArgb()和ofObject()，通过这四个方法可以对不同种类型的值进行动画处理，这四个方法对应了四种TypeEvaluator。

* public static ValueAnimator ofFloat (float… values)：ofFloat方法接收一系列的float类型的值，其内部使用了FloatEvaluator。通过该方法ValueAnimator可以对float值进行动画渐变。

ValueAnimator使用示例：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-47941f9041bfdf99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* public static ValueAnimator ofInt (int… values)：ofInt方法与ofFloat方法很类似，只不过ofInt方法接收int类型的值，ofInt方法内部使用了IntEvaluator，其使用可参考上面ofFloat的使用代码。
* public static ValueAnimator ofArgb (int… values)：该方法接收一系列代表了颜色的int值，其内部使用了ArgbEvaluator，可以用该方法实现将一个颜色动画渐变到另一个颜色，我们从中可以不断获取中间动画产生的颜色值。
* public static ValueAnimator ofObject (TypeEvaluator evaluator, Object… values):ValueAnimator提供了一个ofObject方法，该方法接收一个TypeEvaluator类型的参数，我们需要实现该接口TypeEvaluator的evaluate()方法，只要我们实现了TypeEvaluator接口，我们就能通过ofObject方法处理任意类型的数据。

  3.ObjectAnimator

  ObjectAnimator继承自ValueAnimator。要让属性动画渐变式地更改对象中某个属性的值，可分两步操作：第一步，动画需要计算出某一时刻属性值应该是多少；第二步，需要将计算出的属性值赋值给动画的属性。

ValueAnimator只实现了第一步，也就是说ValueAnimator只负责以动画的形式不断计算不同时刻的属性值，但需要我们开发者自己写代码在动画监听器AnimatorUpdateListener的onAnimationUpdate方法中将计算出的值通过对象的setXXX等方法更新对象的属性值。ObjectAnimator比ValueAnimator更进一步它会自动调用对象的setXXX方法更新对象中的属性值。

ObjectAnimator中常用的方法有：

* ofFloat(Object target, String propertyName, float… values)：target是指要操作的对象，propertyName是指属性名，常见的属性名有：“translationX”(表示横向变换)，“translationY”(纵向变换),”alpha”(变换透明度),”rotation”(旋转变换),”backgroundColor”(表示背景色变换)等等。
* ofInt(Object target, String propertyName, int… values)：与float作用相同，只是数据类型的差别。ofObject(Object target, String propertyName, TypeEvaluator evaluator, Object… values) ：与ValueAnimator 中的ofObject类似，都需要传入自定义的TypeEvaluator对象。
* ofObject(Object target, String propertyName, TypeEvaluator evaluator, Object… values) ：与ValueAnimator 中的ofObject类似，都需要传入自定义的TypeEvaluator对象。
* ofArgb(Object target, String propertyName, int… values)：用该方法实现将一个颜色动画渐变到另一个颜色，我们从中可以不断获取中间动画产生的颜色值。

使用示例：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-568d575f19564e98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


4.AnimatorSet

AnimatorSet继承自Animator。AnimatorSet表示的是动画的集合，我们可以通过AnimatorSet把多个动画集合在一起，让其串行或并行执行，从而创造出复杂的动画效果。

假设anim1,anim2,anim3,anim4是四个已定义的ObjectAnimator动画对象，AnimatorSet可以使用三种方式进行动画组合()：

```java
//方式一
  AnimatorSet animatorSet = new AnimatorSet();
  animatorSet.playSequentially(anim1, anim2, anim4);
  animatorSet.playTogether(anim2, anim3);
  animatorSet.start();
//方式二
AnimatorSet anim23 = new AnimatorSet();
  anim23.playTogether(anim2, anim3);
  AnimatorSet animatorSet = new AnimatorSet();
  animatorSet.playSequentially(anim1, anim23, anim4);
  animatorSet.start();
//方式三
 AnimatorSet animatorSet = new AnimatorSet();
  animatorSet.play(anim1).before(anim2);
  animatorSet.play(anim2).with(anim3);
  animatorSet.play(anim4).after(anim2);
  animatorSet.start();
```

[回到目录](#index)

<h1 id="滑动冲突的解决方法">滑动冲突的解决方法</h1>

滑动冲突分为三类：

* 外部滑动和内部滑动方向不一致：判断是水平滑动还是竖直滑动，根据滑动的方向将滑动事件交给不同的View。
* 外部滑动和内部滑动方向一致：根据业务逻辑或某个状态进行判断，决定点击事件的分发
* 前两种的嵌套：根据业务逻辑或某个状态进行判断，决定点击事件的分发

### 滑动冲突的解决方法：

1.外部拦截法

外部拦截法是指父容器进行拦截处理，我们需要重写父容器的`onInterceptTouchEvent()` 方法 。点击事件首先会到达父容器，触发父容器的`onInterceptTouchEvent()` 方法，如果父容器需要拦截点击事件，则返回true，然后进入到`onTouchEvent()` 中进行处理，需要注意：父容器不能在ACTION_DOWN中返回true，否则这之后的事件序列都会交给它处理，父容器必须在ACTION_DOWN中返回false。在ACTION_MOVE中，父容器进行判断，如果自己需要拦截事件，则返回true，触发自己的`OnTouchEvent()` 方法，否则返回false。在ACTION_UP中，应该返回false。因为ACTION_UP反正也是一个事件序列的末尾了。

外部拦截法的伪代码如下：

```java
public boolean onInterceptTouchEvent(MotionEvent ev) {
    boolean intercepted = false;
    switch (ev.getAction()){
        case MotionEvent.ACTION_DOWN:
            intercepted = false;
            break;
        case MotionEvent.ACTION_MOVE:
            if(父控件需要处理){
                intercepted = true;
            } else{
                intercepted = false;
            }
            break;
        case MotionEvent.ACTION_UP:
            intercepted = false;
            break;
    }

    return intercepted;
}
```

2.内部拦截法

内部拦截法指的是父容器不拦截任何事件，所有事件全部传递给子元素，如果子元素需要就进行消耗，否则交由父容器进行处理。这种方式需要配合ViewGroup的**FLAG_DISALLOW_INTERCEPT** 标志位来使用。设置此标志为可以通过`requestDisallowInterceptTouchEvent()` 函数来设置，如果设置了此标志位，那么ViewGroup就无法拦截除了ACTION_DOWN之外的任何事件。这样首先我们保证ViewGroup的onInterceptTouchEvent方法除了DOWN其他都返回true，DOWN返回false，这样保证了不会拦截DOWN事件，交给它的子View进行处理；重写子View的`dispatchTouchEvent()`函数，在DOWN中设置`parent.requestDisallowInterceptTouchEvent(true)` ，这样父控件在默认的情况下DOWN之后的所有事件它都拦截不到，交由子View来处理，View在MOVE中判断父控件需要时，调用`parent.requestDisallow InterceptTouchEvent(false)` ，这样父控件的拦截又起作用了，相应的事件交给了父控件进行处理。伪代码如下：

父控件中：

```java
@Override
public boolean onInterceptTouchEvent(MotionEvent ev) {
    int action = ev.getAction();
    if(action == MotionEvent.ACTION_DOWN){
        return false;
    } else {
        return true;
    }
}
```

子View中：

```java
@Override
public boolean dispatchTouchEvent(MotionEvent ev) {
    switch (ev.getAction()){
        case MotionEvent.ACTION_DOWN:           getParent().requestDisallowInterceptTouchEvent(true);
            break;
        case MotionEvent.ACTION_MOVE:
            if(父控件需要此点击事件){                getParent().requestDisallowInterceptTouchEvent(false);
            }
            break;
        case MotionEvent.ACTION_UP:
            break;
    }
}
```



 [回到目录](#index)

<h1 id="AIDL的使用">AIDL的使用</h1>

Android Interface Defining Language，Android接口定义语言。引入AIDL目的是为了实现进程间通信，尤其是在涉及多进程并发情况下的进程间通信。

使用AIDL：

1.在xxx.aidl文件中定义AIDL接口（在module目录下创建aidl文件夹，然后将xxx.aidl文件放在这个文件夹）

```java
interface IMessage{
  String getMessage();
  ***
}
```



2.把项目Rebuild之后，Android Studio会自动为AIDL接口生成一个接口IMessage，该接口继承IInterface，IMessage接口中有一个抽象类Stub，该Stub继承了Binder，并实现了IMessage。这样就可以在Java代码中使用IMessage。

```java
public interface IMessage extends android.os.IInterface{
  *****
  //抽象类Stub
  public static abstract class Stub extends android.os.Binder implements IMessage{
    *****
    public boolean onTransact(int code,Parcel data,Parcel reply,int flags){
      //AIDL接口中的方法是由这个int型的code来标识的
      switch(code){
        case TRANSACTION_getMessage:{
          ****
            break;
        }
        case ****
          ****
          break;
      }
    }
    *****
  }

  //代理类Proxy
  private static class Proxy implements IMessage{
    private android.os.IBinder mRemote;
    ****
    //adBinder()返回的是代理对象mRemote
    public android.os.IBinder asBinder(){
      return mRemote;
    }
  }
  public String getMessage(){
    *****
  }
}
```

3.自定义Service（AIDL一般用来调用远程服务）

```java
public class RemoteService extends Service{
  private IMessage.Stub stub = new IMessage.Stub{
    //这里写的是getMessage的具体实现
    public String getMessage(){
      return "Hello Kotlin";
    }
    public void onCreate(){
    	****
    }
    public int onStartCommand(Intent intent,int flags,int startId ){
      ****
    }
    ****
  }
}
```

4.使用RemoteService

```java
****
private IMessage serv;
//创建ServiceConnection对象
ServiceConnection conn = new ServiceConnection(){
  public void onServiceConnected(ComponentName name,IBinder  service){
    serv = IMessage.Stub.adInterface(service);
    String msg = serv.getMessage();
  }
  public void onServiceDisconnected(ComponentName name){
    ****
  }

 *****
  //在绑定RemoteService的地方
  Intent intent = new Intent();
  intent.setAction("aa.aa.aaaa");
  intent.setPackage("xx.xx.xx");
  bindService(intent,conn,BIND_AUTO_CREATE);
}
```

[回到目录](#index)


<h1 id="应用程序Activity的启动过程">应用程序Activity的启动过程</h1>

有两种操作会引发Activity的启动，

* 一种是用户点击应用程序图标时，Launcher会为我们启动应用程序的主Activity；应用程序的默认Activity启动起来后，它又可以在内部通过调用startActvity接口启动新的Activity，依此类推，每一个Activity都可以在内部启动新的Activity。通过这种连锁反应，按需启动Activity，从而完成应用程序的功能。
* 通过startActivity的形式启动另一个Activity

无论是通过哪种方式启动，要借助于应用程序框架层的**ActivityManagerService**服务进程。而Service也是由ActivityManagerService进程来启动的。

**在Android应用程序框架层中，ActivityManagerService是一个非常重要的接口，它不但负责启动Activity和Service，还负责管理Activity和Service。**

Android应用程序框架层中的ActivityManagerService启动Activity的过程大致如下图所示：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-e9ff05352fc980c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


ActivityManagerService和ActivityStack位于同一个进程中，而ApplicationThread和ActivityThread位于另一个进程中。其中，

1. **ActivityManagerService是负责管理Activity的生命周期的**。
2. ActivityManagerService还借助ActivityStack是来把所有的Activity按照后进先出的顺序放在一个任务栈中。
3. **对于每一个应用程序来说，都有一个ActivityThread来表示应用程序的主进程**，而每一个ActivityThread都包含有一个ApplicationThread实例，它是一个Binder对象，负责和其它进程进行通信。

Activity的启动过程是：

1. 无论是通过Launcher来启动Activity，还是通过Activity内部调用startActivity接口来启动新的Activity，都通过Binder进程间通信进入到ActivityManagerService进程中，并且调用ActivityManagerService.startActivity接口；
2. ActivityManagerService调用ActivityStack.startActivityMayWait来做准备要启动的Activity的相关信息；
3. ActivityStack通知ApplicationThread要进行Activity启动调度了，这里的ApplicationThread代表的是调用ActivityManagerService.startActivity接口的进程，对于通过点击应用程序图标的情景来说，这个进程就是Launcher了，而对于通过在Activity内部调用startActivity的情景来说，这个进程就是这个Activity所在的进程了；
4. ApplicationThread不执行真正的启动操作，它通过调用ActivityManagerService.activityPaused接口进入到ActivityManagerService进程中，看看是否需要创建新的进程来启动Activity；
5. 对于通过点击应用程序图标来启动Activity的情景来说，ActivityManagerService在这一步中，会调用startProcessLocked来创建一个新的进程，而对于通过在Activity内部调用startActivity来启动新的Activity来说，这一步是不需要执行的，因为新的Activity就在原来的Activity所在的进程中进行启动；
6. ActivityManagerServic调用ApplicationThread.scheduleLaunchActivity接口，通知相应的进程执行启动Activity的操作；
7. ApplicationThread把这个启动Activity的操作转发给ActivityThread，ActivityThread通过ClassLoader导入相应的Activity类，然后把它启动起来。


[回到目录](#index)





<h1 id="内存泄露以及优化">内存泄露以及优化</h1>

内存泄露：当一个对象已经不需要再使用了，本该被回收时，而有另外一个正在使用的对象持有它的引用从而导致它不能被回收，这导致本该被回收的对象不能被回收而停留在堆内存中，这就产生了内存泄漏。

### Android系统内存分配与回收

* 一个APP就是一个Linux进程，同时也是一个JVM中的进程。Andorid中的APP进程都是由Linux的**Zygote进程** fork出来的。
* GC(垃圾回收器)只有在Heap剩余空间不够的时候才发出垃圾回收。引入GC的优点就是不需要我们手动的进行垃圾回收。但是GC的缺点就是，APP在运行过程中不断的申请内存分配，当发现系统分配给我们的Heap不够用了，然后才进行垃圾回收。
* GC触发时，所有的线程都会被暂停。

APP的内存限制机制：

* 系统会默认给每个APP设置一个最大的内存限制，随不同设备而不同。（通常会是256M）
* 吃内存大户：图片。

切换应用时后台APP清理机制：

* Android是使用分时复用的方式，可以多个应用同时运行，在前台与用户交互的只有一个，在后台运行的可以有多个。APP切换时是采用**LRU** (Least recently used,最近最少使用) 方式，最近使用的排在最前面，被清理的可能性最小。排在末尾的最有可能被清理。
* 当系统清理后台进程时，会触发Activity 的 `onTrimMemory()` 方法。

### APP内存优化方法

1.对于生命周期比Activity长的对象如果需要应该使用ApplicationContext。

2.对于需要在静态内部类中使用**非静态外部成员变量** （如：Context、View )，可以在静态内部类中使用**弱引用** 来引用外部类的变量来避免内存泄漏。

3.对于不再需要使用的对象，显示的将其赋值为null，比如使用完Bitmap后先调用recycle()，再赋为null。

4..保持对对象生命周期的敏感，特别注意单例、静态对象、全局性集合等的生命周期。

5.对于生命周期比Activity长的内部类对象，并且内部类中使用了外部类的成员变量，可以这样做避免内存泄漏：

* 将内部类改为静态内部类
* 静态内部类中使用弱引用来引用外部类的成员变量

6.数据结构优化：(1)频繁地拼接字符串应该使用StringBuilder，而不应该采用+的方式（该方式会产生很多的中间字符串）。(2)使用ArrayMap ,SparseArray替换HashMap，效率更高。(3)内存抖动：由于变量使用不当，在短时间内申请了大量的内存，用完后便马上弃之不用。过了一会又去申请大量内存，用完再弃之不用。应该尽量避免内存抖动。(4)注意：再小的Class都会耗费0.5KB。HashMap里一个entry需要额外占用32B。

7.对象复用：(1)复用系统自带的资源。(2)ListView/GridView的ConvertView复用。(3)避免在onDraw()里面执行对象的创建。

8.避免内存泄露：内存泄露会导致剩余可用的Heap越来越少，频繁触发GC。(1)在使用Context的地方，尽量用Applicaion Context而不是 Activity Context。(2)在执行数据库相关操作时，将Cursor对象及时关闭。(3)Bitmap的在不用的时候应及时调用`recycle()`。

### OnTrimMemory()

Activity的`onTrimMemory(int level)` ，level有以下几个常量值：

* TRIM_MEMORY_BACKGROUND：表示该APP当前是处于后台，已经被放在了LRU队列中。
* TRIM_MEMORY_COMPLETE：表示该应用已经处于LRU队列的末端，如果手机内存紧张的时候就会被清除掉。这是最危险的级别。
* TRIM_MEMORY_MODERATE：表示当前应用处于LRU的中部，内存紧张时还暂时不会KILL该进程。
* TRIM_MEMORY_RUNNING_MODERATE：表示应用程序正常运行，并且不会被杀掉。但是目前手机的内存已经有点低了，系统可能会开始根据LRU缓存规则来去杀死进程了。
* TRIM_MEMORY_RUNNING_LOW ：表示应用程序正常运行，并且不会被杀掉。但是目前手机的内存已经非常低了，我们应该去释放掉一些不必要的资源以提升系统的性能，同时这也会直接影响到我们应用程序的性能。
* TRIM_MEMORY_RUNNING_CRITICAL：表示应用程序仍然正常运行，但是系统已经根据LRU缓存规则杀掉了大部分缓存的进程了。这个时候我们应当尽可能地去释放任何不必要的资源，不然的话系统可能会继续杀掉所有缓存中的进程，并且开始杀掉一些本来应当保持运行的进程，比如说后台运行的服务。
* TRIM_MEMORY_UI_HIDDEN：点击了Home键之后，会收到这个级别的通知。





<h1 id="Handler原理">Handler原理</h1>

### ThreadLocal

ThreadLocal是一个**线程内部的数据存储类** ，实质上是一个泛型类，定义为：public class ThreadLocal<T>。通过它可以在某个指定线程中存储数据，数据存储以后，只有在**指定线程(存储数据的线程)** 中可以获取到它存储的数据，对于其他的线程来说无法获取到它的数据。

通过使用ThreadLocal，能够让同一个数据对象在不同的线程中存在多个副本，而这些副本互不影响。Looper的实现中便使用到了ThreadLocal。通过使用ThreadLocal，每个线程都有自己的Looper，它们是同一个数据对象的不同副本，并且不会相互影响。

ThreadLocal中有一个内部类ThreadLocalMap，ThreadLocal中有一个内部类Entry，Entry中的`Object value `这个value实际上就是每一个线程中的数据副本。ThreadLocalMap中有一个存放Entry的数组：`Entry[] table`。 ThreadLocal类的部分代码如下：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-db1a4ebe6460dbe3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


ThreadLocal的`set` 方法：实际上就是往ThreadLocalMap对象(map)维护的对象数组table中插入数据。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-35ad6771977585e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


ThreadLocal的`get` 方法，调用了ThreadLocalMap的getEntry()方法：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-5e5d85fe37f8571f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


ThreadLocalMap的`getEntry()` 方法:

![image.png](http://upload-images.jianshu.io/upload_images/4143664-e2d84bb835c90205.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


i的值是由线程的哈希码和（table的长度-1）进行“按位与”运算，所有每个线程得到的i是不一样的，因此最终数据副本在table中的位置也不一样。

### MessageQueue

MessageQueue主要包含两个操作，插入和读取。读取操作的函数是`next()` ，该操作同时也会伴随着删除操作(相当于出队列)，插入操作对应的函数是`enqueueMessage()` ，`enqueueMessage()` 实际上就是单链表的插入操作。`next()` 方法是一个无限循环的方法，如果消息队列中没有消息，那么next()方法会一直阻塞。当有新消息到来时，next()方法会返回这条消息并将其从单链表中移除。

### Looper

Looper在Android的消息机制中扮演着消息循环的角色，它会不停地从MessageQueue中查看是否有新的Message到来，如果有新消息就会立刻处理，否则就一直阻塞在那里。一个线程只能有一个Looper对象，从而也只有一个MessageQueue(在Looper的构造方法初始化)。

Looper中的几个重要的成员变量：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-5c261017b760ee4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Looper的构造方法，在构造方法中，创建了一个**MessageQueue** 实例：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-77da9ca8a0784b14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


当需要为一个线程创建Looper对象时，需要调用Looper的`prepare()` 方法（该方法在一个线程中只能调用一次，否则会抛出异常）：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-ef2fa558dea89459.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在`loop()` 的消息循环中，实际上是调用了MessageQueue的**next()** 方法。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-1cc5887a85ec0902.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/4143664-25d050169cc47b9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Looper主要作用：
1、	与当前线程绑定，保证一个线程只会有一个Looper实例，同时一个Looper实例也只有一个MessageQueue。
2、	loop()方法，不断从MessageQueue中去取消息，交给消息的target属性的dispatchMessage去处理。

### Handler

Handler的工作主要是消息的发送和消息接收处理。消息的发送可以通过Handler的`post()` 方法或者`sendMessage()` 方法来实现，消息的处理，需要我们重写handleMessage()函数来进行处理。

Handler的sendMessage()函数：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-96245925d6a7f74a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/4143664-30c0909c09a9e66b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/4143664-28afde61566ded9a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


最后调用了MessageQueue的`enqueueMessage()` 函数：

![image.png](http://upload-images.jianshu.io/upload_images/4143664-26b87b7548a39bfd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/4143664-3b7d2c3b782f8645.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Message 的callback成员变量实际上是一个**Runnable对象** ：

`Runnable callback; `

经常使用的Handler的`post(Runnable r)` 方法，源码是这样的：

```java

    public final boolean post(Runnable r)
    {
       return  sendMessageDelayed(getPostMessage(r), 0);
    }
```

其中，`getPostMessage(r) ` 为：

```java
private static Message getPostMessage(Runnable r) {
        Message m = Message.obtain();
        m.callback = r;
        return m;
    }
```

原来，Handler的`post()`方法实际上是把这个Runnable对象封装到了一个Message中的。

因此，Handler中的事件处理优先级顺序是：

Message.callback(Runnable) -- >  mCallback(Callback接口实现类或Callback匿名内部类)  --->  Handler或其子类的handleMessage()。





[回到目录](#index)

<h1 id="JVM/Davik/ART">JVM/Davik/ART</h1>
### JVM
JAVA程序的执行过程：
* 首先，.java文件经过Java编译器，被编译成字节码文件（.class文件），Java编译器在这个编译过程中做的任务有：语法分析、语义分析、字节码生成等。
* 字节码文件在执行前，先经过类加载机制（加载、连接、初始化）
* 在执行时，字节码文件经过Java虚拟机来执行，Java虚拟机在这个过程中做的工作有：为代码进行机器相关优化和非机器相关的优化，寄存器分配，生成目标代码。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-bc0af1a971628389.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


当启动一个java程序时，一个虚拟机实例就诞生了，当程序结束时，这个虚拟机实例也消亡。

Java虚拟机包含一个类加载器（class loader），它可以从程序和API中加载class文件，Java API中只有程序执行时需要的类才会加载。
![image.png](http://upload-images.jianshu.io/upload_images/4143664-8607520b08b80dc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



Java虚拟机数据类型：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-d1df36f64aa533d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

包括两类数据类型：
* 基本类型：char ,byte,short,int ,long,float,double,boolean,还有一个**returnAddress** ，returnAddress 数据类型被用来实现Java程序中的finally 字句，它的值执行一条虚拟机指令的操作码。比如，在一段代码中，一个`try` 包含了两个`catch` 块，returnAddress是用来标记fanally块执行完后返回的位置，如果后面不使用returnAddress，则finally块执行完就不知道要返回到哪里了。
* 引用类型：类类型，接口类型，数组类型。

#### JVM体系结构：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-b7ae894eea396724.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.class文件：包含了关于类或接口的所有信息，class文件的基本类型如下：
u1 ：代表1个字节，无符号类型
u2 ：代表2个字节，无符号类型
u4 ：代表4个字节，无符号类型
u8 ：代表8个字节，无符号类型

class文件内容包括：
```
ClassFile {

    u4 magic;                                     //魔数：0xCAFEBABE，用来判断是否是Java class文件
    u2 minor_version;                             //次版本号
    u2 major_version;                             //主版本号
    u2 constant_pool_count;                       //常量池大小
    cp_info constant_pool[constant_pool_count-1]; //常量池
    u2 access_flags;                              //类和接口层次的访问标志（通过|运算得到）
    u2 this_class;                                //类索引（指向常量池中的类常量）
    u2 super_class;                               //父类索引（指向常量池中的类常量）
    u2 interfaces_count;                          //接口索引计数器
    u2 interfaces[interfaces_count];              //接口索引集合
    u2 fields_count;                              //字段数量计数器
    field_info fields[fields_count];              //字段表集合
    u2 methods_count;                             //方法数量计数器
    method_info methods[methods_count];           //方法表集合
    u2 attributes_count;                          //属性个数
    attribute_info attributes[attributes_count];  //属性表

}
```

2.类加载器子系统包括四种加载器：
* Bootstrap ClassLoader 启动类加载器
* Extension ClassLoader 扩展类加载器
* Application ClassLoader 应用程序类加载器
* User ClassLoader 自定义类加载器

加载：将类的class文件读入内存中，并为之创建一个java.lang.Class对象
连接：负责把类的二进制数据合并到JRE
初始化：主要是对类变量的初始化。

3.方法区（常量池是在方法区里的！！）
JVM将关于类/接口的一系列信息都存放在了方法区里，方法区中存放了以下信息：
* 类/接口的全限定名（如java.lang.Object这样的）
* 类的直接父类的全限定名
* 这个class文件是**类**还是**接口**
* 访问修饰符
* 常量池：包括直接常量(字符串，整型，和浮点常量等)和对其他类型、字段方法的符号引用。
* 字段信息（字段名，类型，修饰符）
* 方法信息（方法名，修饰符，返回类型，参数数量和类型）
* 除了常量以外的所有类的静态变量


4.堆
Java程序在运行时创建的所有类实例或数组，都放在同一个堆中，由于一个进程（也就是一个JVM实例）只有一个堆空间，因此所有线程共用这个堆空间。Java虚拟机中只有一条在堆中分配对象的指令，却没有释放内存的指令，因为这正是垃圾回收器需要做的事情：回收内存。
（1）存储的全部是对象的实例，每个对象都包含一个与之对应的class的信息。（class的目的是得到操作指令）。
（2）每个jvm实例只有一个堆区（heap）被所有线程共享，堆中不存放基本类型和引用型变量，只存放**对象实例**本身。

5.栈
每当启动一个线程，JVM就会给它分配一个Java栈，Java栈由许多栈帧组成，当线程调用一个Java方法时，虚拟机压入新的栈帧到该线程的Java栈中，当方法返回时，这个栈帧就从Java栈中弹出来。Java虚拟机没有寄存器，其指令集使用Java栈来存储中间数据，这样设计的原因是为了保持Java虚拟机的指令集尽量紧凑。


6.程序记数器
每一个线程都有它的程序计数器，程序计数器也叫PC寄存器，程序计数器既能持有一个本地指针，也能持有一个returnAddress。当线程执行某个Java方法时，程序计数器的值总是下一条被执行指令的地址。这里的地址可以是一个本地指针，也可以是方法字节码中相对该方法起始指令的偏移量。如果该线程正在执行一个本地方法，那么此时程序计数器的值是“undefined”。


7.本地方法栈
任何本地方法接口都会使用某种本地方法栈。当线程调用Java方法时，虚拟机会创建一个新的栈帧并压入Java栈。当它调用的是本地方法（native method）时，虚拟机会保持Java栈不变，不再在线程的Java栈中压入新的栈帧，JVM只是简单地动态连接并直接调用指定的本地方法。

其中方法区和堆由该虚拟机实例中所有线程共享。当虚拟机装载一个class文件时，它会从这个class文件包含的二进制数据中解析类型信息，然后把这些类型信息放到方法区。当程序运行时，虚拟机会把所有该程序在运行时创建的对象放到堆中。

像其它运行时内存区一样，本地方法栈占用的内存区可以根据需要动态扩展或收缩。

#### 执行引擎
在Java虚拟机规范中，执行引擎的行为使用指令集定义。实现执行引擎的设计者将决定如何执行字节码，实现可以采取解释、即时编译或直接使用芯片上的指令执行，还可以是它们的混合。

执行引擎可以理解成一个抽象的规范、一个具体的实现或一个正在运行的实例。抽象规范使用指令集规定了执行引擎的行为。具体实现可能使用多种不同的技术--包括软件方面、硬件方面或树种技术的结合。作为运行时实例的执行引擎就是一个线程。

运行中Java程序的每一个线程都是一个独立的虚拟机执行引擎的实例。从线程生命周期的开始到结束，它要么在执行字节码，要么执行本地方法。

#### 本地方法接口
Java本地接口，也叫JNI（Java Native Interface），是为可移植性准备的。本地方法接口允许本地方法完成以下工作：
* 传递或返回数据
* 操作实例变量
* 操作类变量或调用类方法
* 操作数组
* 对堆的对象加锁
* 装载新的类
* 抛出异常
* 捕获本地方法调用Java方法抛出的异常
* 捕获虚拟机抛出的异步异常
* 指示垃圾收集器某个对象不再需要


### Dalvik 虚拟机
#### 与JVM相比，Dalvik虚拟机有如下特点：
* JVM基于栈，Dalvik基于寄存器。（基于寄存器的虚拟机在执行程序的时候，花费的时间更短）
* JVM执行的是Java字节码(.class文件)，Dalvik虚拟机执行的是.dex字节码文件。（由dx工具将class文件转换成.dex文件，dex文件格式可以减少整体文件尺寸，加快IO读写速度）
* Dalvik主要是完成对象生命周期管理，堆栈管理，线程管理， 安全和异常管理，以及垃圾回收等等重要功能。
* 所有的Android应用程序的线程都对应着一个Linux进程，虚拟机因而可以有更多的依赖操作系统的线程调度和管理机制。
* Dalvik有一个特殊的虚拟机进程Zygote，他是虚拟机实例的孵化器。它在系统启动的时候就会产生，它会完成虚拟机的初始化，库的加载，预制类库和初始化的操作。每当启动一个APP时，就会将Zygote进程fork()出一个新的实例给APP使用。

#### Dalvik虚拟机架构

![image.png](http://upload-images.jianshu.io/upload_images/4143664-4ab8f9c30b40c529.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### Android应用的编译以及运行流程
![image.png](http://upload-images.jianshu.io/upload_images/4143664-446c233a3d9106b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一个Android应用程序源码，需要经过如下步骤，才可以运行在Dalvik虚拟机中：
* 首先将Java代码编译成标准的Java字节码(.class文件)。
* 使用dx工具将class文件转换为.dex文件
* 使用aapt(Android Asset Packaging Tool)工具将Dex文件，Resource文件，Manifest文件组合成一个二进制的程序包（APK）。
* Dalvik将APK中的.dex文件提取出来进行优化，形成.odex文件，将APK文件安装在设备上。
* 运行该APP时，Dalvik将.odex文件翻译成机器码执行。
* Dalvik虚拟机直接翻译.odex文件并运行，如果应用包不发生变化，则.odex文件不会重新生成(每次APP重启，Dalvik都会把.odex翻译一遍才执行)。

#### Dalvik进程管理

(1)Zygote是一个虚拟机进程，同时也是一个虚拟机实例的孵化器。每当系统要求执行一个Android应用程序时，Zygote会fork出一个子进程来执行该程序。

(2)在JVM中，当虚拟机启动一个Java应用后，程序逻辑对于操作系统来说，是一个**单进程** 状态。虽然我们可以使用多线程模型，但是对于操作系统来说，这个多线程并不可见，这些线程是JVM模拟出来的，操作系统可见的只是一个单线程的进程。
Dalvik虚拟机中，每一个线程是对应Linux的线程，每一个进程对应一个Linux进程，进程和线程对操作系统可见。

(3)Dalvik进程模型
Zygote在使用fork机制时有三种不同的方式，分别是：
* `fork()` :  Fork一个普通的进程，该进程属于Zygote进程。

* `forkAndSpecialize()` ：Fork一个特殊的进程，该进程不再试Zygote进程。

* `forkSystemServer()`  ：Fork一个系统服务进程。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-8d712bdaddeaadae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### Dalvik内存管理
Dalvik的内存管理需要依赖Linux的内存管理机制，垃圾回收是采用了标记-清除法，使用标志位来标识内存中的对象是否正在使用。Dalvik是采用 **分离式的标志位方案**：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-d2a38de06c379777.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### ART -- Android Runtime
从Android4.4开始，Google使用ART替代了Dalvik虚拟机，Dalvik执行的是.dex字节码，ART虚拟机执行的是机器码。

#### 两种编译模式

* 即时编译技术(JIT：Just In Time)：JVM使用即时编译技术，javac把程序源码编译成Java字节码，JVM通过逐条解释字节码将其翻译成对应的机器指令，逐条读入，逐条翻译执行，这样的话，速度必然比C/C++编译后的可执行二进制字节码程序慢。
* 预编译技术(AOT : Ahead of Time)：应用程序再第一次安装的时候，字节码就会预先编译成**机器码** ,使之成为真正的本地应用，这样的话，应用每次启动和执行速度都会加快。

在Dalvik中，依靠JIT去编译执行，然后在运行的时候，动态地将执行频率很高的字节码翻译成本地机器码，然后再执行，但是dex字节码翻译成本地机器码的过程是在程序运行的过程中，并且应用程序每次重启，都要做这个翻译工作，效率低下。

应用在安装时，ART虚拟机就把应用程序安装包中的.odex字节码翻译成本地机器码，之后再次打开应用时，执行的都是本地机器码，因此执行效率更快，启动更快。

#### 打包安装运行简化流程
![image.png](http://upload-images.jianshu.io/upload_images/4143664-00f4d7f0a267e8cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


ART的优点
* 系统性能显著提升。
* 应用启动更快，运行更快。
* 支持更低的硬件。
* 续航更快。

ART的缺点
* 更大的存储空间，可能增加10-20%。（空间换时间）
* 更长的应用安装时间。


[回到目录](#index)

<h1 id="Android开机启动流程">Android开机启动流程</h1>

Android 开机过程可分为两个阶段，第一个阶段是Linux的启动，第二个阶段才是Android的启动。
![image.png](http://upload-images.jianshu.io/upload_images/4143664-20d420addf064987.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 启动过程分析：
1. Boot Rom：当长按开机键时，引导芯片开始从固化在ROM的预设代码开始执行，然后加载引导程序到RAM。
2. BootLoader：又称引导程序，它是在操作系统运行的第一个程序，主要工作有检查RAM，初始化硬件参数等。
3. 初始化Kernel：在这个阶段中，初始化内核和各种设备驱动程序。
4. init进程：当初始化内核后，就会启动一个重要的进程，也就是**init进程** ，在Linux中所有的进程都是由init进程直接或间接fork出来的。init进程负责创建系统中最关键的几个子进程：SystemServer, Zygote和一些守护者进程(daemons)。
5. Zygote进程即是所有Android程序的父进程，在Zygote开启时，会调用如下`ZygoteInit.main()` 进行初始化：
```java
public static void main(String argv[]) {
    ....
    // 加载zygote的时候，会传入参数，startSystemServer变为true
     boolean startSystemServer = false;
     for (int i = 1; i < argv.length; i++) {
                if ("start-system-server".equals(argv[i])) {
                    startSystemServer = true;
                } else if (argv[i].startsWith(ABI_LIST_ARG)) {
                    abiList = argv[i].substring(ABI_LIST_ARG.length());
                } else if (argv[i].startsWith(SOCKET_NAME_ARG)) {
                    socketName = argv[i].substring(SOCKET_NAME_ARG.length());
                } else {
                    throw new RuntimeException("Unknown command line argument: " + argv[i]);
                }
            }
......
         // 启动的SystemServer进程
     if (startSystemServer) {
                startSystemServer(abiList, socketName);
         }
......
}

```

6. SystemServer 进程：在Zygote的`ZygoteInit.main()` 中，将标志位startSystemServer 设为true,即启动SystemServer 进程，即启动SystemServer进程，SystemServer进程中启动了一些系统里面重要的服务，如ActivityManagerService，WindowManager,DisplayManagerService,PackageManagerService等。当这些服务都启动完毕后，并注册到ServiceManager中。


7. Home Activity：即手机的"桌面"界面，当ActivityManagerService开启之后，会调用`finishBooting()`，同时发送广播`ACTION_BOOT_COMPLETED` 。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-fe9ee676469bc08c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


[回到目录](#index)


<h1 id="热修复">热修复</h1>
代码修复有两大主要的方案，一类是阿里系的底层替换方案，另一类是腾讯系的类加载方案。

### 底层替换方案
底层替换方案是在已经加载了的类中直接替换掉原有方法，是在原来类的基础上进行修改的，因而无法实现对与原有类进行方法和字段的删减，因为这样会破坏原有类的结构。（只能进行方法的替换）
底层替换的不稳定性：一旦补丁类中出现了方法的增加或减少，就会导致这个类以及整个Dex的方法数发生变化，方法数的变化伴随着方法索引的变化，这样在访问方法时就无法正确地索引到目标方法了。如果字段发生了增加或减少，则所有字段的索引都会发生变化，那么对于原先已经产生的这个类的实例，他们还是原来的结构，这样在访问新增字段的时候就会出现错误。

### 类加载方案
类加载方案的原理是在APP重启后让ClassLoader去加载新的类，因为在APP运行到一半时，所有需要变更的类已经被加载过了，如果不重启，则原来的类还是虚拟机中，此时只有重启，重启后在还没有走到业务逻辑之前抢先加载补丁中的新类，这样后续访问这个类时，就会解析为新类，从而达到热修复的目的。

* 底层替换方案：限制颇多，但时效性好，加载轻快，立即见效。
* 类加载方案：时效性差，需要重新冷启动才能见效，但修复范围广，限制少。

### Sophix热修复方案
Sophix热修复方案综合使用了底层替换方案（热部署）、类加载方案（冷启动）、资源热修复、so库热修复：

#### 1.热部署——底层替换
**虚拟机调用方法的原理** ：
以ART虚拟机为例，每一个Java方法在ART中都对应着一个`ArtMethod` ，ArtMethod 记录了这个Java方法的所有信息，包括所属类，访问权限，代码执行地址等。
Android6.0中，ART虚拟机中的ArtMethod是这样的：
```
class ArtMethod FINAL{
    protected :

    GcRoot<mirror::Class > declaring_class_;
    GcRoot<mirror::PointerArray> dex_cache_resolved_methods_;
    GcRoot<mirror::ObjectArray<mirror::Class> dex_cache_resolved_types_;
    uint32_t access_flags;
    uint32_t dex_method_index;
    uint32_t method_index;

    struct PACKED(4) ptrSizedFields{
        void* entry_point_from_interpreter_;
        void* entry_point_from_jni_;
        void* entry_point_from_quick_compiled_code_;
    }ptr_sized_fields_;
}

```

其中重要的是`entry_point_from_interpreter_` 和`entry_point_from_quick_compiled_code_`  ，它们都指向方法的执行入口，如果是以JIT(即时解释执行)模式的虚拟机，则`entry_point_from_interpreter_` 就是方法的执行地址。如果是以AOT模式的虚拟机，dex文件会被预先编译成机器码，则`entry_point_from_quick_compiled_code_` 就是方法的执行地址。

AndFix底层替换方案是按照开源Android代码中的ArtMethod进行逐个方法替换的，当手机厂商对开源Android中的ArtMethod进行了更改时，逐个方法替换就会造成索引偏移不正确，从而引发在机型适配上的热修复失败的问题。
Sophix采用了对ArtMethod整体替换。使用新的ArtMethod对旧的进行整体替换，从而可以无视底层ArtMethod的结构。


### 2.冷启动——类加载
Dalvik和ART尝试将一个dex文件解析加载到内存时，调用了`DexFile.loadDex()` 方法。
1. Dalvik尝试把dex文件加载到内存时，如果此时压缩包中有多个dex文件，则只会加载**classes.dex** ， 之外的其他dex被直接忽略掉。（Dalvik只能加载一个classes.dex，不支持多dex）
2. ART可以支持加载多个dex文件，当ART会首先加载classes.dex ，如果有其他的dex文件，则再依次加载后续的classes.dex 。所以ART下的冷启动修复方案是：把补丁包命名为classes.dex，将原apk中的dex依次命名为classes2.dex,classes3.dex等等，然后一起打包为一个压缩文件。在DexFile.loadDex得到DexFile对象，最后把该DexFile对象整个替换掉旧的dexElements数组。补丁类是存在于classes.dex中，当而旧的类(bug类)是存在于其他的classes.dex中，当先加载了classes.dex后，由于内存中已经有这个类了，后面便不再加载旧的bug类，从而达到修复的目的。
3. 无论是Dalvik还是ART，当虚拟机将dex文件加载到内存之前，如果dex不存在对应的odex，那么Dalvik会执行`dexopt` ，ART会执行`dexoat` ，最后得到的是一个优化后的dex——odex，实际上虚拟机执行的是odex。

Sophix的最终处理是：
* Dalvik下采用了自行研发的全量dex方案。
* ART下把补丁dex作为主dex文件(classes.dex)，进行先加载。

### 3.资源热修复
Instant Run 中的资源热修复：
* 首先构造一个新的AssetManager，并通过反射调用addAssetPath，把这个完整的新资源包加入到AssetManager中，这样就得到了一个含有所有资源的AssetManager。
* 通过反射得到Activity中的AssetManager的引用处，全部换成新的AssetManager。

APK中的资源实际上是存储在底层AssetManager的mResources成员中，mResources成员是一个ResTable类：
```
class ResTable{
    mutable Mutex     mLock;//互斥锁，用于多进程间访问互斥
    status_t                mError;
    resTable_config     mParams;
    Vector<Header*>     mHeaders;//表示所有resources.arsc原始数据，这就等同于所有通过addAssetPath加
                                                        //载进来的路径资源id信息
    Vector<PackageGroup*> mPackageGroups;//资源包的实体，包含所有加载进来的package id所对应的资源
    uint8_t                 mPackageMaps;//索引表
    uint9_t                 mNextPackageId;
};
```
一个Android进程只包含一个ResTable，ResTable的成员变量mPackageGroups就是所有解析过的资源包的集合，任何一个一个资源包都含有resources.arsc，它记录了所有资源id分配情况和资源中的所有字符串，这些信息是以二进制的方式存储的。底层AssetManager 做的就是解析这个文件，然后把相关信息存储到mPackageGroups里面。

一个resources.arsc里面包含若干个package，默认情况下，由打包工具aapt打出来的包只有一个package，这个package里包含了app中的所有资源信息。资源信息主要是指每个资源的名称和它对应的编号。编号是一个32位的int型，用16进制表示就是0xPPTTEEEE，PP为package id,TT为type id，EEEE为entry id。`type id` 是指资源的类型，如attr的type id为1，drawable的type id 为2,mipmap的type id 为3，layout的type id 为4等。entry id代表在一类资源中，资源文件的id索引。如layout类型，第一个layout的entry id 为0x0000，第二个layout为0x0001，依次类推（这个entry id是根据排布顺序决定的）。


由aapt工具打包后，APP中资源包的package id 为**0x7f**，系统的资源包的package id为**0x01** 。

**Sophix的资源修复原理** ：
构造了一个package id 为`0x66` 的资源包，这个包里只包含已改变的资源项，然后在原有AssetManager中`addAssetPath` 这个包。新增的资源包不与已加载的`0x7f` 冲突，因此直接加入到已有的AssetManager中就可以直接使用了。

### 4.so库修复
Java Api提供了两种方法加载一个so库：
* System.loadLibrary(String libName):传进去的参数是so库名称，默认是表示so库位于apk压缩文件的libs目录，最后复制到apk安装目录下。
* System.load(String pathName):传进去的参数是so库在磁盘中的完整路径，表示加载一个外部的so库文件。

上面两种方法实际上都调用了`nativeLoad()`这个native方法去加载so库。

JNI编程中，动态注册的native方法必须要实现`JNI_OnLoad` 方法，同时实现一个JNINativeMethod[]数组，静态注册的native方法必须使**Java_类完整路径_方法名的格式** 。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-2b770dd8fc382852.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


由于so库热部署实现起来约束多，于是Sophix团队放弃了So库的热部署。Sophix对于so库的修复是采取了冷启动修复的方式：

Sophix采用的反射注入方案：
在`System.loadLibrary("native-lib")` 函数中，最终这个so库最终传给native方法的执行参数是**so库在磁盘中的完整路径** 。so库的路径搜索是调用`DexPathList` 的`findLibrary()` 函数：
```java
//这是sdk<23的实现方式
private final File[] nativeLibraryDirectories;
public String findLibrary(String libraryName){
    String fileName = System.mapLibraryName(libraryName);
    for(File directory : nativeLibraryDirectories){
        String path = new File(directory,fileName).getPath();
        if(IoUtils.canOpenReadOnly(path)){
                //如果path文件存在并且可以打开，则返回该路径
                return path;
        }
    }
    return null;
}
```
实际上寻找so库是遍历`nativeLibraryDirectories` 数组，而只要将补丁so库路径插入到`nativeLibraryDirectories` 数组最前面，这样就能达到加载so库的时候是补丁so库而不是原来so库的目录，从而达到修复的目的。

```java
//这是sdk>=23的实现方式
private final Element[] nativeLibraryElements;
public String findLibrary(String libraryName){
    String fileName = System.mapLibraryName(libraryName);
    for(Element element : nativeLibraryElements){
        String path = new File(directory,fileName).getPath();
        if(IoUtils.canOpenReadOnly(path)){
                //如果path文件存在并且可以打开，则返回该路径
                if(path != null){
                    return path;
                }
        }
    }
    return null;
}
```
当sdk>=23时，只要把补丁so库的完整路径作为参数构造一个`Element` 对象，然后再插入到nativeLibraryElements数组最前面就可以达到修复的目的。

![image.png](http://upload-images.jianshu.io/upload_images/4143664-cf3dee9d2bd4bf52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


[回到目录](#index)

