### 序言
这篇文章主要记录在学习《Android开发艺术探索》第11章的读书笔记，以备日后查用,码字不易转载请注明出处：http://www.jianshu.com/p/64db22fa9bc4

# 1、Android中扮演线程的有Thread 、AsyncTask、IntentService、HandlerThread；AsyncTask底层使用了线程池，、IntentService和HandlerThread底层直接使用线程；
# 2、AsyncTask、IntentService、HandlerThread的比较：
1、AsyncTask：封装了线程池和Handler，主要方便开发者在子线程中更新UI，并不适合特别耗时的任务，
2、IntentService：是一个服务，系统对其进行了封装使其更方便的执行后台任务，IntentService内部采用HandlerThread来执行任务，当任务执行完毕后，IntentService自动退出，适合执行高优先级的后台任务；
3、HandlerThread：是一种具有消息循环的线程，在他的内部可以使用Handler；

# 3、AsyncTask的使用注意事项：
1、提供了onCancelled()方法，在主线程中执行，当异步任务被取消时，onCancelled()方法会被调用，这个时候onPostExecute方法不会被调用；
2、AsyncTask的类必须在主线程中加载，即第一次访问AsyncTask必须发生在主线程中，但这个过程在Android4.1及以上版本系统自动完成；
3、AsyncTask的对象必须在主线程中创建；
4、execute方法必须在UI线程中调用；
5、不能在程序中直接调用onPreExecute()、onPostExecute()、doInBackground和onProgressUpdate方法；
6、一个AsyncTask对象只能执行一次，即只能调用一次execute方法；
7、AsyncTask采用一个线程来串行的执行任务，但可以通过AsyncTask的executeOnExecutor方法来并行的执行任务；

# 4、HandlerThread：由于HandlerThread的run方法是一个无限循环，因此当明确不需要再使用HandlerThread时通过调用他的quite()或quitSafely()方法来终止线程的执行；

# 5、线程池的优点：
1、重用线程池的线程，避免因为线程的创建和销毁所带来的开销；
2、能有效控制线程中的最大并发数，避免大量的线程之间因为互相抢占系统资源而导致的阻塞现象；
3、能对线程进行简单的管理，并提供定时执行以及指定间隔循环执行等功能；

# 6、Java中的Executor是一个接口，通过调用Executors的静态方法创造线程池，所有线程池的实现都是配置ThreadPoolExecutor类来实现的，ThreadPoolExecutor的构造方法：
```
    /**
     * 线程池的最终的构造方法
     *
     * @param corePoolSize    线程池的核心线程数，默认核心线程会在线程池中一直存活，
     *                        即使他们处于闲置状态，但allowsCoreThreadTimeOut方法返回为true ，
     *                        则核心线程在等待新任务到来时会有超时策略，该超时时间由keepAliveTime决定
     * @param maximumPoolSize 线程池所容纳的最大线程数，当活动线程达到这个数值后，新任务将被阻塞
     * @param keepAliveTime   非核心线程闲置时的超时时间，若allowsCoreThreadTimeOut方法返回为true也会作用于核心线程
     * @param unit            用于指定keepAliveTime参数的时间单位 ，是TimeUnit类的枚举
     * @param workQueue       线程池中的任务队列，通过线程池的execute方法提交的Runnable对象会存储在这个参数中
     * @param threadFactory   线程工厂，为线程池提供创建新线程的功能，ThreadFactory是一个接口，只有一个方法：Thread newThread(Runnable r);
     * @param handler         当线程数量达到线程池规定的最大值，这回拒绝执行此任务，会调用该handler的rejectedExecution方法来通知调用者
     *                        ，the handler to use when execution is blocked  because the thread bounds and queue capacities are reached
     */
    public ThreadPoolExecutor(int corePoolSize,
                                   int maximumPoolSize,
                                   long keepAliveTime,
                                   TimeUnit unit,
                                   BlockingQueue<Runnable> workQueue,
                                   ThreadFactory threadFactory,
                                   RejectedExecutionHandler handler)
```
# 7、Runtime.getRuntime().availableProcessors()：获取CPU的核心数；
# 8、线程池的分类和分别适用的场景：
### 1、FixedThreadPool：
创建：Executors.newFixedThreadPool();
适用场景：能更加快速的响应外界的请求；
特点：线程数量固定并且只有核心线程，当线程处于空闲状态也不会被回收，
### 2、CachedThreadPool：
创建：Executors.newCachedThreadPool();
适用场景：适合执行大量的耗时较少的任务，
特点：只有非核心线程并且可以无限大，超时60秒回收线程，当所有线程超时时会全部回收此时几乎不占用任何系统资源，
### 3、ScheduledThreadPool：
创建：Executors.newScheduledThreadPool();
适用场景：主要用于执行定时任务和具有固定周期的重复任务，
特点：核心线程数量固定，非核心线程无限制，非核心线程限制时会被立即回收，
### 4、SingleThreadExecutor：
创建：Executors.newSingleThreadExecutor();
适用场景：统一所有的外界任务到一个线程中，使得这些任务之间不需要处理线程同步的问题
特点：只有一个核心线程，确保所有的任务在同一个线程中按顺序执行，

