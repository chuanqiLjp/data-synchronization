---
title: Android 自定义字体，更换系统默认显示的字体使用自定义字体
layout: post
date: 2018-01-29 10:26:58
comments: true
categories:
  - Android
  - Android字体
tags: [Android 自定义字体,自定义系统字体]
keywords: Android 自定义字体,自定义系统字体
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：转载需要在醒目注明出处


### 序言：

可以免费下载字体的网站：[字体下载大宝库](http://font.knowsky.com/)；[找字网](http://www.zhaozi.cn/)
还有一个字体编辑软件：[FontCreator9.0绿色汉化特别版](http://www.zhaozi.cn/html/prog/47.html)
可以自定义字体的第三方库：[Calligraphy](https://github.com/chrisjenx/Calligraphy)



# 1、指定控件显示指定字体
有时为了美化UI，需要在指定控件中显示特定的字体，而这个字体在Android系统中却没有，此时可将需要的字体文件存放在assets文件夹中，在为控件设置Typeface ：View.setTypeface(Typeface.createFromAsset(getAssets(), "fonts/Nsimsun.ttf"))或View.setTypeface(Typeface.createFromFile(File))，如果软件中多次使用可自定义View简化使用流程；

**注意**：
> 如果有多处使用这个字体文件，如果在每次调用的时候都这样写，会造成每次执行的时候都会重新加载一次该字体，导致内存不断变大，造成内存泄漏。其解决方案是将加载的字体文件Typeface定义为一个常量，需要的时候拿来用就行了！


```
        <!--
        修改显示的字体
        android:typeface="normal(正常)|sans(无)|monospace(等宽字体)|serif(衬线)"
        -->
        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="20dp"
            android:gravity="center"
            android:text="默认的字体 sans —>李㘮"
            android:textAllCaps="false"
            android:textSize="30sp"
            android:typeface="serif"/>

        <TextView
            android:id="@+id/id_ywsflsjtForView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="20dp"
            android:gravity="center"
            android:text="1、指定控件显示指定字体—>禹卫书法隶书简体的字体—>李㘮"
            android:textAllCaps="false"
            android:textSize="30sp"/>

        <TextView
            android:id="@+id/id_NsimsunForView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="20dp"
            android:gravity="center"
            android:text="1、指定控件显示指定字体（解决部分字无法显示）—>Nsimsun的字体—>李㘮"
            android:textAllCaps="false"
            android:textSize="30sp"/>
```


```
        //1、指定控件显示指定字体
        tvYwsflsjtForView = (TextView) findViewById(R.id.id_ywsflsjtForView);
        tvYwsflsjtForView.setTypeface(Typeface.createFromAsset(getAssets(), "fonts/ywsflsjt.ttf"));//禹卫书法隶书简体的字体
        //1、指定控件显示指定字体（解决部分字无法显示）
        tvNsimsunForView = (TextView) findViewById(R.id.id_NsimsunForView);
        tvNsimsunForView.setTypeface(Typeface.createFromAsset(getAssets(), "fonts/Nsimsun.ttf"));
```





# 2、整个软件显示指定字体

当整个软件都需要使用自定义字体，我们就需要对全部显示的控件字体进行自定义，如果再使用1中的方法难免显得麻烦，此时需要自定义Application，在程序刚启动的使用进行系统中
Typeface类的字体变量的引用替换（就是某一字体变量原来引用系统的某一字体如：MONOSPACE，现在将该变量的引用指向我们自定义的字体如：/assets/fonts/Nsimsun.ttf），从而实现整个软件的字体自定义的过程，这里由于系统没有提供相关方法，我们采用反射进行实现。但是需要值得注意的是需要替换的字体变量名也需要在AppTheme进行声明：<item name="android:typeface">serif</item>，替换后有效果的字体变量名有：MONOSPACE、SERIF，Aandroid 5.0及以上我们反射修改Typeface.sSystemFontMap变量的值；
*另一种方式是递归遍历所有的View然后进行替换，但是工作量还是不小，不推荐。*


```
        <TextView
            android:id="@+id/id_ywsflsjtForAPP"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="20dp"
            android:gravity="center"
            android:text="2、整个软件显示指定字体—>禹卫书法隶书简体的字体—>李㘮(代码在Application中)"
            android:textAllCaps="false"
            android:textSize="30sp"/>

        <TextView
            android:id="@+id/id_NsimsunForAPP"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="20dp"
            android:gravity="center"
            android:text="2、整个软件显示指定字体（解决部分字无法显示）—>Nsimsun的字体—>李㘮(代码在Application中)"
            android:textAllCaps="false"
            android:textSize="30sp"/>
```

在AndroidMainfest文件的application节点的的字段 theme的值引用的AppTheme文件中新增节点,
```
    <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="android:typeface">serif</item>
    </style>
```
新增Application文件并在AndroidMainfest文件中引用

```
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        //替换整个APP中没有自定义字体的控件的显示字体，即该软件显示指定字体
//        MyApplication.replaceFont(this, "MONOSPACE", "fonts/ywsflsjt.ttf");  // OK ，需要与<item name="android:typeface">monospace</item> 的值对应
//        MyApplication.replaceFont(this, "NORMAL", "fonts/ywsflsjt.ttf"); // 程序无法运行
//        MyApplication.replaceFont(this, "SANS", "fonts/ywsflsjt.ttf"); // 可以运行，但是显示字体没有修改成功
        MyApplication.replaceFont(this, "SERIF", "fonts/ywsflsjt.ttf"); // OK,需要与<item name="android:typeface">serif</item> 的值对应
//        MyApplication.replaceFont(this, "SERIF", "fonts/Nsimsun.ttf"); //整个软件显示指定字体（解决部分字无法显示）

    }

    /**
     * 替换字体，其本质是将系统底层的字体变量进行替换自己的字体引用 ,
     * 只会替换控件中没有自定义字体的控件，已自定义的就是使用的是自定义的字体
     *
     * @param context
     * @param oldFontName           　支持的名称有 MONOSPACE、SERIF，NORMAL（程序无法运行）、SANS与DEFAULT和DEFAULT_BOLD与SANS_SERIF（可以运行但是显示字体没有修改成功）
     *                              而且需要与 需要与AndroidManifest文件application节点的android:theme引用的styles文件中
     *                              <item name="android:typeface">monospace</item> 的值对应
     * @param newFontNameFromAssets 新的字体路径，必须要放在assets文件夹下，如：fonts/Nsimsun.ttf
     */
    public static void replaceFont(Context context, String oldFontName, String newFontNameFromAssets) {
        Typeface newTypeface = Typeface.createFromAsset(context.getAssets(), newFontNameFromAssets);
        try {
            //android 5.0及以上我们反射修改Typeface.sSystemFontMap变量
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
                Map<String, Typeface> newMap = new HashMap<>();
                newMap.put(oldFontName, newTypeface);
                final Field staticField = Typeface.class.getDeclaredField("sSystemFontMap");
                staticField.setAccessible(true);
                staticField.set(null, newMap);
            } else {
                final Field staticField = Typeface.class.getDeclaredField(oldFontName);
                staticField.setAccessible(true);
                staticField.set(null, newTypeface);
            }
        } catch (NoSuchFieldException | IllegalAccessException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

# 3、修改整个系统的默认字体

请参考我的另一篇文章：[Android更换系统默认显示的字体使用自定义字体](https://www.jianshu.com/p/b0b541b94427)


# 4、WebView加载指定字体（在获取到网页内容后追加内容）

##### 1.系统中已有字体文件的字体
在需要显示的范围标签内中添加 style="font-family:NSimSun;" ，如需要正文全部显示为该字体则可以在标签<html后添加：<html style="font-family:NSimSun;"> ，系统的字体文件路径：/system/fonts/；



##### ２.引用自定义的字体文件
 将需要用到的字体放在assets文件中，在<head标签中增加一个style，内容如下：

```
    <head>
    <style>
         @font-face {
             font-family: 'MyWebFont';
             src:url('fonts/Nsimsun.ttf') format('truetype');
             font-weight: normal;
             font-style: normal;
         }
    </style>
    </head>
```

在需要的标签中添加style，如：<body style="font-family:MyWebFont;">

上面是使用内联的CSS，如果字体文件较多，可将CSS文件单独分离出来，新建一个MyFonts.css的文件，内容为：


```
@font-face {
    font-family: 'MyWebFont';
    src:url('fonts/Nsimsun.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
}
/*多个文件依次往下排列*/
@font-face {
    font-family: 'MyWebFont2';
    src:url('fonts/ywsflsjt.ttf') format('truetype');
    font-weight: normal;
    font-style: normal;
}
```


注意，这个就不用包含在style标签中了，在新建一个bodyFont.css的文件，内容为：

```
body {
    font-family:'MyWebFont2'; /*为body部分MyFonts.css文件中的MyWebFont2中声明的ywsflsjt字体*/
}
```

然后在head标签中添加内容：
```
<head>
<link href="file:///android_asset/MyFonts.css" rel="stylesheet" type="text/css"/>
<link href="file:///android_asset/bodyFont.css" rel="stylesheet" type="text/css"/>
</head>
```




###更多参考：
-  [关于Android7.0字体全局替换的研究](http://www.miui.com/thread-8343134-1-1.html)

- [Android修改全局字体样式，替换整个APP字体](http://blog.csdn.net/Gold_brick/article/details/52865369)

- [ANDROID：更好的自定义字体方案](http://ryanhoo.github.io/blog/2014/05/05/android-better-way-to-apply-custom-font/)

- [Android 4.4.2 系统源码字体库精简、添加](http://chenggoi.com/2015/01/07/Android_Fonts_Customizing/ "Android 4.4.2 系统源码字体库精简、添加")

- [Android字体工作原理与应用](http://blog.csdn.net/flyeek/article/details/44057999)


- **Android 字体修改，所有的细节都在这里 系列【想深入研究强烈推荐】**
>  [Android 字体修改，所有的细节都在这里 | 开篇](https://segmentfault.com/a/1190000011299402)
 [Android 修改字体，跳不过的 Typeface](https://segmentfault.com/a/1190000011299442)
 [粗暴的方式，替换全局字体](https://segmentfault.com/a/1190000011401716)
 [全局修改默认字体，通过反射也能做到](https://segmentfault.com/a/1190000011401796)
[利用 AppCompatDelegate ,半小时就能在成熟项目上全局替换字体！](http://mp.weixin.qq.com/s?__biz=MzIxNjc0ODExMA==&mid=2247484784&idx=1&sn=9d52e1e4bbf11d03d8617ebba3356673&chksm=97851c51a0f2954734f82fdd4f4b896599667ee9dfdec04cb36498c49de62110d8b6c13b6496#rd)
[通过修改 LayoutInflater，全局替换字体！！！](https://segmentfault.com/a/1190000011475000)
[设计师说，我们要在 App 里用十种字体！！！](http://mp.weixin.qq.com/s?__biz=MzIxNjc0ODExMA==&mid=2247484806&idx=1&sn=8c35a7cf49c72b2d24c11965247f8c05&chksm=97851ca7a0f295b14d02ce92d42c53eafdf930b08a61197a29126aa562c646c37bc201f25c15#rd)
[Android Oreo 可下载字体](http://mp.weixin.qq.com/s?__biz=MzIxNjc0ODExMA==&mid=2247484818&idx=1&sn=893e0540a6fc889c6ef84703cad1ac46&chksm=97851cb3a0f295a52db0a5ea6a5f18dcc6c6106d1866cfb7dbb59b6816e9b7cd685ed21d9fba#rd)
[全局替换字体，开源库更方便！！！](https://segmentfault.com/a/1190000011604008)
[看完九篇字体系列的文章，你还觉得我是在说字体？](https://segmentfault.com/a/1190000011693615)


我的CSDN博客：http://blog.csdn.net/wo_ha/article/details/79193141
