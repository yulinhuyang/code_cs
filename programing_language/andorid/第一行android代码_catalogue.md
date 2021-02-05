**第1章 开始启程——你的第一行Android代码**　 

1.1　了解全貌，Android王国简介　 

1.1.1　Android系统架构　 

1.1.2　Android已发布的版本　 

1.1.3　Android应用开发特色　 

1.2　手把手带你搭建开发环境　 

1.2.1　准备所需要的工具　 

1.2.2　搭建开发环境　 

1.3　创建你的第 一个Android项目　 

1.3.1　创建HelloWorld项目　 

1.3.2　启动模拟器　 

1.3.3　运行HelloWorld　 

1.3.4　分析你的第 一个Android程序　 

1.3.5　详解项目中的资源　 

1.3.6　详解build.gradle文件　 

1.4　前行必备：掌握日志工具的使用　 

1.4.1　使用Android的日志工具Log　 

1.4.2　为什么使用Log而不使用println()　 

1.5　小结与点评　 

**第2章 探究新语言，快速入门Kotlin编程**　 

2.1　Kotlin语言简介　 

2.2　如何运行Kotlin代码　 

2.3　编程之本：变量和函数　 

2.3.1　变量　 

2.3.2　函数　 

2.4　程序的逻辑控制　 

2.4.1　if条件语句　 

2.4.2　when条件语句　 

2.4.3　循环语句　 

2.5　面向对象编程　 

2.5.1　类与对象　 

2.5.2　继承与构造函数　 

2.5.3　接口　 

2.5.4　数据类与单例类　 

2.6　Lambda编程 

2.6.1　集合的创建与遍历　 

2.6.2　集合的函数式API　 

2.6.3　Java函数式API的使用　 

2.7　空指针检查　 

2.7.1　可空类型系统　 

2.7.2　判空辅助工具　 

2.8　Kotlin中的小魔术　 

2.8.1　字符串内嵌表达式　 

2.8.2　函数的参数默认值　 

2.9　小结与点评　 

**第3章　先从看得到的入手，探究Activity**　 

3.1　Activity是什么　 

3.2　Activity的基本用法　 

3.2.1　手动创建Activity　 

3.2.2　创建和加载布局　 

3.2.3　在AndroidManifest文件中注册　 

3.2.4　在Activity中使用Toast　 

3.2.5　在Activity中使用Menu　 

3.2.6　销毁一个Activity　 

3.3　使用Intent在Activity之间穿梭　 

3.3.1　使用显式Intent　 

3.3.2　使用隐式Intent　 

3.3.3　更多隐式Intent的用法　

3.3.4　向下一个Activity传递数据　

3.3.5　返回数据给上一个Activity　

3.4　Activity的生命周期　

3.4.1　返回栈　

3.4.2　Activity状态　

3.4.3　Activity的生存期　

3.4.4　体验Activity的生命周期　

3.4.5　Activity被回收了怎么办　

3.5　Activity的启动模式　

3.5.1　standard　

3.5.2　singleTop　

3.5.3　singleTask　

3.5.4　singleInstance　

3.6　Activity的最佳实践　

3.6.1　知晓当前是在哪一个Activity　

3.6.2　随时随地退出程序　

3.6.3　启动Activity的最佳写法　

3.7　Kotlin课堂：标准函数和静态方法　

3.7.1　标准函数with、run和apply　

3.7.2　定义静态方法　

3.8　小结与点评　

**第4章　软件也要拼脸蛋，UI开发的点点滴滴**　

4.1　该如何编写程序界面　

4.2　常用控件的使用方法　

4.2.1　TextView　

4.2.2　Button　

4.2.3　EditText　

4.2.4　ImageView　

4.2.5　ProgressBar　

4.2.6　AlertDialog　

4.3　详解3种基本布局　

4.3.1　LinearLayout　

4.3.2　RelativeLayout　

4.3.3　FrameLayout　

4.4　系统控件不够用？创建自定义控件　

4.4.1　引入布局　

4.4.2　创建自定义控件　

4.5　最常用和最难用的控件：ListView　

4.5.1　ListView的简单用法　

4.5.2　定制ListView的界面　

4.5.3　提升ListView的运行效率　

4.5.4　ListView的点击事件　

4.6　更强大的滚动控件：RecyclerView　

4.6.1　RecyclerView的基本用法　

4.6.2　实现横向滚动和瀑布流布局　

4.6.3　RecyclerView的点击事件　

4.7　编写界面的最佳实践　

4.7.1　制作9-Patch图片　

