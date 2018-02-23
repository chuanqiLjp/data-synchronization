# 第一种解释：

功能差不多,都用来进行线程控制,他们最大本质的区别是:sleep()不释放同步锁,wait()释放同步锁.

  还有用法的上的不同是:sleep(milliseconds)可以用时间指定来使他自动醒过来,如果时间不到你只能调用interreput()来强行打断;wait()可以用notify()直接唤起.

# 第二种解释：

sleep是Thread类的[静态方法](http://blog.csdn.net/liuzhenwen/article/details/4202967)。sleep的作用是让线程休眠制定的时间，在时间到达时恢复，也就是说sleep将在接到时间到达事件事恢复线程执行，例如：
try{
System.out.println("I'm going to bed");
Thread.sleep(1000);
System.out.println("I wake up");
}
catch(IntrruptedException e) {
}
wait是Object的方法，也就是说可以对任意一个对象调用wait方法，调用wait方法将会将调用者的线程挂起，直到其他线程调用同一个对象的notify方法才会重新激活调用者，例如：
//Thread 1

try{
obj.wait();//suspend thread until obj.notify() is called
}
catch(InterrputedException e) {
}

# 第三种解释：

这两者的施加者是有本质区别的.
sleep()是让某个线程暂停运行一段时间,其控制范围是由当前线程决定,也就是说,在线程里面决定.好比如说,我要做的事情是 "点火->烧水->煮面",而当我点完火之后我不立即烧水,我要休息一段时间再烧.对于运行的主动权是由我的流程来控制.

而wait(),首先,这是由某个确定的对象来调用的,将这个对象理解成一个传话的人,当这个人在某个线程里面说"暂停!",也是 thisOBJ.wait(),这里的暂停是阻塞,还是"点火->烧水->煮饭",thisOBJ就好比一个监督我的人站在我旁边,本来该线 程应该执行1后执行2,再执行3,而在2处被那个对象喊暂停,那么我就会一直等在这里而不执行3,但正个流程并没有结束,我一直想去煮饭,但还没被允许, 直到那个对象在某个地方说"通知暂停的线程启动!",也就是thisOBJ.notify()的时候,那么我就可以煮饭了,这个被暂停的线程就会从暂停处 继续执行.

其实两者都可以让线程暂停一段时间,但是本质的区别是一个线程的运行状态控制,一个是线程之间的通讯的问题

 在java.lang.Thread类中，提供了sleep()，
而java.lang.Object类中提供了wait()， notify()和notifyAll()方法来操作线程
sleep()可以将一个线程睡眠，参数可以指定一个时间。
而wait()可以将一个线程挂起，直到超时或者该线程被唤醒。
wait有两种形式wait()和wait(milliseconds).
# sleep和wait的区别有：
  1，这两个方法来自不同的类分别是Thread和Object

  2，**最主要是sleep方法没有释放锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法。**

  3，wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在
    任何地方使用
```
   synchronized(x){
      x.notify()
     //或者wait()
   }
```

   4,sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常
