# Android_Interview
Android面试题目合集
以下的所有内容只是本人复习Android基础知识，在网上找的题目，如果有答案不对，或者是回答不好的地方，欢迎讨论`itgoyo@gmail.com`

## 问答题
### 1.谈谈android数据存储方式
```
SharedPreferences数据存储：通过键值对的形式保存简单的、私有的数据

内部文件存储：将私有数据保存在设备内部的存储介质中

外部文件存储：将公用数据保存在共享的外部存储介质中

SQLite数据库存储：将结构化的数据保存在私有的一个数据库中

网络存储：将私有的数据保存在网络上开发者自己的服务器中

Content Provider数据存储：获取、保存其他应用程序暴露的数据
```
### 2.android:gravity与android:layout_gravity的区别
```
android:gravity是自己内部控件相对于自己的位置

android:layout_gravity是自己相对于父容器的位置
```
### 3.在同一线程中android.Handler和android.MessaegQueue的数量对应关系
```
  1. Handler 必须在 Looper.prepare() 之后才能创建使用
  2. Looper 与当前线程关联，并且管理着一个 MessageQueue
  3. Message 是实现 Parcelable接口的类
  4. 以一个线程为基准，他们的数量级关系是：Handler(N) : Looper(1) : MessageQueue(1) : Thread(1)。
```
### 4.android 中解析XML主要有三种方式().().()
```
1. SAX
2. DOM
3. PULL
```
### 5.请描述一下 Android 开发时，在线程中进行 UI 更新的方式
```
（1）Handler+Thread：适用于复杂的，多线程更新UI的情况；
（2）AsyncTask：适用于单一的线程更新UI的情况；
（3）runOnUiThread；
（4）View.post(runnable)。
```
### 6.Android进程有哪些
```
前台进程
用户当前操作所必需的进程。如果一个进程满足以下任一条件，即视为前台进程：
托管用户正在交互的 Activity（已调用 Activity 的 onResume() 方法）
托管某个 Service，后者绑定到用户正在交互的 Activity
托管正在“前台”运行的 Service（服务已调用 startForeground()）
托管正执行一个生命周期回调的 Service（onCreate()、onStart() 或 onDestroy()）
托管正执行其 onReceive() 方法的 BroadcastReceiver
通常，在任意给定时间前台进程都为数不多。只有在内存不足以支持它们同时继续运行这一万不得已的情况下，系统才会终止它们。 此时，设备往往已达到内存分页状态，因此需要终止一些前台进程来确保用户界面正常响应。

可见进程
没有任何前台组件、但仍会影响用户在屏幕上所见内容的进程。 如果一个进程满足以下任一条件，即视为可见进程：

托管不在前台、但仍对用户可见的 Activity（已调用其 onPause() 方法）。例如，如果前台 Activity 启动了一个对话框，允许在其后显示上一 Activity，则有可能会发生这种情况。
托管绑定到可见（或前台）Activity 的 Service。
可见进程被视为是极其重要的进程，除非为了维持所有前台进程同时运行而必须终止，否则系统不会终止这些进程。

服务进程
正在运行已使用 startService() 方法启动的服务且不属于上述两个更高类别进程的进程。尽管服务进程与用户所见内容没有直接关联，但是它们通常在执行一些用户关心的操作（例如，在后台播放音乐或从网络下载数据）。因此，除非内存不足以维持所有前台进程和可见进程同时运行，否则系统会让服务进程保持运行状态。

后台进程
包含目前对用户不可见的 Activity 的进程（已调用 Activity 的 onStop() 方法）。这些进程对用户体验没有直接影响，系统可能随时终止它们，以回收内存供前台进程、可见进程或服务进程使用。 通常会有很多后台进程在运行，因此它们会保存在 LRU （最近最少使用）列表中，以确保包含用户最近查看的 Activity 的进程最后一个被终止。如果某个 Activity 正确实现了生命周期方法，并保存了其当前状态，则终止其进程不会对用户体验产生明显影响，因为当用户导航回该 Activity 时，Activity 会恢复其所有可见状态。 有关保存和恢复状态的信息，请参阅 Activity文档。

空进程
不含任何活动应用组件的进程。保留这种进程的的唯一目的是用作缓存，以缩短下次在其中运行组件所需的启动时间。 为使总体系统资源在进程缓存和底层内核缓存之间保持平衡，系统往往会终止这些进程。
```

### 7.activity的startActivity和context的startActivity区别
```
(1)从Activity中启动新的Activity时可以直接mContext.startActivity(intent)就好，

(2)如果从其他Context中启动Activity则必须给intent设置Flag:

intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK) ;
mContext.startActivity(intent);
```

### 8.如何保证Service杀不死
```
提供进程优先级，降低进程被杀死的概率
方法一：监控手机锁屏解锁事件，在屏幕锁屏时启动1个像素的 Activity，在用户解锁时将 Activity 销毁掉。

方法二：启动前台service。

方法三：提升service优先级： 在AndroidManifest.xml文件中对于intent-filter可以通过android:priority = "1000"这个属性设置最高优先级，1000是最高值，如果数字越小则优先级越低，同时适用于广播。

在进程被杀死后，进行拉活

方法一：注册高频率广播接收器，唤起进程。如网络变化，解锁屏幕，开机等

方法二：双进程相互唤起。

方法三：依靠系统唤起。

方法四：onDestroy方法里重启service：service +broadcast 方式，就是当service走ondestory的时候，发送一个自定义的广播，当收到广播的时候，重新启动service；

依靠第三方 根据终端不同，在小米手机（包括 MIUI）接入小米推送、华为手机接入华为推送；其他手机可以考虑接入腾讯信鸽或极光推送与小米推送做 A/B Test。
```

### 9.StringBuffer跟StringBuider的区别
```
StringBuider非线程安全，执行速度快，单线程用这个类
```

### 10.sleep(), wait()的区别
```
sleep不释放同步锁，自动唤醒，需要try catch,  wait释放同步锁，需要notify来唤醒

sleep是线程的方法    wait是Object的方法
```

### 11.LinkedList跟ArrayList的区别
```
一个链表结构，一个数组结构，LinkedList查找慢，插入快，ArrayList查找快，插入慢。
```

### 12.手写一个单例
```
public class SigleInstance {

private static SigleInstance instance;

public static SigleInstance getInstance() {

        if (instance == null) {

            syncronized(SigleInstance.class) {

                    if (instance == null) {

                            instance = new SingleInstance();

                    }

            }

       }

        return instance;

}

}
```





------------

