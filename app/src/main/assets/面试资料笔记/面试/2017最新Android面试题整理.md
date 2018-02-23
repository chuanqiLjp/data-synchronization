大家好，在跟大家讲述自己的面试经历，以及遇到的面试题前，先说说几句题外话。

接触Android已经3年，在工作中遇到疑难问题总是在网上（csdn大牛博客，stackoverflow等）搜索答案，各位大牛大神总是把自己的经验分享出来，帮助我们这些需要帮助的人，由此表示衷心感谢！然而现在自己细想了一下，自己也是时候把遇到的问题并把解决方案分享出来，希望能帮助到有需要的人。

随着时间的流逝，很多人说互联网这一块已经越来越不好干了，因为烧钱时代已经过去，剩下的都是根基牢固的大公司，独角兽已经不复存在。这就直接导致了互联网岗位的下降，本人亲测，也的确如此。

2017.05月，本人离职（此时3年工作经验，深圳就职），开始试水安卓市场，寻求一份合适自己，稳定的中大型公司。投了很多公司，面试机会并不是我想象中的那么多，即时面试过程顺利，也没有获得offer（候选人太多太多）。不过借此机会，前前后后我面了10家公司，现在就把我遇到的面试题，并且提供一些面试技巧给各位即将面试的同志们。

