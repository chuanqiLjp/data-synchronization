---
title: HTML标签学习记录
layout: post
date: 2017-12-18 22:48:00
comments: true
categories:
  - HTML
tags: [html,笔记]
keywords: html,笔记
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：转载需要在醒目注明出处

#序言
整理谷歌的小弟的笔记，版权归原作者所有，本文仅作整理，原文链接：http://blog.csdn.net/lfdfhl/article/list/2

# 常用标签
###p标签
p标签在HTML中常用于表示段落，它是英文单词paragraph的缩写。p标签的用法非常简单，只需要在标签中放置一段文本即可。
```
<p>2017，顶着刘海的iPhoneX带着“史上升级变动最大”的iOS11，依然碎片化严重的Android带着“更快、更强大、更安全” 的8.0来到我们面前，忽思十年初，那个触 屏的、没有物理键盘的智能手机惊艳了我 ，但连个复制粘贴功能都没有的时光。回首一顾，从2007到2017，从诺记的Symbian、摩托罗拉的Linux、苹果的iOS、微软的Windows Phone、三星的 Tizen到Google的Android等，移动操作系统也曾百花齐放，但经过十年厮杀各自蚕食，格局已相当明朗，只剩下了iOS和Android两大巨头</p>
```
在浏览器中运行之后可以发现：p标签的上下均有大约一行宽度的留白，这和我们平时看见的文章的每个段落是一样的。

### h标签
h标签用于表示标题，它是英文单词header的缩写。我想这个单词对于Android程序员来说是再熟悉不过了吧；例如，给ListView，Recyclerview设置header和footer。在HTML中h标签细分为h1~h6，请注意：h后的数字越大，那么标题所对应的字体越小。示例如下：
```
<h1>这里是h1</h1>
<h2>这里是h2</h2>
<h3>这里是h3</h3>
<h4>这里是h4</h4>
<h5>这里是h5</h5>
<h6>这里是h6</h6>
```

### **hr标签**

hr标签用于表示一条水平横线，它是英文中Horizontal Rule的缩写。它的用法非常简单：

```
<hr>
```

在页面中只用写一个hr标签，就可以表示一条水平横线。

### **br标签**

br标签用于表示换行，它在英文所对应的单词是break。它的用法也非常简单：

```
<br>
```

### **nobr标签**

会了刚才的br标签，再来看nobr就很简单了；该标签表示不换行。比如，我们想表示一个很长的数学公式，需要将其显示在同一行，从而避免换行后导致可读性变差产生歧义。

```
<nobr>这里是一个很长的数学公式，不能换行显示，只能在一行显示。这里是一个很长的数学公式，不能换行显示，只能在一行显示。这里是一个很长的数学公式，不能换行显示，只能在一行显示。这里是一个很长的数学公式，不能换行显示，只能在一行显示。</nobr>
```

### **center标签**

center标签表示居中显示，比如我们想将一句话显示在页面的水平方向的中间，可以这么做：

```
<center>测试center标签</center>
```

### **marquee标签**

marquee标签用于表示跑马灯效果。做Android开发的童鞋还记得不，在TextView中也有类似的属性：android:ellipsize=”marquee”；它们是非常类似的。

```
<marquee behavior="scroll" direction="left">
        <p>测试marquee标签</p>
</marquee>
```

在marquee标签中可以通过behavior和direction属性控制跑马灯的不同效果。

### **button标签**

button标签用于表示按钮，这和我们在Android开发中常用的Button控件是完全一样的。

```
<button type="button" onclick="onButtonClick()">This is a button</button>
    <script type="text/javascript">
        function onButtonClick(){
            alert('You click button');
        }
    </script>
```

此处，我们给button标签设置一个监听器，当用户点击button后利用JavaScript弹出一个对话框。

### **a标签**

a标签在THML中常用于表示锚点和超链接，它是英文中anchor的缩写；在此，我们主要来瞅瞅利用a标签实现超链接。

```
<a href="http://blog.csdn.net/lfdfhl" title="谷哥的小弟" target="_blank">请点击此处的超链</a>
```

在a标签中利用href属性指明超链接的地址，利用title表示当鼠标悬停在超链接时的提示文字，利用target属性表示打开超链接的方式。如果target的取值为_blank表示在新窗口中打开超链接；假若target的取值为_self表示在当前窗口中打开超链接。

### **img标签**

img标签在HTML中常用于表示图像，它是英文中image的缩写。请看如下示例：

