---
title:  Android中的shape使用笔记和阴影的编码
author: chuanqiljp
layout: post
date: 2018-06-08 09:52:58
comments: true
categories:
  - Android
  - View相关
tags: [shape,阴影]
keywords: shape,阴影
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/


## 版权声明：商业转载请联系我获得授权，非商业转载请在醒目位置注明出处。

# 1.标签shape 
```
        <!-- shape 形状
        android:shape=["rectangle" | "oval" | "line" | "ring"]  shape的形状，默认为矩形，可以设置为矩形（rectangle）、椭圆形(oval)、线性形状(line)、环形(ring)
        下面的属性只有在android:shape="ring时可用：
        android:innerRadius       尺寸，内环的半径。
        android:innerRadiusRatio  浮点型，以环的宽度比率来表示内环的半径，
        例如，如果android:innerRadiusRatio，表示内环半径等于环的宽度除以5，这个值是可以被覆盖的，默认为9.
        android:thickness           尺寸，环的厚度
        android:thicknessRatio      浮点型，以环的宽度比率来表示环的厚度，例如，如果android:thicknessRatio="2"，
        那么环的厚度就等于环的宽度除以2。这个值是可以被android:thickness覆盖的，默认值是3.
        android:useLevel            boolean值，如果当做是LevelListDrawable使用时值为true，否则为false.
        -->
```

# 2. shape 标签下的gradient  标签
```
            <!--
               gradient  渐变色
               android:startColor  颜色值          起始颜色
               android:endColor    颜色值          结束颜色
               android:centerColor 整型           渐变中间颜色，即开始颜色与结束颜色之间的颜色
               android:angle       整型           渐变角度(PS：当angle=0时，渐变色是从左向右。 然后逆时针方向转，当angle=90时为从下往上。
                                                angle必须为45的整数倍)
               android:type        ["linear" | "radial" | "sweep"] 渐变类型(取值：linear、radial、sweep)
                                   linear 线性渐变，这是默认设置
                                   radial 放射性渐变，以开始色为中心。
                                   sweep 扫描线式的渐变。
              android:useLevel   ["true" | "false"] 如果要使用LevelListDrawable对象，就要设置为true。设置为true无渐变。false有渐变色
              android:gradientRadius 整型          渐变色半径.当 android:type="radial" 时才使用。单独使用 android:type="radial"会报错。
              android:centerX      整型            渐变中心X点坐标的相对位置
              android:centerY      整型            渐变中心Y点坐标的相对位置
           -->
```

# 3. shape 标签下的solid  标签
```
            <!--solid  内部填充
                android:color   颜色值 填充颜色
            -->
```
# 4. shape 标签下的corners  标签
```
            <!--
                corners  圆角
                android:radius              整型 半径   圆角的半径 值越大角越圆
                android:topLeftRadius       整型 左上角半径
                android:topRightRadius      整型 右上角半径
                android:bottomLeftRadius    整型 左下角半径
                android:bottomRightRadius   整型 右下角半径
             -->
```


# 5. shape 标签下的padding   标签
```
            <!--
                padding   内边距，即内容与边的距离
                android:left    整型 左内边距
                android:top     整型 上内边距
                android:right   整型 右内边距
                android:bottom  整型 下内边距
              -->
```


# 6. shape 标签下的size 标签
```
            <!--
                size 大小
                android:width   整型 宽度
                android:height  整型 高度
            -->
```
# 7. shape 标签下的stroke   标签
```
            <!-- stroke   描边
               android:width       整型  描边的宽度
               android:color       颜色值     描边的颜色
               android:dashWidth   整型  表示描边的样式是虚线的宽度， 值为0时，表示为实线。值大于0则为虚线
               android:dashGap     整型  表示描边为虚线时，虚线之间的间隔.
            -->
```

# 8.使用layer-list(图集)制作控件或布局的阴影
### 1.在drawable文件夹下创建xml文件并编码
```
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">


    <!-- 阴影部分的Item图像定义 -->
    <!-- 个人觉得更形象的表达：top代表下边的阴影高度，left代表右边的阴影宽度。其实也就是相对应的offset，solid中的颜色是阴影的颜色，也可以设置角度等等 -->

    <item
        android:bottom="0dp"
        android:left="0dp"
        android:right="0dp"
        android:top="0dp">
        <shape android:shape="rectangle">

            <!--
            Android:angle 渐变角度，0从左到右，90表示从下到上，数值为45的整数倍，默认为0；
            Android:type  渐变的样式 liner线性渐变 radial环形渐变 sweep
           渐变颜色 0F000000 -> 4A9c9ca0
            -->
            <gradient
                android:angle="270"
                android:centerColor="#4A9c9ca0"
                android:centerX="50%"
                android:centerY="50%"
                android:endColor="#4A9c9ca0"
                android:startColor="#0F000000"
                android:type="linear"/>

            <corners
                android:bottomLeftRadius="15dp"
                android:bottomRightRadius="15dp"
                android:topLeftRadius="15dp"
                android:topRightRadius="15dp"/>
        </shape>
    </item>
    <!-- 背景部分的Item图像定义 -->
    <!-- 形象的表达：bottom代表背景部分在上边缘超出阴影的高度，right代表背景部分在左边超出阴影的宽度（相对应的offset） -->
    <item
        android:bottom="3dp"
        android:left="3dp"
        android:right="3dp"
        android:top="3dp">
        <shape android:shape="rectangle">

            <gradient
                android:angle="0"
                android:endColor="#FFf"
                android:startColor="#FFf"/>

            <corners
                android:bottomLeftRadius="10dp"
                android:bottomRightRadius="10dp"
                android:topLeftRadius="10dp"
                android:topRightRadius="10dp"/>
        </shape>
    </item>
</layer-list>
```
### 2. 使用时直接设置该文件为背景即可,
```
android:background="@drawable/img_bg_shadow"
```
来一个效果图
![灰色部分就是阴影](https://upload-images.jianshu.io/upload_images/4143664-935441e44411d4bf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

