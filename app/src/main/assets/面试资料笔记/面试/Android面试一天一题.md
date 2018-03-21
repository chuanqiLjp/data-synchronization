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


<h2 id="修改SharedPreferences后两种提交方式有什么区别？">
修改SharedPreferences后两种提交方式有什么区别？
</h2>

[返回目录](#目录)

1. commit这种方式很常用，在比较早的SDK版本中就有了，这种提交修改的方式是同步的，会阻塞调用它的线程，并且这个方法会返回boolean值告知保存是否成功（如果不成功，可以做一些补救措施）。
2. 而apply是异步的提交方式，目前Android Studio也会提示大家使用这种方式。
3. SharedPreferences还提供一个监听接口可以监听SharedPreferences的键值变化，需要监控键值变化的可以用registerOnSharedPreferenceChangeListener添加监听器。
4. Sharedpreferences：系统对它的读写有一定的缓存策略，即在内存中会有一份Sharedpreferences文件的缓存，因此多进程模式下系统对它的读写就不可靠；不建议在多进程中使用（可考虑ContentProvider），开源的替代方案[tray](https://github.com/grandcentrix/tray)


链接：https://www.jianshu.com/p/4dd53e1be5ba



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

<h2 id="有没有遇到OOM的问题？如何优化图片占用的内存空间？">
有没有遇到OOM的问题？如何优化图片占用的内存空间？
</h2>

[返回目录](#目录)

1. 3.0版本的还增加了一个inBitmap属性（BitmapFactory.Options.inBitmap）。如果设置了这个属性则会重用这个Bitmap的内存从而提升性能。但是这个重用是有条件的，在Android4.4之前只能重用相同大小的Bitmap，Android4.4+则只要比重用Bitmap小即可
2. 使用采样率（inSampleSize），如果最终要压缩图片，如显示缩列图，我们并不需要加载完整的图片数据，只需要按一定的比例加载即可；使用Matrix变形等，比如使用Matrix进行放大，虽然图像大了，但并没有占用更多的内存。
3. 如果图片很大，比如他们的占用内存算下来就直接OOM了，那么我们肯定不能直接加载它。解决主法还是有很多的，系统也给我们提供了一个类BitmapRegionDecoder，可以用来分块加载图片

链接：https://www.jianshu.com/p/3c597baa39e5


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



<h2 id="两个Fragment之间如何进行通信？">
两个Fragment之间如何进行通信？
</h2>

[返回目录](#目录)

> 尽管Fragment是作为独立于Activity的对象实现，并且可在多个Activity内使用，但片段的给定实例会直接绑定到包含它的 Activity。具体地说，片段可以通过getActivity()访问Activity实例。同样地，Activity也可以使用findFragmentById()或findFragmentByTag()，通过从FragmentManager获取对Fragment的引用来调用片段中的方法。知道这些基础，你应该很清楚怎么进行通信了。不过我们建议的方式是面向接口编程，在Fragment中getActivity获取到的Activity实例应该是实现了我们某个接口的实例，即在Fragment的代码中不应该出现某个具体的Activity类

如何管理Fragment回退栈?

FragmentTransaction中remove和detach的区别 ?


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
**分发事件：dispatchTouchEvent**
在这里进行事件的分发，onInterceptTouchEvent和onTouchEvent都是由dispatchTouchEvent负责调度的。

**拦截事件：onInterceptTouchEvent**
只有ViewGroup才有这个方法。拦截了的话，ViewGroup就不会把事件继续分发给子View了，即子View的dispatchTouchEvent和onTouchEvent这两个方法都不会被调用。返回true时，表示ViewGroup会拦截事件。

**消费事件：onTouchEvent**
onTouchEvent 返回true时，表示事件被消费掉了。一旦事件被消费掉了，其他父元素的onTouchEvent方法都不会被调用。

![image.png](https://upload-images.jianshu.io/upload_images/4143664-501daf8083e483a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

链接：https://www.jianshu.com/p/9abf6a874feb


<h2 id="知道什么是ART吗？它和Dalvik有什么区别？">
知道什么是ART吗？它和Dalvik有什么区别？
</h2>

[返回目录](#目录)







链接：



你在工作中有遇到什么问题？
> 在工作中开发时遇到一个需求：在主Activity M 中启动另一个Activity B，但是要求必须要上次的A Activity执行完onDestory()方法，怎么办？