```
<img src="myblog.jpg" title="这是我的博客头像">
```

# **文本标签**

文本标签，顾名思义，它是用来显示文本的。在此，我们来瞅瞅HTML中经常使用的文本标签。

### **b标签**

b标签常用于文本加粗，它对应于英文中的bold。

```
<b>b标签用于粗体显示文字</b>
```

### **strong标签**

strong标签的作用和用法与b标签基本相同，但是在HTML5中为strong标签增加了语义，用其表示重要的文本。

```
<strong>strong标签用于粗体显示文本，表示重点内容</strong>
```

### **small标签**

small标签用于显示小号字体，比如：版权信息，法律信息，免责声明

```
<small>本文的原创作者是谷哥的小弟</small>
```

有人在想：既然有了samll标签，那么是不是有对应的big标签呢？嗯哼，以前确实是有这个标签的，但是在HTML5中已经将其删除了。

### **i标签**

i标签用于将文本斜体显示，它源于英语单词italic；常用于显示专业词汇，术语，谚语。

```
<i>service</i>
```

### **em标签**

em标签表示将文本斜体显示，它源于英语单词emphasize.

```
<em>这里是考试的重点</em>
```

看到这里，有的童鞋就有疑问了：i标签和em标签都将文本斜体显示，它们有什么区别么？b标签和strong标签都将文本粗体显示，它们有什么区别呢？

b标签和i标签仅仅表示”此处应该用粗体显示”或者”此处应该用斜体显示”，例如，要突出合同的价格那么可以用b标签粗体显示；要表达一句谚语，可以用i标签将其斜体显示。 
strong标签和em标签是为了强调内容的重要意义而显示粗体或者斜体；对于搜索引擎，爬虫，SEO而言更受重视。例如，我们将”打倒法西斯！”这句话置于strong标签中；那么，语音阅读器时读到此strong标签就会重读。 
概括地来说：b标签和i标签是物理元素 ；strong标签和em标签是逻辑元素。物理元素强调的是一种物理行为。比如说，把一段文字用b标签加粗，意思是告诉浏览器应该加粗显示，没有其他作用；而strong标签不但加粗了字体还起到了强调的作用。同理，i标签和em标签类似，故不再赘述。

### **u标签**

u标签用于表示文本下划线，它源于英文单词underline；请看如下示例：

```
<u>u标签标示文本的下划线</u>
```

### **sup标签**

sup标签用于表示文本的上标，它是英文单词superscript的缩写；请看如下示例：

```
这里是上标<sup>1</sup>
```

### **sub标签**

sub标签用于表示文本的下标，它是英文单词subscript的缩写；请看如下示例：

```
这里是下标<sub>2</sub>
```

### **span标签**

span用于组合文档中的行内元素；它常结合CSS为文本中的某些部分进行特殊处理。说到这里，大家是不是猛然想起来Android里也有类似的东西！比如，要把一部分文字改变颜色，还记得我们在Android里面怎么做的呢？是不是利用SpannableString就可以了？！你瞅瞅，它是不是也是个span呢？所以，这两者是互通的，知道了其中一个，另外一个自然也理解了。

```
<span style="color:#FA0">大家好</span><span style="color:#F00">我是谷哥的小弟</span>
```

###  **font标签**

font标签用于给文本设置文字大小和颜色等属性。示例如下：

```
<font color="red" size="15">测试font</font>
```

虽然font标签可以给文本设置样式，但是在HTML5中建议不再使用该标签，可采用CSS实现相同的功能。

# **语义标签**

在讲这类标签之前，我们先来聊聊标签的语义化。 
HTML5标签语义化的目的：让程序员(甚至是非IT人士)能够直观地认识到标签及其属性的用途和作用。比如，当我们看到h1~h6时就知道：这个标签是用来显示标题的。当然，语义化还有其他非常重要的作用。通过语义化标签可以让爬虫，搜索引擎，SEO读懂我们的页面。比如，我们利用HTML5开发一款新闻朗读软件给盲人朋友用，如果我们把重点内容放入strong标签中，那么该内容会被重读从而突出重点。

###  **blockquote标签**

blockquote用于表示文本的引用。引用的文本会在左、右两侧同时缩进；请看如下示例：

