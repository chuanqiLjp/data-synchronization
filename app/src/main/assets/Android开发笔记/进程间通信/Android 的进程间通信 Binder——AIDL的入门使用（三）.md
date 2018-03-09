---
title: Android 的进程间通信 Binder——AIDL的入门使用（三）
layout: post
date: 2017-12-02 21:25:58
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
1、AIDL的大致使用流程：首先是创建一个AIDL接口文件声明需要在客户端调用的接口，再创建一个Service，接着创建一个类继承自AIDL接口中的Stub类并实现Stub中的抽象方法，在Service 的onBind方法中返回这个类的对象，然后在客户端就可以绑定服务端的Service，建立连接后就可以访问远程服务端的方法了；
2、思考：当有多个AIDL接口文件时怎么办呢？按照以前的思路，只有创建多个Service了，或者将多个AIDL接口的方法放在一个AIDL中了，这回造成代码耦合程度高，这篇我们主要就是解决这个问题的。解决思路：在服务端统一返回一个AIDL文件的binder，这个binder对象只有一个query方法，可以根据传入的值去创建其他对应的AIDL接口的Binder对象在进行返回，这样客户端就可以单独调用服务端的AIDL接口中的方法了，并且只需要一个Service。

# 多个AIDL接口通信服务端：
#### 1、新建三个AIDL接口文件，分别为：IStringComputface、IIntComputInterface 和IBinderInterface ，前两个提供给客户端具体的调用方法，IBinderInterface 提供给客户端查询调用的Binder对象。然后Make Project。
```
// IStringComputface.aidl
package com.ljp.moreaidl_server;
//计算字符串的AIDL接口
interface IStringComputInterface {
    String comput(String str);
}
```

```
// IBinderInterface.aidl
package com.ljp.moreaidl_server;
interface IBinderInterface {
    IBinder quary(int binderCode);//根据binder码查询Binder对象
}
```

```
// IBinderInterface.aidl
package com.ljp.moreaidl_server;
interface IBinderInterface {
    IBinder quary(int binderCode);//根据binder码查询Binder对象
}
```
#### 2、创建3个类分别继承对应AIDL接口的Stub类并实现未实现的方法。
```
package com.ljp.moreaidl_server;
import android.os.RemoteException;
import android.text.TextUtils;
public class StringComputImp extends IStringComputInterface.Stub {
    @Override
    public String comput(String str) throws RemoteException {
        if (TextUtils.isEmpty(str)) {
            return null;
        }
        char[] charArray = str.toCharArray();
        for (int i = 0; i < charArray.length; i++) {
            charArray[i] += 1;
        }
        return new String(charArray);
    }
}
```

```
package com.ljp.moreaidl_server;
import android.os.RemoteException;
public class IntComputImp extends IIntComputInterface.Stub {
    @Override
    public int addComput(int x, int y) throws RemoteException {
        return x + y;
    }
}
```