**1.四大组件**
- Activity
[https://www.jianshu.com/p/7c193337702d](https://www.jianshu.com/p/7c193337702d)
- Service
[https://www.jianshu.com/p/d963c55c3ab9](https://www.jianshu.com/p/d963c55c3ab9)
[https://www.jianshu.com/p/e04c4239b07e](https://www.jianshu.com/p/e04c4239b07e)
- Content Provider
[https://www.jianshu.com/p/ea8bc4aaf057](https://www.jianshu.com/p/ea8bc4aaf057)
- Broadcast Receiver
[https://www.jianshu.com/p/ca3d87a4cdf3](https://www.jianshu.com/p/ca3d87a4cdf3)
请阅读上面的基础知识

（1.1）四大组件是什么

（1.2）四大组件的生命周期

（1.3）Activity之间的通信方式
[https://www.jianshu.com/p/f836432396f0](https://www.jianshu.com/p/f836432396f0)

（1.4）横竖屏切换的时候，Activity 各种情况下的生命周期
[https://blog.csdn.net/hzw19920329/article/details/51345971](https://blog.csdn.net/hzw19920329/article/details/51345971)

（1.5）Activity与Fragment之间生命周期比较
[https://www.jianshu.com/p/b1ff03a7bb1f](https://www.jianshu.com/p/b1ff03a7bb1f)

（1.6）Activity上有Dialog的时候按Home键时的生命周期
[https://blog.csdn.net/hanhan1016/article/details/47977489](https://blog.csdn.net/hanhan1016/article/details/47977489)

（1.7）两个Activity 之间跳转时必然会执行的是哪几个方法？
[https://blog.csdn.net/m_xiaoer/article/details/72881082](https://blog.csdn.net/m_xiaoer/article/details/72881082)

（1.8）Activity的四种启动模式对比以及使用场景
[https://blog.csdn.net/CodeEmperor/article/details/50481726](https://blog.csdn.net/CodeEmperor/article/details/50481726)

（1.9）Activity状态保存与恢复
[https://blog.csdn.net/sinat_33921105/article/details/78631823](https://blog.csdn.net/sinat_33921105/article/details/78631823)

（1.10）Activity 怎么和Service 绑定
[https://www.jianshu.com/p/5d73389f3ab2](https://www.jianshu.com/p/5d73389f3ab2)

（1.11）Service和Activity怎么进行数据交互？
[https://www.jianshu.com/p/cd69f208f395](https://www.jianshu.com/p/cd69f208f395)

（1.12）Service的开启方式
[https://www.jianshu.com/p/2fb6eb14fdec](https://www.jianshu.com/p/2fb6eb14fdec)

（1.13）请描述一下Service 的生命周期
[https://www.jianshu.com/p/8d0cde35eb10](https://www.jianshu.com/p/8d0cde35eb10)

（1.14）谈谈你对ContentProvider的理解
[https://www.jianshu.com/p/f5ec75a9cfea](https://www.jianshu.com/p/f5ec75a9cfea)

（1.15）ContentProvider、ContentResolver、ContentObserver 之间的关系
[https://blog.csdn.net/heqiangflytosky/article/details/31777363](https://blog.csdn.net/heqiangflytosky/article/details/31777363)

（1.16）请描述一下广播BroadcastReceiver的理解（实现原理）
[https://www.jianshu.com/p/ca3d87a4cdf3](https://www.jianshu.com/p/ca3d87a4cdf3)

（1.17）广播的分类
[https://www.jianshu.com/p/ca3d87a4cdf3](https://www.jianshu.com/p/ca3d87a4cdf3)

（1.18）广播使用的方式和场景
[https://www.jianshu.com/p/ca3d87a4cdf3](https://www.jianshu.com/p/ca3d87a4cdf3)

（1.19）本地广播和全局广播有什么差别？
[https://www.jianshu.com/p/bfbb6ebc1c04](https://www.jianshu.com/p/bfbb6ebc1c04)

（1.20）Application 和 Activity 的 Context 对象的区别
[https://www.cnblogs.com/liyiran/p/5283551.html](https://www.cnblogs.com/liyiran/p/5283551.html)

**2.Fragment**

有个大神自己封装了Fragment，基本覆盖了大部分你能遇到的坑，看看他的文章：
[https://www.jianshu.com/p/d9143a92ad94](https://www.jianshu.com/p/d9143a92ad94)

（2.1）什么是Fragment
[https://www.jianshu.com/p/2bf21cefb763](https://www.jianshu.com/p/2bf21cefb763)

（2.2）为什么要用Fragment
[https://www.cnblogs.com/shaweng/p/3918985.html](https://www.cnblogs.com/shaweng/p/3918985.html)

（2.3）Fragment与Activity的通信方式
[https://www.jianshu.com/p/825eb1f98c19](https://www.jianshu.com/p/825eb1f98c19)

（2.4）Fragment各种情况下的生命周期
[https://www.jianshu.com/p/b1ff03a7bb1f](https://www.jianshu.com/p/b1ff03a7bb1f)

（2.5）Fragment之间传递数据的方式？
[https://www.jianshu.com/p/f87baad32662](https://www.jianshu.com/p/f87baad32662)

（2.6）Fragment的add与replace的区别
[https://blog.csdn.net/gsw333/article/details/51858524](https://blog.csdn.net/gsw333/article/details/51858524)

（2.7）用Fragment有遇过什么坑吗，怎么解决
[https://www.jianshu.com/p/d9143a92ad94](https://www.jianshu.com/p/d9143a92ad94)

（2.8）getFragmentManager，getSupportFragmentManager ，getChildFragmentManager三者之间的区别
[https://blog.csdn.net/allan_bst/article/details/64920076](https://blog.csdn.net/allan_bst/article/details/64920076)

（2.9）FragmentPagerAdapter与FragmentStatePagerAdapter的区别与使用场景
[https://blog.csdn.net/lamp_zy/article/details/52446842](https://blog.csdn.net/lamp_zy/article/details/52446842)

**3.自定义组件、动画**

了解自定义view，这里：
[https://www.jianshu.com/p/c84693096e41](https://www.jianshu.com/p/c84693096e41)

（3.1）描述一下View的绘制流程
[https://www.jianshu.com/p/060b5f68da79](https://www.jianshu.com/p/060b5f68da79)

（3.2）说说自定义view的几个构造函数
[http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2016/0806/4575.html](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2016/0806/4575.html)

（3.3）View 里面的 onSavedInstanceState和onRestoreInstanceState的作用
[https://blog.csdn.net/shouniezhe/article/details/47705001](https://blog.csdn.net/shouniezhe/article/details/47705001)

（3.4）onLayout() 和Layout()的区别
[https://blog.csdn.net/h183288132/article/details/50184423](https://blog.csdn.net/h183288132/article/details/50184423)

（3.5）描述一下getX、getRawX、getTranslationX
[https://blog.csdn.net/dmk877/article/details/51550031](https://blog.csdn.net/dmk877/article/details/51550031)

（3.6）Android中的动画有哪几类，它们的特点和区别是什么
[https://www.jianshu.com/p/420629118c10](https://www.jianshu.com/p/420629118c10)

（3.7）Interpolator和TypeEvaluator的作用
[https://www.cnblogs.com/wondertwo/p/5327586.html](https://www.cnblogs.com/wondertwo/p/5327586.html)

（3.8）请描述一下View事件传递分发机制
[https://www.jianshu.com/p/e99b5e8bd67b](https://www.jianshu.com/p/e99b5e8bd67b)

（3.9）事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
[https://blog.csdn.net/huiguixian/article/details/22193977](https://blog.csdn.net/huiguixian/article/details/22193977)

（3.10）View和ViewGroup分别有哪些事件分发相关的回调方法
[https://www.jianshu.com/p/e99b5e8bd67b](https://www.jianshu.com/p/e99b5e8bd67b)

（3.11）View刷新机制
[https://blog.csdn.net/chenzhiqin20/article/details/8628952](https://blog.csdn.net/chenzhiqin20/article/details/8628952)

**4.存储**

（4.1）描述一下你知道的数据存储方式
[https://www.jianshu.com/p/540e44f00d3e](https://www.jianshu.com/p/540e44f00d3e)

（4.2）SharedPreferences的应用场景，核心原理是什么
[https://www.jianshu.com/p/ae2c7004179d](https://www.jianshu.com/p/ae2c7004179d)
[https://blog.csdn.net/dbs1215/article/details/78531258](https://blog.csdn.net/dbs1215/article/details/78531258)

（4.3）SharedPreferences是线程安全的吗？
去源码看看有没有同步锁就知道了，答案是线程安全的。

（4.4）描述一下图片存储在本地的方式
[https://www.jianshu.com/p/8cede074ba5b](https://www.jianshu.com/p/8cede074ba5b)
[https://blog.csdn.net/ccpat/article/details/45314175](https://blog.csdn.net/ccpat/article/details/45314175)

（4.5）sqlite升级，增加字段的语句
[https://blog.csdn.net/xu_song/article/details/49658195](https://blog.csdn.net/xu_song/article/details/49658195)

（4.6）数据库框架对比和源码分析
[https://blog.csdn.net/u012702547/article/details/52226163](https://blog.csdn.net/u012702547/article/details/52226163)
[https://www.jianshu.com/p/c4e9288d2ce6](https://www.jianshu.com/p/c4e9288d2ce6)

（4.7）数据库的优化
[https://www.jianshu.com/p/3b4452fc1bbd](https://www.jianshu.com/p/3b4452fc1bbd)

（4.8）数据库数据迁移问题
[https://www.cnblogs.com/awkflf11/p/6033074.html](https://www.cnblogs.com/awkflf11/p/6033074.html)

**5.网络**

（5.1）描述一次网络请求的流程
[https://blog.csdn.net/seu_calvin/article/details/53304406](https://blog.csdn.net/seu_calvin/article/details/53304406)

（5.2）HTTP报文结构
[https://www.jianshu.com/p/e544b7a76dac](https://www.jianshu.com/p/e544b7a76dac)

（5.3）HttpClient和HttpURLConnection的区别
[https://www.cnblogs.com/zhousysu/p/5483896.html](https://www.cnblogs.com/zhousysu/p/5483896.html)

（5.4）Volley，okhttp，retrofit之间的区别和核心原理和使用场景
[https://www.jianshu.com/p/050c6db5af5a](https://www.jianshu.com/p/050c6db5af5a)

（5.5）描述一下https
[https://showme.codes/2017-02-20/understand-https/](https://showme.codes/2017-02-20/understand-https/)

（5.6）https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解?
[https://showme.codes/2017-02-20/understand-https/](https://showme.codes/2017-02-20/understand-https/)

（5.7）说一下三次握手，四次挥手的具体细节
我经常用面试问别人这道题，哈哈，为什么不能两次握手呢？要三次？
[https://www.cnblogs.com/Andya/p/7272462.html](https://www.cnblogs.com/Andya/p/7272462.html)

（5.8）描述一下socket是什么东西
[https://www.jianshu.com/p/089fb79e308b](https://www.jianshu.com/p/089fb79e308b)

（5.9）从网络加载一个10M的图片，说下注意事项
[https://www.jianshu.com/p/7c81d3742c38](https://www.jianshu.com/p/7c81d3742c38)
[https://www.jianshu.com/p/f850a23ab99c](https://www.jianshu.com/p/f850a23ab99c)

（5.10）TCP与UDP的区别
[https://blog.csdn.net/li_ning_/article/details/52117463](https://blog.csdn.net/li_ning_/article/details/52117463)
（5.11）client如何确定自己发送的消息被server收到?
HTTP协议里，有请求就有响应，根据响应的状态吗就能知道拉。

（5.12）WebSocket与socket的区别
[https://blog.csdn.net/wwd0501/article/details/54582912](https://blog.csdn.net/wwd0501/article/details/54582912)

（5.13）网络请求缓存处理，okhttp如何处理网络缓存的
看源码，看缓存策略
[http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0326/2643.html](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0326/2643.html)

（5.14）自己去设计网络请求框架，怎么做？（随便套个开源框架的原理）
就套okhttp的，被google承认并使用的框架，准没错。
[http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0326/2643.html](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0326/2643.html)

**6.图片**

（6.1）说一下OOM的原因，如何避免
[https://blog.csdn.net/boyupeng/article/details/47726765](https://blog.csdn.net/boyupeng/article/details/47726765)

（6.2）说一下三级缓存的原理
[https://www.jianshu.com/p/97455f080065](https://www.jianshu.com/p/97455f080065)

（6.3）描述一下内存缓存的容器
LruCache其实是一个Hash表，内部使用的是LinkedHashMap存储数据
[https://blog.csdn.net/justloveyou_/article/details/71713781](https://blog.csdn.net/justloveyou_/article/details/71713781)

（6.4）图片库对比
[https://www.jianshu.com/p/fc72001dc18d](https://www.jianshu.com/p/fc72001dc18d)

（6.5）图片库的源码分析
[https://blog.csdn.net/guolin_blog/article/details/53759439](https://blog.csdn.net/guolin_blog/article/details/53759439)

（6.6）图片框架缓存实现
郭霖大神写了几篇文章介绍Glide，都有详细介绍
[https://blog.csdn.net/guolin_blog/article/details/53759439](https://blog.csdn.net/guolin_blog/article/details/53759439)

（6.7）LRUCache原理
[https://www.cnblogs.com/tianzhijiexian/p/4248677.html](https://www.cnblogs.com/tianzhijiexian/p/4248677.html)

（6.9）自己去实现图片库，怎么做？（随便套个开源框架的原理）
套Glide的就OK拉，从设计思想，然后到实现方式

（6.12）说说Glide内存缓存的具体实现？
[https://blog.csdn.net/guolin_blog/article/details/54895665](https://blog.csdn.net/guolin_blog/article/details/54895665)

**7.布局**

（7.1）说一下布局性能的排序，谁的效率最高
[https://blog.csdn.net/seu_calvin/article/details/53047682](https://blog.csdn.net/seu_calvin/article/details/53047682)

（7.2）描述一下约束布局
[https://blog.csdn.net/zhaoyanjun6/article/details/62896784](https://blog.csdn.net/zhaoyanjun6/article/details/62896784)

（7.3）关于布局优化的方案
学会用约束布局，基本优化很多了，但是老方法还是要会，面试官多数比较守旧。因为资深，年纪也可能稍微大一点，哈哈。
[https://www.cnblogs.com/hoolay/p/6248514.html](https://www.cnblogs.com/hoolay/p/6248514.html)

（7.4）怎么检测布局深度
[https://blog.csdn.net/hp910315/article/details/52684039](https://blog.csdn.net/hp910315/article/details/52684039)
（7.5）LinearLayout、RelativeLayout、FrameLayout的特性及对比，并介绍使用场景。
[https://blog.csdn.net/seu_calvin/article/details/53047682](https://blog.csdn.net/seu_calvin/article/details/53047682)

**8.性能优化**

PS：性能优化包括内存，处理效率，视觉流畅度，CPU，电量，流量等方面，针对手机的性能去做相应的方案。个人认为更应该把握好内存优化、处理效率（代码质量）、视觉流畅度（布局优化）。

（8.1）ANR产生的原因是什么？
[https://www.jianshu.com/p/7fd95bc2a55c](https://www.jianshu.com/p/7fd95bc2a55c)

（8.3）oom是什么？
[https://blog.csdn.net/hudfang/article/details/51781997](https://blog.csdn.net/hudfang/article/details/51781997)

（8.4）什么情况导致oom？
[https://blog.csdn.net/hudfang/article/details/51781997](https://blog.csdn.net/hudfang/article/details/51781997)

（8.5）有什么解决方法可以避免OOM？
[https://blog.csdn.net/hudfang/article/details/51781997](https://blog.csdn.net/hudfang/article/details/51781997)

（8.6）Oom 是否可以try catch？为什么？
有一种情况可以：在try语句中声明了很大的对象，导致OOM，并且可以确认OOM是由try语句中的对象声明导致的，但是这通常不是合适的做法。

（8.7）内存泄漏是什么？
[https://www.jianshu.com/p/bf159a9c391a](https://www.jianshu.com/p/bf159a9c391a)

（8.8）什么情况导致内存泄漏？
[https://www.jianshu.com/p/90caf813682d](https://www.jianshu.com/p/90caf813682d)

（8.9）如何防止线程的内存泄漏？
[https://www.jianshu.com/p/90caf813682d](https://www.jianshu.com/p/90caf813682d)
[https://www.cnblogs.com/ywq-come/p/5926422.html](https://www.cnblogs.com/ywq-come/p/5926422.html)

（8.10）内存泄露的解决方法
[https://blog.csdn.net/carson_ho/article/details/79407707](https://blog.csdn.net/carson_ho/article/details/79407707)

（8.11）内存泄漏和内存溢出区别？
[https://blog.csdn.net/sinat_29255093/article/details/52556760](https://blog.csdn.net/sinat_29255093/article/details/52556760)

（8.12）如何对Android 应用进行性能分析以及优化?
这个作者做了很多片性能优化的文章，建议看完
[https://www.jianshu.com/p/da2a4bfcba68](https://www.jianshu.com/p/da2a4bfcba68)

（8.13）怎么去除无用代码？
[https://blog.csdn.net/it_flower/article/details/52305558](https://blog.csdn.net/it_flower/article/details/52305558)

（8.14）性能优化如何分析systrace？
[https://www.jianshu.com/p/6f528e862d31](https://www.jianshu.com/p/6f528e862d31)

（8.15）用IDE如何分析内存泄漏？
跑一段你觉得有问题的代码段，gc，再跑，再gc，看看内存会不会一直上升
[https://blog.csdn.net/u010944680/article/details/51721532](https://blog.csdn.net/u010944680/article/details/51721532)

（8.16）Java多线程引发的性能问题，怎么解决？
[https://zhuanlan.zhihu.com/p/23389156](https://zhuanlan.zhihu.com/p/23389156)

（8.17）启动页白屏及黑屏解决？
[https://blog.csdn.net/zivensonice/article/details/51691136](https://blog.csdn.net/zivensonice/article/details/51691136)

（8.18）启动太慢怎么解决？
应用启动速度，取决于你在application里面时候做了什么事情，比如你集成了很多sdk，并且sdk的init操作都需要在主线程里实现，那自然就慢咯。在非必要的情况下可以把加载延后。或者丢子线程里。
[https://www.jianshu.com/p/4f10c9a10ac9](https://www.jianshu.com/p/4f10c9a10ac9)

（8.19）怎么保证应用启动不卡顿？
同上面一个道理，也可以做个闪屏页当缓冲时间。
[https://www.jianshu.com/p/4f10c9a10ac9](https://www.jianshu.com/p/4f10c9a10ac9)

（8.20）App启动崩溃异常捕捉
[https://www.jianshu.com/p/fb28a5322d8a](https://www.jianshu.com/p/fb28a5322d8a)

（8.21）自定义View注意事项
减少不必要的调用invalidate()方法
[https://blog.csdn.net/whb20081815/article/details/74474736](https://blog.csdn.net/whb20081815/article/details/74474736)

（8.22）现在下载速度很慢,试从网络协议的角度分析原因,并优化(提示：网络的5层都可以涉及)。
这个问题让我去请教一下再来回答

（8.23）Https请求慢的解决办法（提示：DNS，携带数据，直接访问IP）
[https://www.cnblogs.com/mylanguage/p/5635524.html](https://www.cnblogs.com/mylanguage/p/5635524.html)

（8.24）如何保持应用的稳定性
内存，布局优化，代码质量，数据结构效率，针对业务合理的设计框架

（8.25）RecyclerView和ListView的性能对比
[https://blog.csdn.net/fanenqian/article/details/61191532](https://blog.csdn.net/fanenqian/article/details/61191532)

（8.26）ListView的优化
可以说上分页加载哦
[https://www.cnblogs.com/yuhanghzsd/p/5595532.html](https://www.cnblogs.com/yuhanghzsd/p/5595532.html)

（8.27）RecycleView优化
[https://blog.csdn.net/axi295309066/article/details/52741810](https://blog.csdn.net/axi295309066/article/details/52741810)
[https://www.jianshu.com/p/411ab861034f](https://www.jianshu.com/p/411ab861034f)

（8.28）View渲染
[https://www.cnblogs.com/ldq2016/p/6668148.html](https://www.cnblogs.com/ldq2016/p/6668148.html)

（8.29）Bitmap如何处理大图，如一张30M的大图，如何预防OOM
重点是在对于对内存的了解以及内存使用率的掌握
[https://blog.csdn.net/guolin_blog/article/details/9316683](https://blog.csdn.net/guolin_blog/article/details/9316683)

（8.30）java中的四种引用的区别以及使用场景
[https://blog.csdn.net/qq_23547831/article/details/46505287](https://blog.csdn.net/qq_23547831/article/details/46505287)

（8.31）强引用置为null，会不会被回收？
会，GC执行时，就被回收掉，前提是没有被引用的对象
一定要了解垃圾回收原理
[https://blog.csdn.net/qq_33048603/article/details/52727991](https://blog.csdn.net/qq_33048603/article/details/52727991)

**9.JNI**

（9.1）请介绍一下NDK
[https://www.jianshu.com/p/fa613762f516](https://www.jianshu.com/p/fa613762f516)
[https://blog.csdn.net/carson_ho/article/details/73250163](https://blog.csdn.net/carson_ho/article/details/73250163)

（9.2）什么是NDK库?
[https://blog.csdn.net/carson_ho/article/details/73250163](https://blog.csdn.net/carson_ho/article/details/73250163)

（9.3）如何在JNI中注册native函数，有几种注册方式?
[https://blog.csdn.net/wwj_748/article/details/52347341](https://blog.csdn.net/wwj_748/article/details/52347341)

（9.4）Java如何调用c、c++语言？
[https://www.jianshu.com/p/27404a899d88](https://www.jianshu.com/p/27404a899d88)

（9.5）JNI如何调用java层代码？
[https://www.jianshu.com/p/4893848a3249](https://www.jianshu.com/p/4893848a3249)

（9.6）你用JNI来实现过什么功能吗？怎么实现的？
加密处理、影音方面、图形图像处理
[https://blog.csdn.net/summer0527/article/details/1827186](https://blog.csdn.net/summer0527/article/details/1827186)

**10.进程间通信（简称：IPC）**

（10.1）进程间通信的方式？
[https://www.jianshu.com/p/ce1e35c84134](https://www.jianshu.com/p/ce1e35c84134)

（10.2）Binder机制的作用和原理
[http://www.cnblogs.com/innost/archive/2011/01/09/1931456.html](http://www.cnblogs.com/innost/archive/2011/01/09/1931456.html)
[https://blog.csdn.net/luoshengyang/article/details/6618363/](https://blog.csdn.net/luoshengyang/article/details/6618363/)

（10.3）简述IPC？
[https://blog.csdn.net/luoshengyang/article/details/6618363/](https://blog.csdn.net/luoshengyang/article/details/6618363/)

（10.4）什么是AIDL？
[https://www.jianshu.com/p/d1fac6ccee98](https://www.jianshu.com/p/d1fac6ccee98)
[https://www.jianshu.com/p/a5c73da2e9be](https://www.jianshu.com/p/a5c73da2e9be)

（10.5）AIDL解决了什么问题？
官方文档：
Note: Using AIDL is necessary only if you allow clients from different applications to access your service for IPC and want to handle multithreading in your service. If you do not need to perform concurrent IPC across different applications, you should create your interface by implementing a Binder or, if you want to perform IPC, but do not need to handle multithreading, implement your interface using a Messenger. Regardless, be sure that you understand Bound Services before implementing an AIDL.
“只有当你允许来自不同的客户端访问你的服务并且需要处理多线程问题时你才必须使用AIDL”

（10.6）AIDL如何使用？
[https://www.jianshu.com/p/d1fac6ccee98](https://www.jianshu.com/p/d1fac6ccee98)
[https://www.jianshu.com/p/a5c73da2e9be](https://www.jianshu.com/p/a5c73da2e9be)

（10.8）Android进程分类？
[https://blog.csdn.net/zhongshujunqia/article/details/72458271](https://blog.csdn.net/zhongshujunqia/article/details/72458271)

（10.9）进程和 Application 的生命周期？

（10.10）进程调度
[https://blog.csdn.net/innost/article/details/6940136](https://blog.csdn.net/innost/article/details/6940136)

（10.11）谈谈对进程共享和线程安全的认识
[https://blog.csdn.net/coding_glacier/article/details/8230159](https://blog.csdn.net/coding_glacier/article/details/8230159)
[https://blog.csdn.net/oweixiao123/article/details/9057445](https://blog.csdn.net/oweixiao123/article/details/9057445)
问你线程安全的时候，不止要回答主线程跟子线程之间的切换，还有数据结构处理的线程安全问题，多线程操作同一个数据的一致性问题，等等。

**11.WebView**

[https://www.jianshu.com/p/3c94ae673e2a](https://www.jianshu.com/p/3c94ae673e2a)
[https://www.jianshu.com/p/52ec85259ccc](https://www.jianshu.com/p/52ec85259ccc)
过一遍这个

（11.1）描述一下Webview的作用
WebView控件功能强大，除了具有一般View的属性和设置外，还可以对url请求、页面加载、渲染、页面交互进行强大的处理。

（11.2）WebView的内核是什么
Android的Webview在低版本和高版本采用了不同的webkit版本内核，4.4后直接使用了Chrome。

（11.3）描述一下WebView与js的交互方式
[https://blog.csdn.net/carson_ho/article/details/64904691](https://blog.csdn.net/carson_ho/article/details/64904691)

（11.4）描述一下WebView的缓存机制
[https://www.jianshu.com/p/5e7075f4875f](https://www.jianshu.com/p/5e7075f4875f)

（11.5）关于WebView的优化你知道哪些
[https://www.jianshu.com/p/95d4d73be3d1](https://www.jianshu.com/p/95d4d73be3d1)

（11.6）有没有用过第三方WebView组件？讲一讲优势
[https://www.jianshu.com/p/d3ef9c62b6c8](https://www.jianshu.com/p/d3ef9c62b6c8)

**12.进程保活**

关于守护进程、不死进程、进程保活这些话题，有几句话想说一下：
这个近期是面的越来越少。在google的控制下，高版本基本上是扼杀了这种无赖行为，市面上现在做进程保活基本都是走厂商白名单和系统签名进程等方式，又或者应用之间互相拉起，各大应用相互合作。
但并不是说不能做，只能用各种方式混搭，去提高保活的成功率。

熟悉进程保活的干货：

[https://www.jianshu.com/p/63aafe3c12af](https://www.jianshu.com/p/63aafe3c12af)
[https://blog.csdn.net/tencent_bugly/article/details/52192423](https://blog.csdn.net/tencent_bugly/article/details/52192423)
[https://www.cnblogs.com/Doing-what-I-love/p/5530291.html](https://www.cnblogs.com/Doing-what-I-love/p/5530291.html)

还有一个大神深入研究这个话题，并开源出自己的代码，在github上挺受欢迎的，大家可以去拿来试一下：
blog：[https://blog.csdn.net/marswin89/article/details/48015453](https://blog.csdn.net/marswin89/article/details/48015453)
github：[https://github.com/Marswin/MarsDaemon](https://github.com/Marswin/MarsDaemon)

看完以上文章，so以下的问题大家心里都有数了吧？

（12.1）做过进程保活吗？

（12.2）5.0下和5.0上的保活方式了解吗？

（12.3）描述一下进程回收的过程

（12.4）如何降低进程的oom_adj

**13.杂7杂8**

（13.1）Handler机制和底层实现
[https://www.cnblogs.com/ryanleee/p/8204450.html](https://www.cnblogs.com/ryanleee/p/8204450.html)
[https://www.jianshu.com/p/b03d46809c4d](https://www.jianshu.com/p/b03d46809c4d)

（13.2）Handler、Thread和HandlerThread的差别
[https://blog.csdn.net/lmj623565791/article/details/47079737/](https://blog.csdn.net/lmj623565791/article/details/47079737/)

（13.3）handler发消息给子线程，looper怎么启动？
什么问题呢。。发消息就是把消息塞进去消息队列，looper在应用起来的时候已经就启动了，一直在轮询取消息队列的消息。

（13.4）关于Handler，在任何地方new Handler 都是什么线程下?
我自己看不太懂这个问题

（13.5）ThreadLocal原理，实现及如何保证Local属性？
[https://blog.csdn.net/singwhatiwanna/article/details/48350919](https://blog.csdn.net/singwhatiwanna/article/details/48350919)

（13.6）请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系
[https://blog.csdn.net/lovedren/article/details/51701477](https://blog.csdn.net/lovedren/article/details/51701477)

（13.7）AsyncTask机制
[https://blog.csdn.net/yanbober/article/details/46117397](https://blog.csdn.net/yanbober/article/details/46117397)
（13.8）AsyncTask原理及不足
[https://www.cnblogs.com/absfree/p/5357678.html](https://www.cnblogs.com/absfree/p/5357678.html)

（13.9）如何取消AsyncTask？
调用cancel()
但是：
[https://www.jianshu.com/p/0c6f4b6ed558](https://www.jianshu.com/p/0c6f4b6ed558)

（13.10）为什么不能在子线程更新UI？
[https://blog.csdn.net/cewei711/article/details/53028358?locationNum=2&fps=1](https://blog.csdn.net/cewei711/article/details/53028358?locationNum=2&fps=1)

（13.11）LruCache默认内存缓存大小
基本上设置为手机内存的1/8

（13.12）ContentProvider的权限管理(解答：读写分离，权限控制-精确到表级，URL控制)

（13.13）如何通过广播拦截和abort一条短信？
[https://blog.csdn.net/ljw124213/article/details/50492449](https://blog.csdn.net/ljw124213/article/details/50492449)

（13.14）广播是否可以请求网络？
子线程可以，主线程超过10s引起anr

（13.15）广播引起anr的时间限制是多少？
onReceive的生命周期为10秒
[https://blog.csdn.net/me_dong/article/details/53582115](https://blog.csdn.net/me_dong/article/details/53582115)

（13.16）描述一下Activity栈
[https://www.jianshu.com/p/c1386015856a](https://www.jianshu.com/p/c1386015856a)

（13.17）Android线程有没有上限？
跟内存挂钩，我也不太清楚，自己查哈

（13.18）线程池有没有上限？
跟内存挂钩，我也不太清楚，自己查哈
[http://www.trinea.cn/android/java-android-thread-pool/](http://www.trinea.cn/android/java-android-thread-pool/)
[https://blog.csdn.net/cfy137000/article/details/51422316](https://blog.csdn.net/cfy137000/article/details/51422316)

（13.19）ListView重用的是什么？
[https://blog.csdn.net/u011692041/article/details/53099584](https://blog.csdn.net/u011692041/article/details/53099584)

（13.20）Android为什么引入Parcelable？
[https://blog.csdn.net/jaycee110905/article/details/21517853](https://blog.csdn.net/jaycee110905/article/details/21517853)

（13.21）有没有尝试简化Parcelable的使用？
as的插件

（13.22）ListView 中图片错位的问题是如何产生的?
[https://blog.csdn.net/a394268045/article/details/50726560](https://blog.csdn.net/a394268045/article/details/50726560)

（13.23）混合开发有了解吗？
[https://blog.csdn.net/sinat_33195772/article/details/72961814](https://blog.csdn.net/sinat_33195772/article/details/72961814)

（13.24）知道哪些混合开发的方式？说出它们的优缺点和各自使用场景？（解答：比如:RN，weex，H5，小程序，WPA等)
[https://www.jianshu.com/p/22aa14664cf9](https://www.jianshu.com/p/22aa14664cf9)
[https://www.jianshu.com/p/52071a3d07b4](https://www.jianshu.com/p/52071a3d07b4)

（13.25）屏幕适配的处理技巧都有哪些?
[https://blog.csdn.net/wangwangli6/article/details/63258270](https://blog.csdn.net/wangwangli6/article/details/63258270)

（13.26）服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达？

（13.27）动态布局的理解
[https://blog.csdn.net/ustory/article/details/42424313](https://blog.csdn.net/ustory/article/details/42424313)

（13.28）画出 Android 的大体架构图
[https://blog.csdn.net/wang5318330/article/details/51917092](https://blog.csdn.net/wang5318330/article/details/51917092)

（13.29）Recycleview和ListView的区别
[https://blog.csdn.net/sanjay_f/article/details/48830311](https://blog.csdn.net/sanjay_f/article/details/48830311)
（13.30）ListView图片加载错乱的原理和解决方案
[https://blog.csdn.net/a394268045/article/details/50726560](https://blog.csdn.net/a394268045/article/details/50726560)

（13.31）动态权限适配方案，权限组的概念
[https://blog.csdn.net/xc765926174/article/details/49103483](https://blog.csdn.net/xc765926174/article/details/49103483)

（13.32）Android系统为什么会设计ContentProvider？
重点，跨进程
[https://www.cnblogs.com/zhainanJohnny/articles/3275908.html](https://www.cnblogs.com/zhainanJohnny/articles/3275908.html)

（13.33）下拉状态栏是不是影响activity的生命周期
不会

（13.36）Bitmap 使用时候注意什么？
[https://blog.csdn.net/newbie_coder/article/details/9842995](https://blog.csdn.net/newbie_coder/article/details/9842995)

（13.37）Bitmap的recycler()
[https://blog.csdn.net/lonelyroamer/article/details/7569248](https://blog.csdn.net/lonelyroamer/article/details/7569248)

（13.38）Android中开启摄像头的主要步骤
[https://www.yiibai.com/android/android_camera.html](https://www.yiibai.com/android/android_camera.html)

（13.39）ViewPager使用细节，如何设置成每次只初始化当前的
懒加载
[https://www.jianshu.com/p/e324e8378948](https://www.jianshu.com/p/e324e8378948)
[https://blog.csdn.net/linglongxin24/article/details/53205878](https://blog.csdn.net/linglongxin24/article/details/53205878)

（13.41）点击事件被拦截，但是想传到下面的View，如何操作？
问题就是viewgroup被拦截，要传到子view那里，好好看这篇分发机制的文章，你就知道了
[https://www.jianshu.com/p/e99b5e8bd67b](https://www.jianshu.com/p/e99b5e8bd67b)

（13.42）描述一下微信主页面的实现方式
自己去研究下吧这个，无非fragment嵌套

（13.43）invalidate和postInvalidate的区别及使用
[https://blog.csdn.net/mars2639/article/details/6650876](https://blog.csdn.net/mars2639/article/details/6650876)

（13.44）Activity-Window-View三者的差别
[https://www.jianshu.com/p/a533467f5af5](https://www.jianshu.com/p/a533467f5af5)

（13.45）谈谈对Volley的理解
[https://blog.csdn.net/ysh06201418/article/details/46443235](https://blog.csdn.net/ysh06201418/article/details/46443235)

（13.46）ActivityThread，AMS，WMS的工作原理
[https://www.jianshu.com/p/47eca41428d6](https://www.jianshu.com/p/47eca41428d6)

（13.47）LaunchMode应用场景
[https://blog.csdn.net/CodeEmperor/article/details/50481726](https://blog.csdn.net/CodeEmperor/article/details/50481726)

（13.48）SpareArray原理
[https://blog.csdn.net/easyer2012/article/details/37871031](https://blog.csdn.net/easyer2012/article/details/37871031)

（13.49）请介绍下ContentProvider 是如何实现数据共享的？
[https://blog.csdn.net/yhaolpz/article/details/51304345](https://blog.csdn.net/yhaolpz/article/details/51304345)

（13.50）IntentService原理及作用是什么？
[https://www.jianshu.com/p/4dd46616564d](https://www.jianshu.com/p/4dd46616564d)

（13.51）ApplicationContext和ActivityContext的区别
[https://www.jianshu.com/p/94e0f9ab3f1d](https://www.jianshu.com/p/94e0f9ab3f1d)

（13.53）封装View的时候怎么知道view的大小
[https://blog.csdn.net/fwt336/article/details/52979876](https://blog.csdn.net/fwt336/article/details/52979876)

（13.55）AndroidManifest的作用与理解
[https://www.jianshu.com/p/6ed30112d4a4](https://www.jianshu.com/p/6ed30112d4a4)

上面试题来源:https://blog.csdn.net/huangqili1314/article/details/79824830

-----

## 一、java面试题

熟练掌握java是很关键的，大公司不仅仅要求你会使用几个api，更多的是要你熟悉源码实现原理，甚至要你知道有哪些不足，怎么改进，还有一些java有关的一些算法，设计模式等等。

##### （一） java基础面试知识点

*   java中==和equals和hashCode的区别
*   int、char、long各占多少字节数
*   int与integer的区别
*   探探对java多态的理解
*   String、StringBuffer、StringBuilder区别
*   什么是内部类？内部类的作用
*   抽象类和接口区别
*   抽象类的意义
*   抽象类与接口的应用场景
*   抽象类是否可以没有方法和属性？
*   接口的意义
*   泛型中extends和super的区别
*   父类的静态方法能否被子类重写
*   进程和线程的区别
*   final，finally，finalize的区别
*   序列化的方式
*   Serializable 和Parcelable 的区别
*   静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？
*   静态内部类的设计意图
*   成员内部类、静态内部类、局部内部类和匿名内部类的理解，以及项目中的应用
*   谈谈对kotlin的理解
*   闭包和局部内部类的区别
*   string 转换成 integer的方式及原理

##### （二） java深入源码级的面试题（有难度）

*   哪些情况下的对象会被垃圾回收机制处理掉？
*   讲一下常见编码方式？
*   utf-8编码中的中文占几个字节；int型几个字节？
*   静态代理和动态代理的区别，什么场景使用？
*   Java的异常体系
*   谈谈你对解析与分派的认识。
*   修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？
*   Java中实现多态的机制是什么？
*   如何将一个Java对象序列化到文件里？
*   说说你对Java反射的理解
*   说说你对Java注解的理解
*   说说你对依赖注入的理解
*   说一下泛型原理，并举例说明
*   Java中String的了解
*   String为什么要设计成不可变的？
*   Object类的equal和hashCode方法重写，为什么？

##### （三） 数据结构

*   常用数据结构简介
*   并发集合了解哪些？
*   列举java的集合以及集合之间的继承关系
*   集合类以及集合框架
*   容器类介绍以及之间的区别（容器类估计很多人没听这个词，Java容器主要可以划分为4个部分：List列表、Set集合、Map映射、工具类（Iterator迭代器、Enumeration枚举类、Arrays和Collections），具体的可以看看这篇博文 [Java容器类](http://alexyyek.github.io/2015/04/06/Collection/)）
*   List,Set,Map的区别
*   List和Map的实现方式以及存储方式
*   HashMap的实现原理
*   HashMap数据结构？
*   HashMap源码理解
*   HashMap如何put数据（从HashMap源码角度讲解）？
*   HashMap怎么手写实现？
*   ConcurrentHashMap的实现原理
*   ArrayMap和HashMap的对比
*   HashTable实现原理
*   TreeMap具体实现
*   HashMap和HashTable的区别
*   HashMap与HashSet的区别
*   HashSet与HashMap怎么判断集合元素重复？
*   集合Set实现Hash怎么防止碰撞
*   ArrayList和LinkedList的区别，以及应用场景
*   数组和链表的区别
*   二叉树的深度优先遍历和广度优先遍历的具体实现
*   堆的结构
*   堆和树的区别
*   堆和栈在内存中的区别是什么(解答提示：可以从数据结构方面以及实际实现方面两个方面去回答)？
*   什么是深拷贝和浅拷贝
*   手写链表逆序代码
*   讲一下对树，B+树的理解
*   讲一下对图的理解
*   判断单链表成环与否？
*   链表翻转（即：翻转一个单项链表）
*   合并多个单有序链表（假设都是递增的）

##### （四） 线程、多线程和线程池

*   开启线程的三种方式？
*   线程和进程的区别？
*   为什么要有线程，而不是仅仅用进程？
*   run()和start()方法区别
*   如何控制某个方法允许并发访问线程的个数？
*   在Java中wait和seelp方法的不同；
*   谈谈wait/notify关键字的理解
*   什么导致线程阻塞？
*   线程如何关闭？
*   讲一下java中的同步的方法
*   数据一致性如何保证？
*   如何保证线程安全？
*   如何实现线程同步？
*   两个进程同时要求写或者读，能不能实现？如何防止进程的同步？
*   线程间操作List
*   Java中对象的生命周期
*   Synchronized用法
*   synchronize的原理
*   谈谈对Synchronized关键字，类锁，方法锁，重入锁的理解
*   static synchronized 方法的多线程访问和作用
*   同一个类里面两个synchronized方法，两个线程同时访问的问题
*   volatile的原理
*   谈谈volatile关键字的用法
*   谈谈volatile关键字的作用
*   谈谈NIO的理解
*   synchronized 和volatile 关键字的区别
*   synchronized与Lock的区别
*   ReentrantLock 、synchronized和volatile比较
*   ReentrantLock的内部实现
*   lock原理
*   死锁的四个必要条件？
*   怎么避免死锁？
*   对象锁和类锁是否会互相影响？
*   什么是线程池，如何使用?
*   Java的并发、多线程、线程模型
*   谈谈对多线程的理解
*   多线程有什么要注意的问题？
*   谈谈你对并发编程的理解并举例说明
*   谈谈你对多线程同步机制的理解？
*   如何保证多线程读写文件的安全？
*   多线程断点续传原理
*   断点续传的实现

##### （五）并发编程有关知识点（这个是一般Android开发用的少的，所以建议多去看看）：

平时Android开发中对并发编程可以做得比较少，Thread这个类经常会用到，但是我们想提升自己的话，一定不能停留在表面，,我们也应该去了解一下java的关于线程相关的源码级别的东西。

**学习的参考资料如下：**

> Java 内存模型

*   [java线程安全总结](http://www.iteye.com/topic/806990)
*   [深入理解java内存模型系列文章](http://ifeve.com/java-memory-model-0/)

> 线程状态：

*   [一张图让你看懂JAVA线程间的状态转换](https://my.oschina.net/mingdongcheng/blog/139263)

> 锁：

*   [锁机制：synchronized、Lock、Condition](http://blog.csdn.net/vking_wang/article/details/9952063)
*   [Java 中的锁](http://wiki.jikexueyuan.com/project/java-concurrent/locks-in-java.html)

> 并发编程：

*   [Java并发编程：Thread类的使用](http://www.cnblogs.com/dolphin0520/p/3920357.html)
*   [Java多线程编程总结](http://blog.51cto.com/lavasoft/27069)
*   [Java并发编程的总结与思考](https://www.jianshu.com/p/053943a425c3#)
*   [Java并发编程实战—–synchronized](http://www.cnblogs.com/chenssy/p/4701027.html)
*   [深入分析ConcurrentHashMap](http://www.infoq.com/cn/articles/ConcurrentHashMap#)

* * *

## 二、Android面试题

Android面试题包括Android基础，还有一些源码级别的、原理这些等。所以想去大公司面试，一定要多看看源码和实现方式，常用框架可以试试自己能不能手写实现一下，锻炼一下自己。

##### （一）Android基础知识点

*   四大组件是什么
*   四大组件的生命周期和简单用法
*   Activity之间的通信方式
*   Activity各种情况下的生命周期
*   横竖屏切换的时候，Activity 各种情况下的生命周期
*   Activity与Fragment之间生命周期比较
*   Activity上有Dialog的时候按Home键时的生命周期
*   两个Activity 之间跳转时必然会执行的是哪几个方法？
*   前台切换到后台，然后再回到前台，Activity生命周期回调方法。弹出Dialog，生命值周期回调方法。
*   Activity的四种启动模式对比
*   Activity状态保存于恢复
*   fragment各种情况下的生命周期
*   Fragment状态保存startActivityForResult是哪个类的方法，在什么情况下使用？
*   如何实现Fragment的滑动？
*   fragment之间传递数据的方式？
*   Activity 怎么和Service 绑定？
*   怎么在Activity 中启动自己对应的Service？
*   service和activity怎么进行数据交互？
*   Service的开启方式
*   请描述一下Service 的生命周期
*   谈谈你对ContentProvider的理解
*   说说ContentProvider、ContentResolver、ContentObserver 之间的关系
*   请描述一下广播BroadcastReceiver的理解
*   广播的分类
*   广播使用的方式和场景
*   在manifest 和代码中如何注册和使用BroadcastReceiver?
*   本地广播和全局广播有什么差别？
*   BroadcastReceiver，LocalBroadcastReceiver 区别
*   AlertDialog,popupWindow,Activity区别
*   Application 和 Activity 的 Context 对象的区别
*   Android属性动画特性
*   如何导入外部数据库?
*   LinearLayout、RelativeLayout、FrameLayout的特性及对比，并介绍使用场景。
*   谈谈对接口与回调的理解
*   回调的原理
*   写一个回调demo
*   介绍下SurfView
*   RecycleView的使用
*   序列化的作用，以及Android两种序列化的区别
*   差值器
*   估值器
*   Android中数据存储方式

##### （二）Android源码相关分析

*   Android动画框架实现原理
*   Android各个版本API的区别
*   Requestlayout，onlayout，onDraw，DrawChild区别与联系
*   invalidate和postInvalidate的区别及使用
*   Activity-Window-View三者的差别
*   谈谈对Volley的理解
*   如何优化自定义View
*   低版本SDK如何实现高版本api？
*   描述一次网络请求的流程
*   HttpUrlConnection 和 okhttp关系
*   Bitmap对象的理解
*   looper架构
*   ActivityThread，AMS，WMS的工作原理
*   自定义View如何考虑机型适配
*   自定义View的事件
*   AstncTask+HttpClient 与 AsyncHttpClient有什么区别？
*   LaunchMode应用场景
*   AsyncTask 如何使用?
*   SpareArray原理
*   请介绍下ContentProvider 是如何实现数据共享的？
*   AndroidService与Activity之间通信的几种方式
*   IntentService原理及作用是什么？
*   说说Activity、Intent、Service 是什么关系
*   ApplicationContext和ActivityContext的区别
*   SP是进程同步的吗?有什么方法做到同步？
*   谈谈多线程在Android中的使用
*   进程和 Application 的生命周期
*   封装View的时候怎么知道view的大小
*   RecycleView原理
*   AndroidManifest的作用与理解

##### （三）常见的一些原理性问题

*   Handler机制和底层实现
*   Handler、Thread和HandlerThread的差别
*   handler发消息给子线程，looper怎么启动？
*   关于Handler，在任何地方new Handler 都是什么线程下?
*   ThreadLocal原理，实现及如何保证Local属性？
*   请解释下在单线程模型中Message、Handler、Message Queue、Looper之间的关系
*   请描述一下View事件传递分发机制
*   Touch事件传递流程
*   事件分发中的onTouch 和onTouchEvent 有什么区别，又该如何使用？
*   View和ViewGroup分别有哪些事件分发相关的回调方法
*   View刷新机制
*   View绘制流程
*   自定义控件原理
*   自定义View如何提供获取View属性的接口？
*   Android代码中实现WAP方式联网
*   AsyncTask机制
*   AsyncTask原理及不足
*   如何取消AsyncTask？
*   为什么不能在子线程更新UI？
*   ANR产生的原因是什么？
*   ANR定位和修正
*   oom是什么？
*   什么情况导致oom？
*   有什么解决方法可以避免OOM？
*   Oom 是否可以try catch？为什么？
*   内存泄漏是什么？
*   什么情况导致内存泄漏？
*   如何防止线程的内存泄漏？
*   内存泄露场的解决方法
*   内存泄漏和内存溢出区别？
*   LruCache默认缓存大小
*   ContentProvider的权限管理(解答：读写分离，权限控制-精确到表级，URL控制)
*   如何通过广播拦截和abort一条短信？
*   广播是否可以请求网络？
*   广播引起anr的时间限制是多少？
*   计算一个view的嵌套层级
*   Activity栈
*   Android线程有没有上限？
*   线程池有没有上限？
*   ListView重用的是什么？
*   Android为什么引入Parcelable？
*   有没有尝试简化Parcelable的使用？

##### （四）开发中常见的一些问题

*   ListView 中图片错位的问题是如何产生的?
*   混合开发有了解吗？
*   知道哪些混合开发的方式？说出它们的优缺点和各自使用场景？（解答：比如:RN，weex，H5，小程序，WPA等。做Android的了解一些前端js等还是很有好处的)；
*   屏幕适配的处理技巧都有哪些?
*   服务器只提供数据接收接口，在多线程或多进程条件下，如何保证数据的有序到达？
*   动态布局的理解
*   怎么去除重复代码？
*   画出 Android 的大体架构图
*   Recycleview和ListView的区别
*   ListView图片加载错乱的原理和解决方案
*   动态权限适配方案，权限组的概念
*   Android系统为什么会设计ContentProvider？
*   下拉状态栏是不是影响activity的生命周期
*   如果在onStop的时候做了网络请求，onResume的时候怎么恢复？
*   Bitmap 使用时候注意什么？
*   Bitmap的recycler()
*   Android中开启摄像头的主要步骤
*   ViewPager使用细节，如何设置成每次只初始化当前的Fragment，其他的不初始化？
*   点击事件被拦截，但是想传到下面的View，如何操作？
*   微信主页面的实现方式
*   微信上消息小红点的原理
*   CAS介绍（这是阿里巴巴的面试题，我不是很了解，可以参考博客: [CAS简介](http://blog.csdn.net/jly4758/article/details/46673835)）

* * *

## 三、高端技术面试题

**这里讲的是大公司需要用到的一些高端Android技术，这里专门整理了一个文档，希望大家都可以看看。这些题目有点技术含量，需要好点时间去研究一下的。**

##### （一）图片

*   图片库对比
*   图片库的源码分析
*   图片框架缓存实现
*   LRUCache原理
*   图片加载原理
*   自己去实现图片库，怎么做？
*   Glide源码解析
*   Glide使用什么缓存？
*   Glide内存缓存如何控制大小？

##### （二）网络和安全机制

*   网络框架对比和源码分析
*   自己去设计网络请求框架，怎么做？
*   okhttp源码
*   网络请求缓存处理，okhttp如何处理网络缓存的
*   从网络加载一个10M的图片，说下注意事项
*   TCP的3次握手和四次挥手
*   TCP与UDP的区别
*   TCP与UDP的应用
*   HTTP协议
*   HTTP1.0与2.0的区别
*   HTTP报文结构
*   HTTP与HTTPS的区别以及如何实现安全性
*   如何验证证书的合法性?
*   https中哪里用了对称加密，哪里用了非对称加密，对加密算法（如RSA）等是否有了解?
*   client如何确定自己发送的消息被server收到?
*   谈谈你对WebSocket的理解
*   WebSocket与socket的区别
*   谈谈你对安卓签名的理解。
*   请解释安卓为啥要加签名机制?
*   视频加密传输
*   App 是如何沙箱化，为什么要这么做？
*   权限管理系统（底层的权限是如何进行 grant 的）？

##### （三）数据库

*   sqlite升级，增加字段的语句
*   数据库框架对比和源码分析
*   数据库的优化
*   数据库数据迁移问题

##### （四）算法

*   排序算法有哪些？
*   最快的排序算法是哪个？
*   手写一个冒泡排序
*   手写快速排序代码
*   快速排序的过程、时间复杂度、空间复杂度
*   手写堆排序
*   堆排序过程、时间复杂度及空间复杂度
*   写出你所知道的排序算法及时空复杂度，稳定性
*   二叉树给出根节点和目标节点，找出从根节点到目标节点的路径
*   给阿里2万多名员工按年龄排序应该选择哪个算法？
*   GC算法(各种算法的优缺点以及应用场景)
*   蚁群算法与蒙特卡洛算法
*   子串包含问题(KMP 算法)写代码实现
*   一个无序，不重复数组，输出N个元素，使得N个元素的和相加为M，给出时间复杂度、空间复杂度。手写算法
*   万亿级别的两个URL文件A和B，如何求出A和B的差集C(提示：Bit映射->hash分组->多文件读写效率->磁盘寻址以及应用层面对寻址的优化)
*   百度POI中如何试下查找最近的商家功能(提示：坐标镜像+R树)。
*   两个不重复的数组集合中，求共同的元素。
*   两个不重复的数组集合中，这两个集合都是海量数据，内存中放不下，怎么求共同的元素？
*   一个文件中有100万个整数，由空格分开，在程序中判断用户输入的整数是否在此文件中。说出最优的方法
*   一张Bitmap所占内存以及内存占用的计算
*   2000万个整数，找出第五十大的数字？
*   烧一根不均匀的绳，从头烧到尾总共需要1个小时。现在有若干条材质相同的绳子，问如何用烧绳的方法来计时一个小时十五分钟呢？
*   求1000以内的水仙花数以及40亿以内的水仙花数
*   5枚硬币，2正3反如何划分为两堆然后通过翻转让两堆中正面向上的硬8币和反面向上的硬币个数相同
*   时针走一圈，时针分针重合几次
*   N*N的方格纸,里面有多少个正方形
*   x个苹果，一天只能吃一个、两个、或者三个，问多少天可以吃完？

##### （五）插件化、模块化、组件化、热修复、增量更新、Gradle

*   对热修复和插件化的理解
*   插件化原理分析
*   模块化实现（好处，原因）
*   热修复,插件化
*   项目组件化的理解
*   描述清点击 Android Studio 的 build 按钮后发生了什么

##### （六）架构设计和设计模式

*   谈谈你对Android设计模式的理解
*   MVC MVP MVVM原理和区别
*   你所知道的设计模式有哪些？
*   项目中常用的设计模式
*   手写生产者/消费者模式
*   写出观察者模式的代码
*   适配器模式，装饰者模式，外观模式的异同？
*   用到的一些开源框架，介绍一个看过源码的，内部实现过程。
*   谈谈对RxJava的理解
*   RxJava的功能与原理实现
*   RxJava的作用，与平时使用的异步操作来比的优缺点
*   说说EventBus作用，实现方式，代替EventBus的方式
*   从0设计一款App整体架构，如何去做？
*   说一款你认为当前比较火的应用并设计(比如：直播APP，P2P金融，小视频等)
*   谈谈对java状态机理解
*   Fragment如果在Adapter中使用应该如何解耦？
*   Binder机制及底层实现
*   对于应用更新这块是如何做的？(解答：灰度，强制更新，分区域更新)？
*   实现一个Json解析器(可以通过正则提高速度)
*   统计启动时长,标准

##### （七）性能优化

*   如何对Android 应用进行性能分析以及优化?
*   ddms 和 traceView
*   性能优化如何分析systrace？
*   用IDE如何分析内存泄漏？
*   Java多线程引发的性能问题，怎么解决？
*   启动页白屏及黑屏解决？
*   启动太慢怎么解决？
*   怎么保证应用启动不卡顿？
*   App启动崩溃异常捕捉
*   自定义View注意事项
*   现在下载速度很慢,试从网络协议的角度分析原因,并优化(提示：网络的5层都可以涉及)。
*   Https请求慢的解决办法（提示：DNS，携带数据，直接访问IP）
*   如何保持应用的稳定性
*   RecyclerView和ListView的性能对比
*   ListView的优化
*   RecycleView优化
*   View渲染
*   Bitmap如何处理大图，如一张30M的大图，如何预防OOM
*   java中的四种引用的区别以及使用场景
*   强引用置为null，会不会被回收？

##### （八）NDK、jni、Binder、AIDL、进程通信有关

*   请介绍一下NDK
*   什么是NDK库?
*   jni用过吗？
*   如何在jni中注册native函数，有几种注册方式?
*   Java如何调用c、c++语言？
*   jni如何调用java层代码？
*   进程间通信的方式？
*   Binder机制
*   简述IPC？
*   什么是AIDL？
*   AIDL解决了什么问题？
*   AIDL如何使用？
*   Android 上的 Inter-Process-Communication 跨进程通信时如何工作的？
*   多进程场景遇见过么？
*   Android进程分类？
*   进程和 Application 的生命周期？
*   进程调度
*   谈谈对进程共享和线程安全的认识
*   谈谈对多进程开发的理解以及多进程应用场景
*   什么是协程？

##### （九）framework层、ROM定制、Ubuntu、Linux之类的问题

*   java虚拟机的特性
*   谈谈对jvm的理解
*   JVM内存区域，开线程影响哪块内存
*   对Dalvik、ART虚拟机有什么了解？
*   Art和Dalvik对比
*   虚拟机原理，如何自己设计一个虚拟机(内存管理，类加载，双亲委派)
*   谈谈你对双亲委派模型理解
*   JVM内存模型，内存区域
*   类加载机制
*   谈谈对ClassLoader(类加载器)的理解
*   谈谈对动态加载（OSGI）的理解
*   内存对象的循环引用及避免
*   内存回收机制、GC回收策略、GC原理时机以及GC对象
*   垃圾回收机制与调用System.gc()区别
*   Ubuntu编译安卓系统
*   系统启动流程是什么？（提示：Zygote进程 –> SystemServer进程 –> 各种系统服务 –> 应用进程）
*   大体说清一个应用程序安装到手机上时发生了什么
*   简述Activity启动全部过程
*   App启动流程，从点击桌面开始
*   逻辑地址与物理地址，为什么使用逻辑地址？
*   Android为每个应用程序分配的内存大小是多少？
*   Android中进程内存的分配，能不能自己分配定额内存？
*   进程保活的方式
*   如何保证一个后台服务不被杀死？（相同问题：如何保证service在后台不被kill？）比较省电的方式是什么？
*   App中唤醒其他进程的实现方式

* * *

## 四、非技术性问题&HR问题汇总

**这里整理的是一些与技术没有直接关系的面试题，但是能够考察你的综合水平，所以不要以为不是技术问题，就不看，往往有时候就是这样一些细节的题目被忽视，而错过了一次次面试机会。**

##### （一）非技术问题

*   介绍你做过的哪些项目
*   都使用过哪些框架、平台？
*   都使用过哪些自定义控件？
*   研究比较深入的领域有哪些？
*   对业内信息的关注渠道有哪些？
*   最近都读哪些书？
*   有没有什么开源项目？
*   自己最擅长的技术点，最感兴趣的技术领域和技术点
*   项目中用了哪些开源库，如何避免因为引入开源库而导致的安全性和稳定性问题
*   实习过程中做了什么，有什么产出？

##### （二）HR提出的面试问题

*   您在前一家公司的离职原因是什么？
*   讲一件你印象最深的一件事情
*   介绍一个你影响最深的项目
*   介绍你最热爱最擅长的专业领域
*   公司实习最大的收获是什么？
*   与上级意见不一致时，你将怎么办？
*   自己的优点和缺点是什么？并举例说明？
*   你的学习方法是什么样的？实习过程中如何学习？实习项目中遇到的最大困难是什么以及如何解决的？
*   说一件最能证明你能力的事情
*   针对你你申请的这个职位，你认为你还欠缺什么
*   如果通过这次面试我们单位录用了你，但工作一段时间却发现你根本不适合这个职位，你怎么办？
*   项目中遇到最大的困难是什么？如何解决的？
*   你的职业规划以及个人目标、未来发展路线及求职定位
*   如果你在这次面试中没有被录用，你怎么打算？
*   评价下自己，评价下自己的技术水平，个人代码量如何？
*   通过哪些渠道了解的招聘信息，其他同学都投了哪些公司？
*   业余都有哪些爱好？
*   你做过的哪件事最令自己感到骄傲？
*   假如你晚上要去送一个出国的同学去机场，可单位临时有事非你办不可，你怎么办？
*   就你申请的这个职位，你认为你还欠缺什么？
*   当前的offer状况；如果BATH都给了offer该如何选？
*   你对一份工作更看重哪些方面？平台，技术，氛围，城市，还是money？
*   理想薪资范围；杭州岗和北京岗选哪个？
*   理想中的工作环境是什么？
*   谈谈你对跳槽的看法
*   说说你对行业、技术发展趋势的看法
*   实习过程中周围同事/同学有哪些值得学习的地方？
*   家人对你的工作期望及自己的工作期望
*   如果你的工作出现失误，给本公司造成经济损失，你认为该怎么办？
*   若上司在公开会议上误会你了，该如何解决？
*   是否可以实习，可以实习多久？
*   在五年的时间内，你的职业规划
*   你看中公司的什么？或者公司的那些方面最吸引你？

以上题目整理来自：https://blog.csdn.net/lzw2497727771/article/details/79452842


-------