```
<blockquote cite="http://blog.csdn.net/lfdfhl/article/details/77825765">代理模式(Proxy Pattern)是面向对象中一种非常常见的设计模式。其实，不单是在软件开发领域，在我们的日常生活中对于代理也时常可见。比如：房东要将自家的房租出售，于是到房地产中介公司找一个代理，由他来帮自己完成销售房屋，签订合同等等事宜。</blockquote>
```

在该标签中，可使用cite属性标明引用内容的来源。

###  **cite标签**

刚才我们看到cite是blockquote标签的中的一个属性；其实，cite还可以单独作为一个标签使用。cite标签用于表示文本对某个参考文献的引用；比如书籍或者杂志的标题；请看如下示例：

```
这段话出自<cite>《java编程思想》</cite>
```

###  **address标签**

address标签用于表示地址，显示效果通常为斜体，请看如下示例：

```
<address>中国四川省成都市高新区</address>
```

###  **code标签**

code标签用于表示计算机代码，请看如下示例：

```
<code>system.out.println()</code>
```

###  **var标签**

var标签用于表示变量，请看如下示例：

```
<var>count</var>
```

###  **dfn标签**

dfn标签用于定义专业术语，它源于短语defining instance，请看如下示例：

```
<dfn>量子网络通信</dfn>
```

###  **del标签**

del标签用于表示删除，在该标签中的文本会被画一条横线，请看如下示例：

```
<del>该方法已经废弃</del>
```

###  **pre标签**

pre标签表示预先的格式化，它源自于英语单词preformatted，该标签中的空格，回车等格式字符都会被保留。请看如下示例：

```
<pre>
        <p>       第一行文字</p>

        <p>第二行文字</p>
</pre>
```

###  **mark标签**

mark标签用于标记文本中的重点内容，默认采用荧光色标记。请看如下示例：

```
<mark>排序算法是我们面试的重点</mark>
```

###  **details和summary标签**

details标签用于表示详细信息；summary标签常用于表示摘要信息；两者常结合起来使用。请看如下示例：

```
<details>
    <summary>java编程思想</summary>
    这本书写得非常好，值得一看
</details>
```

<small></small>

# **结构标签**

我们在HTML页面中常用一些标签将页面划分为不同的区域用以表示页面结构。比如，可使用div标签将整个页面分为header，body，footer三部分。现在我们就来学习这些与页面结构有关的标签。

###  **div标签**

div标签在页面中非常常见，也常将其称为标签容器。我们可以将一组功能相关的标签放到同一个div中，也可以对该标签内的元素作统一处理，比如设置对齐方式，背景颜色。请看如下示例：

```
<strong>学习div标签</strong>
<div align="center" style="color:#0000FF">
      <p>这是div中一个p标签</p>
      <p>这是div中另外一个p标签</p>
</div>
```

但是，请注意：div标签本身没有任何语义，多用作布局以及样式化或脚本的钩子(hook)。正如，官方文档所言：

> The div element has no special meaning at all

所以，在页面中大量使用div标签导致页面的语义性下降。因此HTML5中引入了新的结构标签article和section

###  **section标签**

先来瞅瞅section标签的文档释义：

> The section element represents a generic section of a document or application. A section , in this context, is a thematic grouping of content, typically with a heading

这段话的大概含义是：section不仅仅是一个标签容器，它带有明显的语义。该标签常用于对网站或者应用程序中页面上的内容进行分块。比如，网站的主页可以分成简介、新闻和联系方式等几部分，那么每一部分都可以放到一个section里面；类似地，文章的章节、标签对话框中的标签页、或者论文中有编号的部分也可以放到一个section中。请注意：官方文档建议，在使用section时每个section标签中应带一个标题标签(h1-h6)，从而表达更清晰的语义。

### <a name="t5" target="_blank" style="word-wrap: break-word; box-sizing: border-box; color: rgb(12, 137, 207);"></a>**article标签**

先来瞅瞅article标签的文档释义：

> The article element represents a self-contained composition in a document, page, application, or site and that is, in principle,independently distributable or reusable, e.g. in syndication.

这段话的大概含义是：article是一个特殊的section标签，它比section更具有明确的语义。也就是说：无论从结构上还是内容上来说，它代表一个独立的、完整的相关内容块。例如：博客中的一篇文章，论坛中的一个帖子或者一段浏览者的评论等。一般来说，article也有标题部分(通常包含在header内)，类似地它还有footer部分。

# **列表标签**

### **ul标签**

可能猛地一下看到ul不知道它是干嘛的。可是，如果我告诉你它源自于英语短句unordered list，你是否就反应过来了呢？对的，它用于表示无序列表。请看如下示例：