```
package com.ljp.moreaidl_server;
import android.os.IBinder;
import android.os.RemoteException;
public class BinderImp extends IBinderInterface.Stub {
    public static final int ComputString = 0;
    public static final int ComputInt = 1;
    @Override
    public IBinder quary(int binderCode) throws RemoteException {
        IBinder binder = null;
        switch (binderCode) {
            case ComputString:
                binder = new StringComputImp();
                break;
            case ComputInt:
                binder = new IntComputImp();
                break;
        }
        return binder;
    }
}
```
#### 3、创建一个Service，并在onBinder方法中返回IBinderInterface的实现类BinderImp的对象，并在注册Service。服务端的代码就完成了。
```
package com.ljp.moreaidl_server;
import android.app.Service;
import android.content.Intent;
import android.os.IBinder;
public class MyServerService extends Service {
    BinderImp mBinderImp = new BinderImp();
    @Override
    public IBinder onBind(Intent intent) {
        return mBinderImp;
    }
}
```
在AndroidMainfest文件中注册Service。
```
        <service
            android:name=".MyServerService"
            android:enabled="true"
            android:exported="true">
            <!-- 程序的包名：com.ljp.moreaidl_server 绑定服务端时使用服务端程序的包名。-->
            <intent-filter>
                <action android:name="com.ljp.moreAidl.server.action"/>
            </intent-filter>
        </service>
```
# 多个AIDL接口通信客户端：
#### 1、这里写一个Binder连接池类，采用单例模式，在创建对象时就去绑定服务端，绑定成功后会回调ServiceConnection对象的onServiceConnected方法，在onServiceConnected方法中给binder设置死亡代理，当服务端的binder对象死亡时系统回调mDeathRecipient.binderDied（）方法从而进行重连。并暴露一个quaryBinder的方法，
```
package com.ljp.moreaidl_client;
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.IBinder;
import android.os.RemoteException;
import android.util.Log;
import com.ljp.moreaidl_server.IBinderInterface;
public class BinderPool {
    private static final String TAG = "moreAidl";
    public static final int ComputString = 0;
    public static final int ComputInt = 1;
    private static volatile BinderPool instance;
    private Context mContext;
    private BinderPool(Context context) {
        mContext = context.getApplicationContext();
        connectService();
    }
    public static BinderPool getInstance(Context context) {
        if (instance == null) {
            synchronized (BinderPool.class) {
                if (instance == null) {
                    instance = new BinderPool(context);
                }
            }
        }
        return instance;
    }
    IBinder.DeathRecipient mDeathRecipient = new IBinder.DeathRecipient() {
        @Override
        public void binderDied() {
            //当Binder死亡时，系统会回调该方法，在此移除之前绑定的Binder代理并重新绑定远程服务
            Log.e(TAG, "binderDied: 与服务端连接的Binder对象死亡。请求重连！");
            mIBinderInterface.asBinder().unlinkToDeath(mDeathRecipient, 0);
            mIBinderInterface = null;
            connectService();
        }
    };
    IBinderInterface mIBinderInterface = null;
    ServiceConnection mConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            mIBinderInterface = IBinderInterface.Stub.asInterface(service);//返回一个可以查询其他AIDL接口的Binder。
            try {
                //给binder设置死亡代理，当服务端的binder对象死亡时系统回调mDeathRecipient.binderDied（）方法
                mIBinderInterface.asBinder().linkToDeath(mDeathRecipient, 0);
            } catch (RemoteException e) {
                e.printStackTrace();
            }
            Log.e(TAG, "onServiceConnected: 与服务端连接成功");
        }
        @Override
        public void onServiceDisconnected(ComponentName name) {
            Log.e(TAG, "onServiceDisconnected: 与服务端断开连接");
            mDeathRecipient.binderDied();
        }
    };
    private synchronized void connectService() {
        Intent intent = new Intent();
        intent.setPackage("com.ljp.moreaidl_server");
        intent.setAction("com.ljp.moreAidl.server.action");
        boolean success = mContext.bindService(intent, mConnection, Context.BIND_AUTO_CREATE);
        Log.e(TAG, "connectService: 绑定服务端＝" + success);
    }
    public synchronized void unConnect() { //断开与服务端的连接
        if (mIBinderInterface != null) {
            mContext.unbindService(mConnection);//不会回调onServiceDisconnected()
            mIBinderInterface.asBinder().unlinkToDeath(mDeathRecipient, 0);
        }
        mIBinderInterface = null;
        instance = null;
    }
    public IBinder quaryBinder( int binderCode) { //AIDL的调用都是耗时操作，建议放在子线程中
        IBinder binder = null;
        if (mIBinderInterface != null) {
            try {
                binder = mIBinderInterface.quary(binderCode); //调用服务端的查询
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }
        return binder;
    }
}
```
#### 2、在客户端中调用服务端的方法时，先调用BinderPool对象的quaryBinder( int )，然后在使用对应的AIDL接口.Stub.asInterface(binder)进行转换为相应的AIDL接口对象，这时就可以调用该AIDL接口服务端的方法了，由于AIDL的调用都是耗时操作，建议都放在子线程中处理。这里第一次绑定时并不会立即连接成功，需要等待一小段时间，因此建议在Application中获取一次BinderPool的对象进行绑定连接，后面就可以直接使用了。
```
    public void StringComput(View view) {
        IBinder binder = BinderPool.getInstance(this).quaryBinder(BinderPool.ComputString);
        if (binder == null) {
            return;
        }
        IStringComputInterface anInterface = IStringComputInterface.Stub.asInterface(binder);
        try {
            String comput = anInterface.comput("abcdefg"); //AIDL的调用都是耗时操作，建议放在子线程中
            Log.e(TAG, "StringComput: comput=" + comput);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
    public void IntComput(View view) {
        IBinder binder = BinderPool.getInstance(this).quaryBinder(BinderPool.ComputInt);
        if (binder == null) {
            return;
        }
        IIntComputInterface anInterface = IIntComputInterface.Stub.asInterface(binder);
        try {
            int addComput = anInterface.addComput(10, 20);//AIDL的调用都是耗时操作，建议放在子线程中
            Log.e(TAG, "StringComput: addComput=" + addComput);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
    @Override
    protected void onDestroy() {
        BinderPool.getInstance(this).unConnect();
        super.onDestroy();
    }
```
测试结果：
```
12-02 21:16:04.176 27386-27386/com.ljp.moreaidl_client E/moreAidl: connectService: 绑定服务端＝true
12-02 21:16:04.186 27386-27386/com.ljp.moreaidl_client E/moreAidl: onServiceConnected: 与服务端连接成功
12-02 21:16:05.976 27386-27386/com.ljp.moreaidl_client E/moreAidl: StringComput: comput=bcdefgh
12-02 21:16:07.536 27386-27386/com.ljp.moreaidl_client E/moreAidl: StringComput: addComput=30
```
 我的CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/78698458
