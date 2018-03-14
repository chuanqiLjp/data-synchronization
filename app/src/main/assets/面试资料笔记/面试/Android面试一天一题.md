<h2 id="目录">
目录
</h2>

[返回目录](#目录)



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


<h2 id="通过new创建的View实例它的onSaveStateInstance会被调用吗？">
通过new创建的View实例它的onSaveStateInstance会被调用吗？
</h2>

[返回目录](#目录)
自定义View控件的状态被保存需要满足两个条件：
1. View有唯一的ID；
2. View的初始化时要调用 setSaveEnabled(true) ；

链接：https://www.jianshu.com/p/4f482548de59


<h2 id="Java的值传递和引用传递问题">
Java的值传递和引用传递问题
</h2>

[返回目录](#目录)

> “在Java里面参数传递都是按值传递”这句话的意思是：按值传递是传递的值的拷贝，按引用传递其实传递的是引用的地址值，所以统称按值传递。简单的说，基本类型是按值传递的，方法的实参是一个原值的复本。类对象是按对象的引用地址（内存地址）传递地址的值，那么在方法内对这个对象进行修改是会直接反应在原对象上的（或者说这两个引用指向同一内存地址）。不过要注意String这个类型

有很多人并不重视基础的问题，总认为不知道也无防，用的时候有问题自然会报出来，到时候再解决就好了，你知道的也没比我多能耐。我只能说，知道的话确实不比别人多能耐，只是多了一份从容。

链接： https://www.jianshu.com/p/c0c5e0540928



<h2 id="能讲讲Android的Handler机制吗？">
能讲讲Android的Handler机制吗？
</h2>

[返回目录](#目录)

> Message：消息分为硬件产生的消息(如按钮、触摸)和软件生成的消息；

>  MessageQueue：消息队列的主要功能向消息池投递消息(MessageQueue.enqueueMessage)和取走消息池的消息(MessageQueue.next)；

>  Handler：消息辅助类，主要功能向消息池发送各种消息事件(Handler.sendMessage)和处理相应消息事件(Handler.handleMessage)；

>  Looper：不断循环执行(Looper.loop)，按分发机制将消息分发给目标处理者。

链接：https://www.jianshu.com/p/108db0240a34


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




链接：



你在工作中有遇到什么问题？
> 在工作中开发时遇到一个需求：在主Activity M 中启动另一个Activity B，但是要求必须要上次的A Activity执行完onDestory()方法，怎么办？