```
<ul>
        <li>华为</li>
        <li>三星</li>
        <li>小米</li>
        <li>锤子</li>
</ul>
```

### <a name="t4" target="_blank" style="word-wrap: break-word; box-sizing: border-box; color: rgb(12, 137, 207);"></a>**ol标签**

看到ol，调皮的同学一下子就想到了office lady，于是乎就开始莫名地兴奋了，白领，制服，丝袜。。。。。浮想联翩。嗯哼，不好意思，你想多了，此处的ol源自于英语短句ordered list，它用于表示有序列表。请看如下示例：

```
<ol>
        <li>华为</li>
        <li>三星</li>
        <li>小米</li>
        <li>锤子</li>
</ol>
```

### **dl标签**

dl标签表示定义一个定义列表，它源于英语短句definition list。请看如下示例：

```
<dl>
        <dt>Android四大组件</dt>
        <dd>Activity</dd>
        <dd>广播接收者</dd>
        <dd>内容提供者</dd>
        <dd>服务</dd>
        <dt>Android常用布局</dt>
        <dd>线性布局</dd>
        <dd>相对布局</dd>
        <dd>百分比布局</dd>
</dl>
```

# **表格标签**

在HTML中与表格相关的标签简述如下：

*   table标签用于展示表格

*   caption标签用于显示表格的标题

*   tr标签用于表示表格的行

*   th标签用于表示表格的表头单元格

*   td标签用于表示单元格

OK，来吧，我们写个表格瞅瞅。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML表格标签</title>
</head>
<body>

    <table  border="1" width="600"  height="400" align="center"
    bgcolor="pink" cellspacing="0" cellpadding="0">

    <caption><h2>中国著名演员</h2></caption>

    <tr>
        <th>姓名</th>
        <th>年龄</th>
        <th>性别</th>
        <th>城市</th>
    </tr>

    <tr align="center">
        <td>李冰冰</td>
        <td>32</td>
        <td>女</td>
        <td>北京</td>
    </tr>

    <tr align="center">
        <td>范冰冰</td>
        <td>20</td>
        <td>女</td>
        <td>上海</td>
    </tr>

    <tr align="center">
        <td>刘德华</td>
        <td>47</td>
        <td>男</td>
        <td>香港</td>
    </tr>

