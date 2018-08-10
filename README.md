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
### Android进程有哪些
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
