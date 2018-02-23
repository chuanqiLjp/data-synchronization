CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/54138417
一、Android官方推出的百分比布局的使用

1、导入依赖
```
dependencies {
    compile 'com.android.support:percent:25.0.+'
}
```
2、提供了如下的属性
```
支持的布局有：PercentRelativeLayout，PercentFrameLayout
属性如下：
heightPercent
widthPercent
marginBottomPercent
marginEndPercent
marginLeftPercent
marginPercent
marginRightPercent
marginStartPercent
marginTopPercent
```
更多请参考：https://juliengenoud.github.io/android-percent-support-lib-sample/（需要正确上网）

二、Android官方增强版百分比布局的使用——推荐使用

注：在官方的基础上增加了布局PercentLinearLayout，支持百分比设置正方形，未改变官方原有的使用，支持设置字体的百分比，因此更推荐使用
1、导入依赖
```
dependencies {
    //...
    compile 'com.zhy:percent-support-extends:1.1.1'
}
```
2、支持的布局有
```
com.zhy.android.percent.support.PercentLinearLayout
com.zhy.android.percent.support.PercentRelativeLayout
com.zhy.android.percent.support.PercentFrameLayout
```
3、支持的属性有
```
支持的属性 :
layout_heightPercent
layout_widthPercent
layout_marginBottomPercent
layout_marginEndPercent
layout_marginLeftPercent
layout_marginPercent
layout_marginRightPercent
layout_marginStartPercent
layout_marginTopPercent
layout_textSizePercent
layout_maxWidthPercent
layout_maxHeightPercent
layout_minWidthPercent
layout_minHeightPercent
layout_paddingPercent
layout_paddingTopPercent
layout_paddingBottomPercent
layout_paddingLeftPercent
layout_paddingRightPercent
对于值可以取：10%w , 10%h , 10% , 10%sw , 10%sh
```
4、使用实例
```
<com.zhy.android.percent.support.PercentRelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:percent="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ImageButton
        android:src="@mipmap/ic_launcher"
        android:scaleType="fitXY"
        percent:layout_heightPercent="50%"
        percent:layout_widthPercent="50%" />
</com.zhy.android.percent.support.PercentRelativeLayout>
```
效果图

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-1043f1eaf0ddad71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    <ImageButton
        android:scaleType="fitXY"
        android:src="@mipmap/ic_launcher"
        percent:layout_heightPercent="50%w"
        percent:layout_widthPercent="50%" />
效果图
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-8b0c743c7f28b3d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    <ImageButton
        android:scaleType="fitXY"
        android:src="@mipmap/ic_launcher"
        percent:layout_heightPercent="50%"
        percent:layout_widthPercent="50%h" />
效果图

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-fe89c0dea8329232.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

    <ImageButton
        android:src="@mipmap/ic_launcher"
        android:scaleType="fitXY"
        percent:layout_heightPercent="50%sh"
        percent:layout_widthPercent="50%sw" />
效果好像与直接使用xx%差不多，但是交换过来好像就不一样了
效果图

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/4143664-5134c69506624f3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#使用总结：

#####1.属性就是在Android原有的属性后增加Percent，如android：layout_height —>android：layout_heightPercent
#####2.百分号后面的单位，如10%w ：占手机屏幕宽度的十分之一, 10%h：占手机屏幕高度的十分之一 , 10% ：占手机屏幕宽/高度的十分之一, 10%sw、10%sh 与10%w、10%h基本相同

更多使用请参考：
https://github.com/hongyangAndroid/android-percent-support-extend
http://blog.csdn.net/lmj623565791/article/details/46767825