</table>
</body>
</html>
```

运行代码后，在浏览器中的效果图如下所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-d7aafbc44da0b77a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在该示例中，我们还用到了不少与表格标签相关的属性，比如利用border表示表格的边框，利用cellspacing 设置单元格之间的距离，利用cellpadding设置单元格中文字距离单元格边框的距离，利用align设置对齐方式，利用bgcolor设置背景颜色。请注意：刚才提到的这些属性在THML5中建议开发人员不再在HTML中直接使用，而应该把这些与样式相关的属性全部放到CSSS中去。

# **表单标签**

HTML表单用于搜集用户输入的不同类型的数据并将其上传至服务端。嗯哼，了解完表单的作用，我们就来一起学习表单中最常用的标签。

###  **input标签**

input标签是表单中功能最丰富的标签，以下几种输入元素均可使用input实现。

*   单行文本框 
    只需将input标签的type属性设置为text即可

*   密码输入框 
    只需将input标签的type属性设置为password即可

*   数字输入框 
    只需将input标签的type属性设置为number即可

*   邮箱输入框 
    只需将input标签的type属性设置为email即可

*   日期输入框 
    只需将input标签的type属性设置为date即可

*   时间输入框 
    只需将input标签的type属性设置为time即可

*   颜色输入框 
    只需将input标签的type属性设置为color即可

*   单选框 
    只需将input标签的type属性设置为radio即可

*   复选框 
    只需将input标签的type属性设置为checkbox即可

*   文件上传 
    只需将input标签的type属性设置为file即可

*   提交 
    只需将input标签的type属性设置为submit即可

*   重置 
    只需将input标签的type属性设置为reset即可

###  **select和option标签**

利用select和option标签可实现下拉选择，比如用户注册时的省份选择。

###  **textarea标签**

利用textarea标签可在HTML中创建供用户输入的文本区域

* * *

##  **表单的提交**

嗯哼，利用刚才提到的这些标签就可以实现简单的表单页面了；在此之后我们需要将表单提交至服务器。在此介绍与表单提交有关的几个属性。

###  **action**

处理表单数据的服务器地址

###  **method**

提交表单的方式，常用的为get和post

###  **enctype**

enctype表示将表单数据发送到服务器之前对表单数据进行编码。它有三种取值：

*   application/x-www-form-urlencoded：此为默认方式，在发送数据前将数据中的特殊字符进行URL编码处理。比如，将空格变为+号，将特殊符号转换为 ASCII HEX 值。

*   text/plain：该取值的作用与application/x-www-form-urlencoded非常类似，它也将表示将空格转换为 “+” 加号，但不对特殊字符编码

*   multipart/form-data：表示不对字符编码。在使用包含文件上传控件的表单时，必须使用该值。

    其实，这和我们之前写Android代码是非常类似的，是不是觉得很眼熟？比如，在APP中上传图片，我们会设置：

    > multipartBodyBuilder.setType(MultipartBody.FORM);

    点开源码就会发现MultipartBody.FORM的值正是multipart/form-data.所以，这不是什么新鲜玩意，它是我们的老朋友啦！

###  **target**

提交表单数据后，服务器会作出相应的响应；所以，我们可以在浏览器中显示服务器返回的数据。那么，是在原来的窗口显示数据呢？还是新打开一个窗口呢？此时可通过target属性来指定显示方式。target属性值常用的有：

*   _self 
    它表示在原窗口中显示数据

*   _blank 
    它表示在新窗口中显示数据

* * *

##  **表单示例**

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML表单</title>
</head>
<body>
    <form id="userform" action="your url" method="post"
    title="用户注册表单" target="_self" enctype="multipart/form-data">
        <fieldset>
            <legend>用户注册信息</legend>
            <br>
            昵称:<input type="text" name="un" maxlength="15" value="Tom">
            <br>
            <br>
            密码:<input type="password" name="pw" maxlength="10">
            <br>
            <br>
            性别:<input type="radio" name="gender" value="m" checked="checked">男
            <input type="radio" name="gender" value="w">女
            <br>
            <br>
            头像:<input id="userphoto" type="file" name="profile">
            <br>
            <br>
            籍贯:<select name="province">
                <option >河北</option>
                <option >辽宁</option>
                <option >吉林</option>
                <option >云南</option>
                <option selected="selected">广西</option>
            </select>
            <br>
            <br>
            爱好:<input name="hobby" type="checkbox">读书
            <input name="hobby" type="checkbox">写字
            <input name="hobby" type="checkbox" checked="checked">弹琴
            <br>
            <br>
            个人简介:
            <br>
            <br>
            <textarea name="introduce" cols="30" rows="10">请在此输入简介</textarea>
            <br>
            <br>
            个人网站:<input name="userurl" type="url">
            <br>
            <br>
            个人邮箱:<input name="useremail" type="email">
            <br>
            <br>
            身体体重:<input name="userweight" type="number">
            <br>
            <br>
            出生日期:<input name="userdate" type="date">
            <br>
            <br>
            详细时间:<input name="usertime" type="time">
            <br>
            <br>
            性格颜色:<input type="color" name="usercolor">
            <br>
            <br>
            <input type="submit" value="开始注册">
            <input type="reset" value="重置信息">
            <br>
            <br>

        </fieldset>
    </form>

</body>
</html>
```

运行后效果图如下所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-3972dc1fb8d5d462.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

结合刚才的示例，在此强调一些需要注意的地方：

*   上传的表单中含有文件时，请选用post方式提交

*   上传的表单中含有文件时，请将enctype属性值设置为multipart/form-data

*   利用多个input标签组合在一起实现单选时，请将它们的 type均设置为radio；并将它们的name均设置为同一值。多选的情况，亦类似；不再赘述

* * *

##  **HTML5中表单的新特性**

###  **form属性**

在HTML5之前，所有的表单标签都必须放在form标签中。但是，在HTML5中新增了form属性，用于表示该标签所属的form标签。所以，每个标签不必必须放在form标签中也能成为表单的一部分，只需把该标签的form属性的值设置为其所属表单的id即可。例如，在刚才的示例中再添加一个输入框用于记录毕业院校：

```
毕业院校:<input type="text" name="school" form="userform">
```

代码如上所示，那么该input标签也属于了userform表单；亦会被提交至服务端。

###  **datalist标签**

