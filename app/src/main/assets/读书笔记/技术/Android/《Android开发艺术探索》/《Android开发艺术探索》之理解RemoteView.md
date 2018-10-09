1. RemoteView: 一种远程的View，可以在其他进程中显示，常见的为通知栏和桌面小部件,支持的View类型有限:FrameLayout,LinearLayout,RelativeLayout,GridLayout,Button,ImageView,TextView,PrograssBar(注:不支持EditText)
2. 通知栏： NotificationManager(管理通知，通过notify和cancel方法)，NotificationCompat.Builder构建通知对象Notification,PendingIntent进行页面跳转,RemoteView进行通知栏页面的自定义;
3. 横幅通知栏(注意:华为手机无法显示)
```
        Random random=new Random();
        Intent intent = new Intent();
        PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, PendingIntent.FLAG_ONE_SHOT);
        NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
        builder.setContentTitle("横幅通知")
                .setContentText("请在设置通知管理中开启消息横幅提醒权限")
                .setSmallIcon(R.mipmap.ic_launcher)
                .setDefaults(Notification.DEFAULT_ALL)
                .setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.ic_launcher)).setContentIntent(pIntent)
                .setFullScreenIntent(pIntent, true)//这句是重点
                .setVisibility(Notification.VISIBILITY_PUBLIC)
                .setAutoCancel(true);
        Notification notification = builder.build();
        notificationManager.notify(1+random.nextInt(100), notification);
```