4.7.2　编写精美的聊天界面　

4.8　Kotlin课堂：延迟初始化和密封类　

4.8.1　对变量延迟初始化　

4.8.2　使用密封类优化代码　

4.9　小结与点评　

**第5章　手机平板要兼顾，探究Fragment**　

5.1　Fragment是什么　

5.2　Fragment的使用方式　

5.2.1　Fragment的简单用法　

5.2.2　动态添加Fragment　

5.2.3　在Fragment中实现返回栈　

5.2.4　Fragment和Activity之间的交互　

5.3　Fragment的生命周期　

5.3.1　Fragment的状态和回调　

5.3.2　体验Fragment的生命周期　

5.4　动态加载布局的技巧　

5.4.1　使用限定符　

5.4.2　使用最小宽度限定符　

5.5　Fragment的最佳实践：一个简易版的新闻应用　

5.6　Kotlin课堂：扩展函数和运算符重载　

5.6.1　大有用途的扩展函数　

5.6.2　有趣的运算符重载　

5.7　小结与点评　

**第6章　全局大喇叭，详解广播机制**　

6.1　广播机制简介　

6.2　接收系统广播　

6.2.1　动态注册监听时间变化　

6.2.2　静态注册实现开机启动　

6.3　发送自定义广播　

6.3.1　发送标准广播　

6.3.2　发送有序广播　

6.4　广播的最佳实践：实现强制下线功能　

6.5　Kotlin课堂：高阶函数详解　

6.5.1　定义高阶函数　

6.5.2　内联函数的作用　

6.5.3　noinline与crossinline　

6.6　Git时间：初识版本控制工具　

6.6.1　安装Git　

6.6.2　创建代码仓库　

6.6.3　提交本地代码　

6.7　小结与点评　

**第7章　数据存储全方案，详解持久化技术**　

7.1　持久化技术简介　

7.2　文件存储　

7.2.1　将数据存储到文件中　

7.2.2　从文件中读取数据　

7.3　SharedPreferences存储　

7.3.1　将数据存储到SharedPreferences中　

7.3.2　从SharedPreferences中读取数据　

7.3.3　实现记住密码功能　

7.4　SQLite数据库存储　

7.4.1　创建数据库　

7.4.2　升级数据库　

7.4.3　添加数据　

7.4.4　更新数据　

7.4.5　删除数据　

7.4.6　查询数据　

7.4.7　使用SQL操作数据库　

7.5　SQLite数据库的最佳实践　

7.5.1　使用事务　

7.5.2　升级数据库的最佳写法　

7.6　Kotlin课堂：高阶函数的应用　

7.6.1　简化SharedPreferences的用法　

7.6.2　简化ContentValues的用法　

7.7　小结与点评　

**第8章　跨程序共享数据，探究ContentProvider**　

8.1　ContentProvider简介　

8.2　运行时权限　

8.2.1　Android权限机制详解　

8.2.2　在程序运行时申请权限　

8.3　访问其他程序中的数据　

8.3.1　ContentResolver的基本用法　

8.3.2　读取系统联系人　

8.4　创建自己的ContentProvider　

8.4.1　创建ContentProvider的步骤　

8.4.2　实现跨程序数据共享　

8.5　Kotlin课堂：泛型和委托　

8.5.1　泛型的基本用法　

8.5.2　类委托和委托属性　

8.5.3　实现一个自己的lazy函数　

8.6　小结与点评　

**第9章　丰富你的程序，运用手机多媒体**　

9.1　将程序运行到手机上　

9.2　使用通知　

9.2.1　创建通知渠道　

9.2.2　通知的基本用法　

9.2.3　通知的进阶技巧　

9.3　调用摄像头和相册　

9.3.1　调用摄像头拍照　

9.3.2　从相册中选择图片　

9.4　播放多媒体文件　

9.4.1　播放音频　

9.4.2　播放视频　

9.5　Kotlin课堂：使用infix函数构建更可读的语法　

9.6　Git时间：版本控制工具进阶　

9.6.1　忽略文件　

9.6.2　查看修改内容　

9.6.3　撤销未提交的修改　

9.6.4　查看提交记录　

9.7　小结与点评　

**第10章 后台默默的劳动者，探究Service**　

10.1　Service是什么　

10.2　Android多线程编程　

10.2.1　线程的基本用法　

10.2.2　在子线程中更新UI　

10.2.3　解析异步消息处理机制　

10.2.4　使用AsyncTask　

10.3　Service的基本用法　

10.3.1　定义一个Service　