datalist标签用于展示文本框与下拉菜单组合在一起的效果，请注意datalist的id值必须是form表单的list属性值。请看如下示例：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>datalist标签</title>
</head>
<body>
    <form action="url" method="get">
        请输入你最喜欢的女明星:<input type="text" name="name" list="namesList">
    </form>
    <datalist id="namesList">
        <option value="lbb">李冰冰</option>
        <option value="fbb">范冰冰</option>
        <option value="gyy">高圆圆</option>
    </datalist>
</body>
</html>
```

运行后效果如下图所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-4aa9b66041a9200d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


###  **formxxxx属性**

为了更加方便的操控表单标签，在HTML5中新增了几个formxxxx属性，简介如下：

*   formaction属性用于指定表单提交的地址

*   formmethod属性用于指定表单提交的方式

*   formtarget属性用于指定打开服务端响应URL的方式

*   formenctype属性用于指定表单数据提交时的编码方式

请看如下示例：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML表单中的formxxx属性</title>
</head>
<body>
    <form>
        username:<input type="text" name="un">
        <br>
        <br>
        password:<input type="password" name="pw">
        <br>
        <br>
        <input type="submit" value="注册" formaction="regist url" formmethod="get" formtarget="_self" formenctype="application/x-www-form-urlencoded" >

        <input type="submit" value="登录" formaction="login url" formmethod="post" formtarget="_blank" formenctype="multipart/form-data">
    </form>
</body>
</html>
```

在该示例中，有两个功能：登录和注册；不同的功能那么就有不同的action、method、target、enctype。在此通过formaction、formmethod、formtarget、formenctype属性灵活指定了在不同的操作下不同的表单提交方式。

 # **H5新增的标签和API**

### **meter标签**

meter标签用于表示度量结果，请看如下示例：

```
笔记本剩余电量:<meter value="7" min="0" max="10"></meter>
```

运行后结果如下图所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-d0d6aa86c2614618.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### **progress标签**

progress标签用于表示进度，请看如下示例：

```
本月已完成工作:<progress value="80" max="100"></progress>
```

运行后结果如下图所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-b50472dfe86ccba4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### **audio标签和video标签**

在HTML5之前若想在网页中播放音频和视频都需要借助第三方插件。现在，HTML5直接提供了audio标签和video标签实现音频(推荐采用ogg格式)，视频(推荐采用VP8格式)的播放。请看如下示例：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML的多媒体标签</title>
</head>
<body>

    <h3>利用audio标签播放音频</h3>
    <audio src="word.mp3" controls="true">
        当您看到这行文字时，意味着您的设备不支持audio标签
    </audio>

    <br>
    <br>

    <h3>利用video标签播放视频</h3>
    <video src="movie.mp4" controls="true">
        当您看到这行文字时，意味着您的设备不支持video标签
    </video>
</body>
</html>
```

运行后界面效果如下图所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-e7350f41d091dad3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* * *

##  **HTML5强大的API**

在HTML5中融入了众多非常实用的功能，比如：控件的拖拽，绘图，多媒体，地理位置，网络状态，数据存储，全屏等等。这部分功能多涉及到JavaScript，但是呢？嘿嘿，我们还没有讲JavaScript呢！在此，我们先体验一把，待我们学完JavaScript再来深入学习这部分知识。

###  **HTML5监听网络状态**

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>HTML监听网络状态</title>
</head>

<body>
    <script type="text/javascript">

    window.addEventListener('online', function() {
        alert('网络连接已建立！');
    });

    window.addEventListener('offline', function() {
        alert('网络连接已断开！');
    })

    </script>
</body>

</html>
```

这玩意儿咋们熟悉不？太熟悉了！咋们在Android里面是不是可以通过监听系统广播判断网络的状态？！

###  **HTML5定位功能**

```
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title>HTML5定位</title>
</head>
<body>
    <script>
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(successCallback, errorCallback);
    } else {
        alert("您的浏览器不支持地理定位");
    }

    // 获取地理位置成功的回调函数
    function successCallback(position) {
        var longitude = position.coords.longitude;
        var latitude = position.coords.latitude;
        alert("经度=" + longitude + "，纬度=" + latitude);
    }

    // 获取地理位置失败的回调函数
    function errorCallback(error) {
        alert("获取用户位置失败");
    }
    </script>
</body>
</html>
```

这玩意陌生不？一点都不陌生！我们在Android里面也经常利用高德地图，百度地图实现定位功能；在HTML5中依然可以！


我的CSDN博客：http://blog.csdn.net/wo_ha/article/details/78808282