---
title: Android 的进程间通信 Binder——Messenger的入门使用
layout: post
date: 2017-11-30 11:32:58
comments: true
categories:
  - Android
  - 进程间通信
tags: [进程间通信,Binder,AIDL,Messenger]
keywords: 进程间通信,Binder,AIDL,Messenger
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

##### 序言：Messenger是Google为我们封装好的简洁版的AIDL，当面对少量的并发进程间通信更适用。而且不用考虑线程同步问题。
# Messenger进程间通信服务端
### 1、创建服务端Module “messenger_server”，并创建Service文件：右键单击包名——>New——>Service——>Service——>命名为MyServerService——>Finish；
![image.png](http://upload-images.jianshu.io/upload_images/4143664-f839a320595c757f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/4143664-d7933a0a80dc9793.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###  2、在服务端的Service中创建一个Handler对象用于接收客户端发送过来的消息，使用handler对象创建一个服务端的Messenger，并在onBind方法中返回messenger.getBinder()
```
package com.ljp.messenger_server;
import android.app.Service;
import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.os.IBinder;
import android.os.Message;
import android.os.Messenger;
import android.os.RemoteException;
import android.util.Log;
public class MyServerService extends Service {
    private static final String TAG = "messenger";
    //1、用于接收客户端发送过来的消息
    Handler mServerHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case 1:
                    //获取来自客户端的消息
                    Bundle data = msg.getData();
                    String clientMsg = data.getString("clientMsg");
                    Log.e(TAG, "handleMessage: 收到来自客户端的消息：clientMsg=" + clientMsg);
                    // 向客户端回复消息
                    Messenger clientMessenger = msg.replyTo; //获取到客户端的 Messenger，这里可以保存为全局变量，以后就不用每次都去获取了
                    Message toClientMsg = Message.obtain();
                    toClientMsg.what=1;
                    Bundle bundle = new Bundle();
                    bundle.putString("serverMsg", "我是服务端，已经收到你的消息了");
                    toClientMsg.setData(bundle);
                    try {
                        clientMessenger.send(toClientMsg);//回复客户端
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                    break;
                default:
                    super.handleMessage(msg);
                    break;
            }
        }
    };
    //2、使用Handler创建Messenger。
    Messenger mServerMessenger = new Messenger(mServerHandler);
    @Override
    public IBinder onBind(Intent intent) {
        return mServerMessenger.getBinder();//3、向客户端返回服务端的Binder
    }
    @Override
    public void onDestroy() {
        super.onDestroy();
        Log.e(TAG, "onDestroy: MyServerService销毁" );
    }
}
```
### 3、在AndroidMainfest文件中注册Service并添加对应的Action并对外暴露使用。
```
        <!--注：服务端的包名为：com.ljp.messenger_server，客户端绑定Service的时候是使用程序的包名，不是使用MyServerService类的包名-->
        <service
            android:name=".MyServerService"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="com.ljp.messenger_server.server.action"/>
            </intent-filter>
        </service>
```
服务端的创建完成了,再来看看客户端。
# Messenger进程间通信客户端
### 1、创建服务端Module “messenger_client”，创建一个客户端Handler对象用于接收服务端发送过来的消息，使用handler对象创建一个客户端的Messenger，在bindService成功后保存服务端的Messenger并用此向服务端发送消息，代码中有详细的步骤。
```
package com.ljp.messenger_client;
import android.content.ComponentName;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.Handler;
import android.os.IBinder;
import android.os.Message;
import android.os.Messenger;
import android.os.RemoteException;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    Messenger mServerMessenger = null;
    ServiceConnection mConnection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            mServerMessenger = new Messenger(service);
            Log.e(TAG, "onServiceConnected: 绑定服务端成功");
        }
        @Override
        public void onServiceDisconnected(ComponentName name) {//注：unbindService不会调用此方法
            mServerMessenger = null;
            Log.e(TAG, "onServiceDisconnected: 与服务端断开联系");
        }
    };
    //3、绑定服务端并在回调方法onServiceConnected中保存下服务端的Messenger
    public void bind(View view) {
        Intent intent = new Intent();
        intent.setPackage("com.ljp.messenger_server");
        intent.setAction("com.ljp.messenger_server.server.action");
        boolean bindService = bindService(intent, mConnection, BIND_AUTO_CREATE);
        Log.e(TAG, "bind: bindService=" + bindService);
    }
    private static final String TAG = "messenger";
    //1、创建一个Handler用于接收服务端发送过来的消息
    Handler mClientHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case 1:
                    //获取来自服务端的消息
                    Bundle data = msg.getData();
                    String serverMsg = data.getString("serverMsg");
                    Log.e(TAG, "handleMessage: 收到来自服务端的消息：serverMsg=" + serverMsg);
                    break;
                default:
                    super.handleMessage(msg);
                    break;
            }
        }
    };
    //2、构建一个客户端的Messenger，方便服务端使用
    Messenger mClientMessenger = new Messenger(mClientHandler);
    //4、使用服务端的Messenger发送消息给服务端，并携带上客户端的Messenger。
    public void sendMessageToServer(View view) {
        if (mServerMessenger == null) return;
        // 向服务端发送消息
        Message toServerMsg = Message.obtain();
        toServerMsg.what = 1;
        toServerMsg.replyTo = mClientMessenger;//将客户端的Messenger携带上，服务端可以用来回复客户端的消息
        Bundle bundle = new Bundle();
        bundle.putString("clientMsg", "你好，服务端*~*");
        toServerMsg.setData(bundle);
        try {
            mServerMessenger.send(toServerMsg);//使用服务端的Messenger发送消息给服务端
            Log.e(TAG, "sendMessageToServer: ");
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
    public void unBind(View view) {
        if (mServerMessenger != null) {
            unbindService(mConnection);
            mServerMessenger = null;
        }
        Log.e(TAG, "unBind: ");
    }
    @Override
    protected void onDestroy() {
        super.onDestroy();
        unBind(null);
    }
}
```
测试结果如下：
```
11-30 10:38:33.543 27094-27094/com.ljp.messenger_client E/messenger: bind: bindService=true
11-30 10:38:33.553 27094-27094/com.ljp.messenger_client E/messenger: onServiceConnected: 绑定服务端成功
11-30 10:38:35.313 26811-26811/com.ljp.messenger_server E/messenger: handleMessage: 收到来自客户端的消息：clientMsg=你好，服务端*~*
11-30 10:38:35.323 27094-27094/com.ljp.messenger_client E/messenger: sendMessageToServer:
11-30 10:38:35.323 27094-27094/com.ljp.messenger_client E/messenger: handleMessage: 收到来自服务端的消息：serverMsg=我是服务端，已经收到你的消息了
11-30 10:38:37.113 27094-27094/com.ljp.messenger_client E/messenger: unBind:
11-30 10:38:37.113 26811-26811/com.ljp.messenger_server E/messenger: onDestroy: MyServerService销毁
```
# Messenger的总结
##### 1、客户端报错：java.lang.RuntimeException: Can't marshal non-Parcelable objects across processes.；是因为发送消息时使用了msg.obj字段赋值了一个字符串，可以采用不实用msg.obj，使用Bundle传递数据，msg.setData(bundle),bundle=msg.getData();
##### 2、Message中能使用的载体只有what、arg1、arg2、replyTo、Bundle；不要使用 obj 字段；
##### 3、Android 5.0以后不支持隐式intent启动Service（未验证）,需要将隐式的intent转换为显式的intent，转换方法为：
```
        Intent intent = new Intent();
        intent.setPackage("com.ljp.messenger_server");
        intent.setAction("com.ljp.messenger_server.server.action");//以上构成了隐式的intent
        PackageManager packageManager = getPackageManager();
        ResolveInfo resolveInfo = packageManager.resolveService(intent, 0); //我们先通过一个隐式的Intent获取可能会被启动的Service的信息，
        if (resolveInfo!=null){//系统中有这个意图，说明我们能通过上面隐式的Intent找到对应的Service
            String packageName = resolveInfo.serviceInfo.packageName;//得到的Service的包名
            String name = resolveInfo.serviceInfo.name;//得到的Service的类名
            ComponentName componentName=new ComponentName(packageName,name);
            intent.setComponent(componentName);//已经转为了显式的intent了
            Log.e(TAG, "bind: 已经转为了显式的intent了" );
        }
```
##### 4、Messenger的构造方法的使用
```
    public Messenger(Handler target) {//一般用于需要接受消息的一端。
        mTarget = target.getIMessenger();
    }
    public Messenger(IBinder target) {//一般使用在客户端。
        mTarget = IMessenger.Stub.asInterface(target);
    }
```
##### 5、使用场景：少量的一对多的串行进程间通信，支持实时通信，使用比较简单，不支持直接调用服务端的方法（可以通过msg.what进行间接调用并返回调用结果）。


 我的CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/78674323