OK，进入主题，请看Android知识图谱。 
![这里写图片描述](http://upload-images.jianshu.io/upload_images/4143664-8e88723670ccec2c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

面试，无非都是问上面这些问题（挺多的 - -!），聘请中高级的安卓开发会往深的去问，并且会问一延伸二。以下我先提出几点重点，是面试官基本必问的问题，请一定要去了解！

*   **基础知识 – 四大组件（生命周期，使用场景，如何启动）**
*   **java基础 – 数据结构，线程，mvc框架**
*   **通信 – 网络连接（HttpClient，HttpUrlConnetion），Socket**
*   **数据持久化 – SQLite，SharedPreferences，ContentProvider**
*   **性能优化 – 布局优化，内存优化，电量优化**
*   **安全 – 数据加密，代码混淆，WebView/Js调用，https**
*   **UI– 动画**
*   **其他 – JNI，AIDL，Handler，Intent等**
*   **开源框架 – Volley，Gilde，RxJava等（简历上写你会的，用过的）**
*   **拓展 – Android6.0/7.0/8.0特性，kotlin语言，I/O大会**

急急忙忙投简历，赶面试，还不如沉淀一两天时间，再过一遍以上内容。想稳妥拿到一个offer，最好能理解实现原理，并且知道使用场景了。不要去背！要去理解！面试官听了一天这些内容是很厌倦的，最好能说出一些自己的见解。

* * *

## 面试题（固定答案不解答，自己可以找到）

顺序是根据记忆排的，没有优先级之分，都是重点。

**1.Activity的启动过程（不要回答生命周期）** 
[http://blog.csdn.net/luoshengyang/article/details/6689748](http://blog.csdn.net/luoshengyang/article/details/6689748)

**2.Activity的启动模式以及使用场景** 
（1）manifest设置，（2）startActivity flag 
[http://blog.csdn.net/CodeEmperor/article/details/50481726](http://blog.csdn.net/CodeEmperor/article/details/50481726) 
此处延伸：栈(First In Last Out)与队列(First In First Out)的区别

**3.Service的两种启动方式** 
（1）startService()，（2）bindService() 
[http://www.jianshu.com/p/2fb6eb14fdec](http://www.jianshu.com/p/2fb6eb14fdec)

**4.Broadcast注册方式与区别** 
（1）静态注册(minifest)，（2）动态注册 
[http://www.jianshu.com/p/ea5e233d9f43](http://www.jianshu.com/p/ea5e233d9f43) 
此处延伸：什么情况下用动态注册

**5.HttpClient与HttpUrlConnection的区别** 
[http://blog.csdn.net/guolin_blog/article/details/12452307](http://blog.csdn.net/guolin_blog/article/details/12452307) 
此处延伸：Volley里用的哪种请求方式（2.3前HttpClient，2.3后HttpUrlConnection）

**6.http与https的区别** 
[http://blog.csdn.net/whatday/article/details/38147103](http://blog.csdn.net/whatday/article/details/38147103) 
此处延伸：https的实现原理

**7.http与https的区别** 
[http://blog.csdn.net/whatday/article/details/38147103](http://blog.csdn.net/whatday/article/details/38147103) 
此处延伸：https的实现原理

**8.手写算法（选择冒泡必须要会）** 
[http://www.jianshu.com/p/ae97c3ceea8d](http://www.jianshu.com/p/ae97c3ceea8d)

**9.进程保活（不死进程）** 
[http://www.jianshu.com/p/63aafe3c12af](http://www.jianshu.com/p/63aafe3c12af) 
此处延伸：进程的优先级是什么（下面这篇文章，都有说） 
[https://segmentfault.com/a/1190000006251859](https://segmentfault.com/a/1190000006251859)

**10.进程间通信的方式** 
（1）AIDL，（2）广播，（3）Messenger 
AIDL : [http://www.jianshu.com/p/ae97c3ceea8d](http://www.jianshu.com/p/ae97c3ceea8d) 
Messenger : [http://blog.csdn.net/lmj623565791/article/details/47017485](http://blog.csdn.net/lmj623565791/article/details/47017485) 
此处延伸：简述Binder ， [http://blog.csdn.net/luoshengyang/article/details/6618363/](http://blog.csdn.net/luoshengyang/article/details/6618363/)

**11.加载大图** 
PS：有家小公司（规模写假的，给骗过去了），直接把项目给我看，让我说实现原理。。 
最让我无语的一次面试，就一个点问的我底裤都快穿了，就差帮他们写代码了。。 
[http://blog.csdn.net/lmj623565791/article/details/49300989](http://blog.csdn.net/lmj623565791/article/details/49300989)

**12.三级缓存（各大图片框架都可以扯到这上面来）** 
（1）内存缓存，（2）本地缓存，（3）网络 
内存：[http://blog.csdn.net/guolin_blog/article/details/9526203](http://blog.csdn.net/guolin_blog/article/details/9526203) 
本地：[http://blog.csdn.net/guolin_blog/article/details/28863651](http://blog.csdn.net/guolin_blog/article/details/28863651)

**13.MVP框架（必问）** 
[http://blog.csdn.net/lmj623565791/article/details/46596109](http://blog.csdn.net/lmj623565791/article/details/46596109) 
此处延伸：手写mvp例子，与mvc之间的区别，mvp的优势

**14.讲解一下Context** 
[http://blog.csdn.net/lmj623565791/article/details/40481055](http://blog.csdn.net/lmj623565791/article/details/40481055)

**15.JNI** 
[http://www.jianshu.com/p/aba734d5b5cd](http://www.jianshu.com/p/aba734d5b5cd) 
此处延伸：项目中使用JNI的地方，如：核心逻辑，密钥，加密逻辑

**16.java虚拟机和Dalvik虚拟机的区别** 
[http://www.jianshu.com/p/923aebd31b65](http://www.jianshu.com/p/923aebd31b65)

**17.线程sleep和wait有什么区别** 
[http://blog.csdn.net/liuzhenwen/article/details/4202967](http://blog.csdn.net/liuzhenwen/article/details/4202967)

**18.View，ViewGroup事件分发** 
[http://blog.csdn.net/guolin_blog/article/details/9097463](http://blog.csdn.net/guolin_blog/article/details/9097463) 
[http://blog.csdn.net/guolin_blog/article/details/9153747](http://blog.csdn.net/guolin_blog/article/details/9153747)

**19.保存Activity状态** 
onSaveInstanceState() 
[http://blog.csdn.net/yuzhiboyi/article/details/7677026](http://blog.csdn.net/yuzhiboyi/article/details/7677026)

**20.WebView与js交互（调用哪些API）** 
[http://blog.csdn.net/cappuccinolau/article/details/8262821/](http://blog.csdn.net/cappuccinolau/article/details/8262821/)

**21.内存泄露检测，内存性能优化** 
[http://blog.csdn.net/guolin_blog/article/details/42238627](http://blog.csdn.net/guolin_blog/article/details/42238627) 
这篇文章有四篇，很详细。 
此处延伸： 
（1）内存溢出（OOM）和内存泄露（对象无法被回收）的区别。 
（2）引起内存泄露的原因

**22.布局优化** 
[http://blog.csdn.net/guolin_blog/article/details/43376527](http://blog.csdn.net/guolin_blog/article/details/43376527)

**23.自定义view和动画** 
以下两个讲解都讲得很透彻，这部分面试官多数不会问很深，要么就给你一个效果让你讲原理。 
（1）[http://www.gcssloop.com/customview/CustomViewIndex](http://www.gcssloop.com/customview/CustomViewIndex) 
（2）[http://blog.csdn.net/yanbober/article/details/50577855](http://blog.csdn.net/yanbober/article/details/50577855)

**24.设计模式（单例，工厂，观察者。作用，使用场景）** 
一般说自己会的就ok，不要只记得名字就一轮嘴说出来，不然有你好受。 
[http://blog.csdn.net/jason0539/article/details/23297037/](http://blog.csdn.net/jason0539/article/details/23297037/) 
此处延伸：Double Check的写法被要求写出来。

**25.String，Stringbuffer，Stringbuilder 区别** 
[http://blog.csdn.net/kingzone_2008/article/details/9220691](http://blog.csdn.net/kingzone_2008/article/details/9220691)

**26.开源框架，为什么使用，与别的有什么区别** 
这个问题基本必问。在自己简历上写什么框架，他就会问什么。 
如：Volley，面试官会问我Volley的实现原理，与okhttp和retrofit的区别。 
开源框架很多，我就选几个多数公司都会用的出来（框架都是针对业务和性能，所以不一定出名的框架就有人用） 
网络请求：Volley，okhttp，retrofit 
异步：RxJava，AsyncTask 
图片处理：Picasso，Glide 
消息传递：EventBus 
以上框架请自行查找，太多了就不贴出来了。

**27.RecyclerView** 
这个挺搞笑的。有另外一个同事也在找工作，面试官嫌他没用过RecyclerView直接pass掉。 
[http://blog.csdn.net/lmj623565791/article/details/45059587](http://blog.csdn.net/lmj623565791/article/details/45059587)

OK，点到即止。

## 结语

面试官面什么，完全是看他们个人的（性格，心情，天气，你的面相）。以上只是一些我觉得重要的点，当然还有很多深层的东西不是一时半日可以补上来的，还是要看自己平时的经验积累。面试不单单是技术面，还有高层面，人事面，这些都要看个人发挥了。

PS：如果面试官说，还有什么想问的，千万不要给自己挖坑，说今天自己表现怎样，能不能被录取。要往公司的团队，氛围去问，尽量表现的对公司有兴趣。 
如：我想知道公司是否定期有开技术会议，老员工是否会分享自己的一些经验等这些问题。

生活不易，如果有面试官（你将来有一天也会面试别人）看到这篇文章，请放下架子或者偏见，尊重每一位面试者。

最后，我列出以下面试需要注意的几个点。

*   **面带微笑，有礼貌，谦逊**
*   **穿的体面一点，穿拖鞋的gg了8成**
*   **一定要带简历和笔**
*   **来了说谢谢，面完说谢谢**
*   **要学会看面试官的表情，如果答的不好不要继续往下说**
*   **不要吹的自己以前做过的项目有多牛b，也不要自吹**
*   **答题要冷静，不要一轮嘴说一堆，面试官很烦的**

最后祝大家面试顺利，早日找到自己心仪的公司。 
日后会相继写一些项目中遇到的难点和深层一点的技术博文，请大家多多支持！