10.3.2　启动和停止Service　

10.3.3　Activity和Service进行通信　

10.4　Service的生命周期　

10.5　Service的更多技巧　

10.5.1　使用前台Service　

10.5.2　使用IntentService　

10.6　Kotlin课堂：泛型的高级特性　

10.6.1　对泛型进行实化　

10.6.2　泛型实化的应用　

10.6.3　泛型的协变　

10.6.3　泛型的逆变　

10.7　小结与点评　

**第11章 看看精彩的世界，使用网络技术**　

11.1　WebView的用法　

11.2　使用HTTP访问网络　

11.2.1　使用HttpURLConnection　

11.2.2　使用OkHttp　

11.3　解析XML格式数据　

11.3.1　Pull解析方式　

11.3.2　SAX解析方式　

11.4　解析JSON格式数据　

11.4.1　使用JSONObject　

11.4.2　使用GSON　

11.5　网络请求回调的实现方式　

11.6　最好用的网络库：Retrofit　

11.6.1　Retrofit的基本用法　

11.6.2　处理复杂的接口地址类型　

11.6.3　Retrofit构建器的最佳写法　

11.7　Kotlin课堂：使用协程编写高效的并发程序　

11.7.1　协程的基本用法　

11.7.2　更多的作用域构建器　

11.7.3　使用协程简化回调的写法　

11.8　小结与点评　

**第12章 最佳的UI体验，MaterialDesign实战**　

12.1　什么是Material Design　

12.2　Toolbar　

12.3　滑动菜单　

12.3.1　DrawerLayout　

12.3.2　NavigationView　

12.4　悬浮按钮和可交互提示　

12.4.1　FloatingActionButton　

12.4.2　Snackbar　

12.4.3　CoordinatorLayout　

12.5　卡片式布局　

12.5.1　MaterialCardView　

12.5.2　AppBarLayout　

12.6　下拉刷新　

12.7　可折叠式标题栏　

12.7.1　CollapsingToolbarLayout　

12.7.2　充分利用系统状态栏空间　

12.8　Kotlin课堂：编写好用的工具方法　

12.8.1　求N个数的最大最小值　

12.8.2　简化Toast的用法　

12.8.3　简化Snackbar的用法　

12.9　Git时间：版本控制工具的高级用法　

12.9.1　分支的用法　

12.9.2　与远程版本库协作　

12.10　小结与点评　

**第13章 高级程序开发组件，探究Jetpack**　

13.1　Jetpack简介　

13.2　ViewModel　

13.2.1　ViewModel的基本用法　

13.2.2　向ViewModel传递参数　

13.3　Lifecycles　

13.4　LiveData　

13.4.1　LiveData的基本用法　

13.4.2　map和switchMap　

13.5　Room　

13.5.1　使用Room进行増删改查　

13.5.2　Room的数据库升级　

13.6　WorkManager　

13.6.1　WorkManager的基本用法　

13.6.2　使用WorkManager处理复杂的任务　

13.7　Kotlin课堂：使用DSL构建专有的语法结构　

13.8　小结与点评　

**第14章 继续进阶，你还应该掌握的高级技巧**　

14.1　全局获取Context的技巧　

14.2　使用Intent传递对象　

14.2.1　Serializable方式　

14.2.2　Parcelable方式　

14.3　定制自己的日志工具　

14.4　调试Android程序　

14.5　深色主题　

14.6　Kotlin课堂：Java与Kotlin代码之间的转换　

14.7　总结　

**第15章 进入实战，开发一个天气预报App**　

15.1　功能需求及技术可行性分析　

15.2　Git时间：将代码托管到GitHub上　

15.3　搭建MVVM项目架构　

15.4　搜索全球城市数据　

15.4.1　实现逻辑层代码　

15.4.2　实现UI层代码　

15.5　显示天气信息　

15.5.1　实现逻辑层代码　

15.5.2　实现UI层代码　

15.5.3　记录选中的城市　

15.6　手动刷新天气和切换城市　

15.6.1　手动刷新天气　

15.6.2　切换城市　

15.7　制作App的图标　

15.8　生成正式签名的APK文件　

15.8.1　使用Android Studio生成　

15.8.2　使用Gradle生成　

15.9　你还可以做的事情　

**第16章 编写并发布一个开源库，PermissionX**　

16.1　开发前的准备工作　

16.2　实现PermissionX开源库　

16.3　对开源库进行测试　

16.4　将开源库发布到jcenter仓库　

16.5　体验我们的成果　

16.6　结束语　
