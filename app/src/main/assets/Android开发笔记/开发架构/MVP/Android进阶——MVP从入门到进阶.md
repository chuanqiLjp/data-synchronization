---
title: Android进阶——MVP从入门到进阶
layout: post
date: 2017-03-01 15:10:58
comments: true
categories:
  - Android
  - 开发架构
tags: [MVP]
keywords: MVP
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：商业转载请联系我获得授权，非商业转载请在醒目位置注明出处。



1.定义
====

MVP的全称为Model-View-Presenter，即模型-视图-协调器(主持者)
```
Model：处理数据和业务逻辑等，如：数据库的操作，数据的请求，数据运算，JavaBean；
View：显示界面，展示结果等，一切与界面相关的，如：XML文件，Activity，Fragment，Dialog；
Presenter：协调Model和View模块工作，处理交互；
```
![这里写图片描述](http://upload-images.jianshu.io/upload_images/4143664-5bae7da3ba3e6e44?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.使用
====

下面通过一个列子做一个解说，这个列子很简单，就是在ListView上展示文字和图片，主界面的布局很简单，就是一个ListView，然后定义一个Girl 的Bean对象，再自定义一个普通的适配器GirlAdapter，在自定义一个ListView中的Item的一个布局文件item_v1.xml,以上由于代码简单，就不再这里给出代码了，文末有这个Demo的下载链接，各位可以自行查看。

2.1 Model层（加载数据）
----------------

既然是MVP，我们先来看一下Model层，Model层主要用来加载数据及运算，这里常是业务的核心，在实际开发中更改最多的就是Model和View，所以我们才需要把Model与VIew分离开来，使修改其中一个不至于影响另一个，根据业务需求我们可以先定义一个Model接口
```
public interface IGirlModel {
	void loadGirl(onGirlListener listener);//加载数据
	interface onGirlListener{//数据加载完成后的监听回调
		void onComplete(List<Girl> list);
	}
}
```
在定义一个类去实现它
```
public class GirlModelImpl implements IGirlModel {
	@Override
	public void loadGirl(onGirlListener listener) {
		//加载数据
		List<Girl> list=new ArrayList<Girl>();
		list.add(new Girl(R.drawable.image1, "你是我的小苹果image1"));
		list.add(new Girl(R.drawable.image2, "你是我的小苹果image2"));
		list.add(new Girl(R.drawable.image3, "你是我的小苹果image3"));
		list.add(new Girl(R.drawable.image4, "你是我的小苹果image4"));
		list.add(new Girl(R.drawable.image5, "你是我的小苹果image5"));
		list.add(new Girl(R.drawable.image6, "你是我的小苹果image6"));
		list.add(new Girl(R.drawable.image7, "你是我的小苹果image7"));
		list.add(new Girl(R.drawable.image8, "你是我的小苹果image8"));
		//返回数据
		listener.onComplete(list);
	}
}
```
这样Model层就基本完成了，代码很少，逻辑也比较清晰（加载数据再返回）

2.2 View层（展示数据）
---------------

在MVP中我们常把Activity、Fragment、Dialog等作为View层，这里也是先定义一个接口
```
/**
 * 视图层接口，定义与视图操作相关的接口
 */
public interface IGirlView {
	//加载的提示
	void showDialog();
	//显示加载后的数据
	void showGirls(List<Girl>list);
}
```
再让我们的MainActivity去实现它
```
public class MainActivity extends Activity implements IGirlView {
	ListView listView;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		initView();
	}
	private void initView() {
		listView = (ListView) findViewById(R.id.listview);
	}
	@Override
	public void showDialog() {
		Toast.makeText(this, "正在加载数据中。。。", Toast.LENGTH_LONG).show();
	}
	@Override
	public void showGirls(List<Girl> list) {
		listView.setAdapter(new GirlAdapter(this, list));
	}
}
```

2.3Persenter层（数据与视图的交互）
-----------------------

Persenter层主要的工作就是控制当数据加载后呈现在View中
```
public class GirlPresenter {
	// View 的引用
	IGirlView iGirlView;
	// Model的引用
	IGirlModel iGirlModel=new GirlModelImpl();
	public GirlPresenter(IGirlView iGirlView) {
		this.iGirlView = iGirlView;
	}
	public void feach() {
		if (iGirlModel != null && iGirlView != null) {
			// 显示进度条
			iGirlView.showDialog();
			// 加载数据
			iGirlModel.loadGirl(new onGirlListener() {
				@Override
				public void onComplete(List<Girl> list) {
					// 返回数据，显示到View层
					iGirlView.showGirls(list);
				}
			});
		}
	}
}
```
好了，一个简单的MVP架构的Demo就写好了；老板明天叫我用GridView展示我就新增加一个IGirlView的实现类而Model层不用修改，后天叫我加载网络数据我就增加一个IGirlModel的实现类而View层不用更改，[Demo点击下载](http://download.csdn.net/detail/wo_ha/9767302)，这样做有以下好处：
```
1.Model层与View层完全分离（在也不要担心改需求了）
2.分工更加明确，逻辑更加清晰，易于维护代码
3.便于单元测试
```

3.进阶
====

有经验的同学可能会发现，上面有个巨大的问题：内存泄漏（有可能加载数据的耗时操作中界面关闭了，由于长期持有Activity的引用造成当onDestory时释放不了内存）；
下面就一步一步的解决这个问题

3.1使用弱引用（[参考](https://mp.weixin.qq.com/s/nIIzHv_sqZom_y2nblCjJQ)）解决内存泄漏的问题
------------------------------------------------------------------------

```
public class GirlPresenter<T extends IGirlView> {
	//View 的引用
	protected WeakReference<T>mReference;//使用弱引用,避免内存泄漏
	//Model的引用
	IGirlModel iGirlModel=new GirlModelImpl();
	public GirlPresenter(T view) {
		this.mReference = new WeakReference<T>(view);
	}
	public void feach(){
	        //显示进度条
			if (mReference.get()!=null) {
				mReference.get().showDialog();
			}
		     //加载数据
			iGirlModel.loadGirl(new onGirlListener() {
				@Override
				public void onComplete(List<Girl> list) {
					//返回数据，显示到View层
					if (mReference.get()!=null) {
						mReference.get().showGirls(list);
					}
				}
			});
	}
｝
```

3.2性能优化
-------

以上代码炸已看，好像能解决内存泄漏的问题，但是弱引用是当GC触发时才被回收，当Activity的onDestory方法调用时我们的GC可能被触发也有可能不会，因此Activity的onDestory调用时的引用可能被回收也有可能没有被回收，试想一种情况：加载网络数据需要20s，界面在第5s的时候就关闭了，20s后GC没有被触发即mReference.get()!=null，下面我们拿着Activity的引用去操作，去界面展示，我们不是去鞭尸吗？这又如何解决呢？其实在Android系统中有一个类似的模型，那就是Activity与Fragment（Activity销毁了，而Fragment还在执行耗时操作），那Google又是怎么解决的呢？
还记得public void onAttach(Activity activity)和public void onDetach()这两个方法吗，在onAttach时将Activity的引用赋值，在onAttach主动将Activity的引用置为null，代码如下

```
public class GirlPresenter<T extends IGirlView> {
	// View 的引用
	// IGirlView iGirlView;
	protected WeakReference<T> mReference;// 使用弱引用,避免内存泄漏
	// Model的引用
	IGirlModel iGirlModel = new GirlModelImpl();
	public GirlPresenter() {//不再构造方法里将activity的引用赋值
		super();
		//this.mReference = new WeakReference<T>(view);
	}
	public void feach() {
		if (iGirlModel != null && mReference.get() != null) {
			// 显示进度条
			if (mReference.get() != null) {
				mReference.get().showDialog();
			}
			SystemClock.sleep(2000);// Android的休眠，已忽略了中断的异常
			// 加载数据
			iGirlModel.loadGirl(new onGirlListener() {
				@Override
				public void onComplete(List<Girl> list) {
					// 返回数据
					// 显示到View层
					if (mReference.get() != null) {
						mReference.get().showGirls(list);
					}
				}
			});
		}
	}
	/**
	 * 连接上View模型，类型于Activity与Fragment的连接onAttach()
	 */
	public void attachView(T view) {
		mReference = new WeakReference<T>(view);
	}
	/**
	 * 断开与View模型的连接，类型于Activity与Fragment的断开onDetach() 防止后面做一些无用的事情
	 */
	public void detachView() {
		if (mReference != null) {
			mReference.clear();
		}
	}
}
```
同时在MainActivity的onCreate中presenter.attachView(this);在onDestory中presenter.detachView();

3.3 代码优化
--------

上面虽然彻底解决了内存泄漏的问题，但是又引出了新问题，我们需要在每个Activity中都需要关联Activity与断开Activity，这样就显得代码很臃肿，为此我们还需要将代码再次抽层，这个很容易想到的就是让我们的Activity去继承我们自己的BaseActivity，让我们的Presenter继承BasePresenter，在BaseActivity去统一做关联，但是我们不可能每一个Activity关联的Presenter都一样吧，在此能想到 的时使用泛型，来看看代码中怎么使用吧
**3.3.1 在BaseActivity中**
```
// 泛型V ：表示Activity本身，泛型P：表示需要关联的Presenter
public abstract class BaseActivity<V,P extends BasePresenter<V>> extends Activity {
	protected P presenter;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		presenter = createPresenter();
		presenter.attachView((V)this);
	}
	/**
	 * 创建一个与之关联的Presenter
	 * @return
	 */
	protected abstract P  createPresenter();
	@Override
	protected void onDestroy() {
		super.onDestroy();
		presenter.detachView();
	}
}
```
**3.3.2 在BasePresenter中**
```
// 泛型V:表示与之关联的Activity
public class BasePresenter<V> {
	//View 的引用
	protected WeakReference<V>mReference;//使用弱引用,避免内存泄漏

	/**
	 * 连接上View模型，类型于Activity与Fragment的连接onTachActivity()
	 * @param view
	 */
	public void attachView(V view){
		mReference=new WeakReference<V>(view);
	}
	/**
	 * 断开与View模型的连接，类型于Activity与Fragment的断开ondeTachActivity()
	 * 防止后面做一些无用的事情
	 * @param view
	 */
	public void detachView(){
		if (mReference!=null) {
			mReference.clear();
		}
	}
	protected V getView(){
		return mReference.get();
	}
}
```
**3.3.3 在MainActivity中**
```
public class MainActivity extends BaseActivity<IGirlView,GirlPresenterV2> implements IGirlView {
	ListView listView;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		initView();
		presenter.feach();
	}
｝
```
**3.3.4 在GirlPresenter中**
```
public class GirlPresenter extends BasePresenter<IGirlView> {
	//Model的引用
	IGirlModel iGirlModel=new GirlModelImpl();
	public void feach(){
		if (iGirlModel!=null&&getView()!=null) {
			//显示进度条
			if (getView()!=null) {
				getView().showDialog();
			}
			SystemClock.sleep(2000);// Android的休眠，已忽略了中断的异常
			//加载数据
			iGirlModel.loadGirl(new onGirlListener() {
				@Override
				public void onComplete(List<Girl> list) {
					//返回数据
					//显示到View层
					if (getView()!=null) {
						getView().showGirls(list);
					}
				}
			});
		}
	}
}
```
到此我们的优化就结束了，希望你有所收获！[优化后的Demo点击下载](http://download.csdn.net/detail/wo_ha/9767308)
我的CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/59108525