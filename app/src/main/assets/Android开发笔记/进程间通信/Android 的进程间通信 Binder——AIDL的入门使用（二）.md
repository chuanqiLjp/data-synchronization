---
title: Android 的进程间通信 Binder——AIDL的入门使用（二）
layout: post
date: 2017-12-01 11:07:58
comments: true
categories:
  - Android
  - 进程间通信
tags: [进程间通信,Binder,AIDL]
keywords: 进程间通信,Binder,AIDL
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：商业转载请联系我获得授权，非商业转载请在醒目位置注明出处。



###进程间通信系列
##### [AIDL的入门使用(一)](http://www.jianshu.com/p/00cd607b5292)
##### [AIDL的入门使用(二)](http://www.jianshu.com/p/5f394bf22849)
##### [AIDL的入门使用(三)](http://www.jianshu.com/p/0ea8e4d42ee5)
##### [Messenger的入门使用](http://www.jianshu.com/p/f1dc06457047)
### 序言：
在[Android 的进程间通信 Binder——AIDL的入门使用（一）](http://www.jianshu.com/p/00cd607b5292)中我们可以通过AIDL调用服务端的方法进行操作，那可不可以反过来呢，服务端调用客户端的方法，场景：图书馆有新书时自动通知所有订阅的读者；这里就可以使用观察者模式，客户端在服务端注册一个接口，当服务端有新书，自动调用客户端注册的接口。这种方式也可以用于消息推送的通知其他进程（猜测）。
# 对服务端进行的改造：（过程参考《Android开发艺术探索》）
### 1、定义一个当有新书到来时的通知接口，由于需在客户端回调使用到了跨进程，所以需要定义在AIDL文件中。
```
// IOnNewBookArrivedListener.aidl
package com.ljp.aidl_server.aidl;
// Declare any non-default types here with import statements
import com.ljp.aidl_server.aidl.Book;//虽然在同一个包中，但还是要进行导包操作，否则会报错。
interface IOnNewBookArrivedListener {//当有新书的时候的通知接口。
    void onNewBookArrived(in Book newBook);
}
```
### 2、在IMyAidlInterface.aidl文件中增加注册和解除注册的方法。
```
// IMyAidlInterface.aidl
package com.ljp.aidl_server.aidl;
// Declare any non-default types here with import statements
import com.ljp.aidl_server.aidl.Book;//虽然在同一个包中，但还是要进行导包操作，否则会报错。
import com.ljp.aidl_server.aidl.IOnNewBookArrivedListener;
interface IMyAidlInterface {
    ........
    void registerListener(in IOnNewBookArrivedListener listener);// 注册监听
    void unregisterListener(in IOnNewBookArrivedListener listener);//解除注册
}
```
### 3、在AidlSerVerService中的IMyAidlInterface.Stub实现registerListener和unregisterListener方法，在onCreat方法中开启一个线程，每隔5秒就加入一本新书并通知已注册的客户端。
```
package com.ljp.aidl_server.aidl;
import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
import android.os.RemoteCallbackList;
import android.os.RemoteException;
import android.text.TextUtils;
import android.util.Log;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;
public class AidlSerVerService extends Service {
    private static final String TAG = "AidlSerVerService";
    private volatile boolean mIsServiceDestoryed = false;//Service是否销毁
    // 用于保存跨进程接口回调的对象，已自动实现了线程同步
    private RemoteCallbackList<IOnNewBookArrivedListener> mListenerList = new RemoteCallbackList<>();
    //CopyOnWriteArrayList支持并发读写并自动进行线程同步 ，
    private CopyOnWriteArrayList<Book> mBookList = new CopyOnWriteArrayList<>();
    private String tag = "empty";
    private int num = -1;
    private IMyAidlInterface.Stub stub_binder = new IMyAidlInterface.Stub() {//IMyAidlInterface.Stub为Binder的子类
        @Override
        public void basicTypes(int anInt, long aLong, boolean aBoolean, float aFloat, double aDouble, String aString) throws RemoteException {
        }
        @Override
        public void addBook(Book book) throws RemoteException {
            addBookNotify(book);
        }
        @Override
        public List<Book> getBooks() throws RemoteException {
            return mBookList;//这里虽然返回的是CopyOnWriteArrayList，但底层会按照ArrayList进行读取然后返回给客户端
        }
        @Override
        public void registerListener(com.ljp.aidl_server.aidl.IOnNewBookArrivedListener listener) throws RemoteException {
            mListenerList.register(listener);
            Log.e(TAG, "registerListener: 已经注册Size=" + mListenerList.getRegisteredCallbackCount());
        }
        @Override
        public void unregisterListener(com.ljp.aidl_server.aidl.IOnNewBookArrivedListener listener) throws RemoteException {
            mListenerList.unregister(listener);
            Log.e(TAG, "unregisterListener: 现注册的Size=" + mListenerList.getRegisteredCallbackCount());
        }
        @Override
        public void setTag(String tag) throws RemoteException {
            synchronized (this) {//进行同步操作，有可能并发
                if (!TextUtils.isEmpty(tag)) {
                    AidlSerVerService.this.tag = tag;
                }
            }
        }
        @Override
        public String getTag() throws RemoteException {
            return tag;
        }
        @Override
        public void setNum(int num) throws RemoteException {
            synchronized (this) {
                AidlSerVerService.this.num = num;
            }
        }
        @Override
        public int getNum() throws RemoteException {
            return num;
        }
    };
    @Override
    public IBinder onBind(Intent intent) {
        // TODO: Return the communication channel to the service.
        return stub_binder;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        mBookList.add(new Book(100, "Android开发", 20));
        mBookList.add(new Book(101, "Java开发", 22));
        new Thread(new Worker()).start(); //开启一个线程，每5秒添加一本新书并通知到已注册的全部客户端
        Log.e("aidl_server_Service", "onCreate: ");
    }
    @Override
    public void onDestroy() {
        mIsServiceDestoryed=true;
        super.onDestroy();
    }
    /**
     * 添加书籍并通知客户端
     * @param newBook   添加的新书
     * @throws RemoteException
     */
    private void addBookNotify(Book newBook) throws RemoteException {
        mBookList.add(newBook);
        int count = mListenerList.beginBroadcast();//必须与 finishBroadcast()方法配对使用
        Log.e(TAG, "addBookNotify: 通知所有的监听器，size= " + count);
        for (int i = 0; i < count; i++) {
            IOnNewBookArrivedListener item = mListenerList.getBroadcastItem(i);
            if (item != null) {
                item.onNewBookArrived(newBook);//调用客户端的方法，通知客户端，由于调用了客户端的方法为耗时操作建议放在子线程中
            }
        }
        mListenerList.finishBroadcast();//必须与 beginBroadcast()方法配对使用
    }
    private class Worker implements Runnable {
        @Override
        public void run() {
            while (!mIsServiceDestoryed) {
                try {
                    Thread.sleep(5 * 1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                int bookId = mBookList.size() + 1;
                Book newBook = new Book(bookId, "new book #" + bookId, bookId * 1.5);
                try {
                    addBookNotify(newBook);
                } catch (RemoteException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```
###### 上面使用到了CopyOnWriteArrayList和RemoteCallbackList，这里的监听器管理不能使用List，否则在最后解除注册不会成功（因为服务端收到的listener每次都是新创建的对象），因此采用RemoteCallbackList管理。
```
CopyOnWriteArrayList：支持并发读写，自动进行线程同步，使用和ArrayList相同，实现了List接口，但和ArrayList没有任何关系。类似的还有ConcurrentHashMap
RemoteCallbackList：系统专门提供用于删除跨进程listener的接口，使用了泛型支持管理任意的AIDL接口，其内部采用ArrayMap<IBinder, Callback>键值对的方式存储，当客户端进程终止后它能自动移除客户端已注册的listener，，内部已自动实现了线程同步，beginBroadcast()必须与 finishBroadcast()方法配对使用
```
# 对客户端的改造：
### 将服务端的AIDL包复制到客户端的对应位置下，创建一个监听器对象在绑定服务端成功后注册，在onDestory方法中解除注册。
```
    @Override
    protected void onDestroy() {
        UnbindAidlService(null);
        super.onDestroy();
    }
    private IMyAidlInterface mService_face;
    private static final String TAG = "Main_Client";
    ServiceConnection mConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            Log.e(TAG, "onServiceConnected: ");
            mService_face = IMyAidlInterface.Stub.asInterface(service);
            try {
                service.linkToDeath(mDeathRecipient, 0);//客户端远程绑定服务成功后，给binder设置死亡代理，当服务端的binder对象死亡时系统回调mDeathRecipient.binderDied（）方法，service.isBinderAlive();//可以判断服务端的Binder是否死亡
                mService_face.registerListener(mIOnNewBookArrivedListener);//2、绑定成功后注册通知的监听器。
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }
        @Override
        public void onServiceDisconnected(ComponentName name) {
            Log.e(TAG, "onServiceDisconnected: ");
        }
    };
    private IBinder.DeathRecipient mDeathRecipient = new IBinder.DeathRecipient() {
        @Override
        public void binderDied() {
            //当Binder死亡时，系统会回调该方法，在此移除之前绑定的Binder代理并重新绑定远程服务
            if (mService_face == null) return;
            mService_face.asBinder().unlinkToDeath(mDeathRecipient, 0);
            mService_face = null;
            Log.e(TAG, "binderDied: Binder死亡时,移除之前绑定的Binder代理并重新绑定远程服务");
            bindAidlService(null);
        }
    };
    //1、创建一个监听器用于服务端添加新书时的通知。
    private IOnNewBookArrivedListener mIOnNewBookArrivedListener=new IOnNewBookArrivedListener.Stub(){
        @Override
        public void onNewBookArrived(Book newBook) throws RemoteException {
            Log.e(TAG, "onNewBookArrived: 收到了服务端更新的通知，newbook="+newBook );
        }
    };
    public void bindAidlService(View view) {
        Intent intent_service = new Intent();
        intent_service.setPackage("com.ljp.aidl_server"); //设置需要绑定的服务端的包名，不是服务端Service的包名
        intent_service.setAction("server.aidl.service.action");//设置你所需调用服务的意图
        boolean successful = bindService(intent_service, mConnection, BIND_AUTO_CREATE);
        Log.e(TAG, "bindAidlService: successful=" + successful);
    }
    public void UnbindAidlService(View view) {
        //3、客户端销毁时解除注册在服务端的监听器
        if (mService_face!=null&&mService_face.asBinder().isBinderAlive()){
            try {
                mService_face.unregisterListener(mIOnNewBookArrivedListener);//解除在服务端的注册
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }
        if (mConnection != null) {
            unbindService(mConnection);
            Log.e(TAG, "UnbindAidlService: ");
        }
    }
```
测试结果：
```
12-01 10:59:12.689 4143-4143/com.ljp.aidl_client E/aidlLog: bindAidlService: successful=true
12-01 10:59:12.719 4143-4143/com.ljp.aidl_client E/aidlLog: onServiceConnected:
12-01 10:59:12.719 4352-4364/com.ljp.aidl_server:remote E/aidlLog: registerListener: 已经注册Size=1
12-01 10:59:16.249 4352-4363/com.ljp.aidl_server:remote E/aidlLog: addBookNotify: 通知所有的监听器，size= 1
12-01 10:59:16.249 4143-4143/com.ljp.aidl_client E/aidlLog: onNewBookArrived: 收到了服务端更新的通知，newbook=Book{id=0, name='book0', price=30.5}
12-01 10:59:16.249 4143-4143/com.ljp.aidl_client E/aidlLog: addBook_AidlService:
12-01 10:59:17.709 4352-5093/com.ljp.aidl_server:remote E/aidlLog: addBookNotify: 通知所有的监听器，size= 1
12-01 10:59:17.709 4143-4158/com.ljp.aidl_client E/aidlLog: onNewBookArrived: 收到了服务端更新的通知，newbook=Book{id=4, name='new book #4', price=6.0}
12-01 10:59:22.699 4352-5093/com.ljp.aidl_server:remote E/aidlLog: addBookNotify: 通知所有的监听器，size= 1
12-01 10:59:22.709 4143-4157/com.ljp.aidl_client E/aidlLog: onNewBookArrived: 收到了服务端更新的通知，newbook=Book{id=5, name='new book #5', price=7.5}
12-01 10:59:24.879 4352-4364/com.ljp.aidl_server:remote E/aidlLog: unregisterListener: 现注册的Size=0
12-01 10:59:24.879 4143-4143/com.ljp.aidl_client E/aidlLog: UnbindAidlService:
12-01 10:59:27.709 4352-5093/com.ljp.aidl_server:remote E/aidlLog: addBookNotify: 通知所有的监听器，size= 0
```

 我的CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/78684695