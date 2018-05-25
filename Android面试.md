
Android
====
1.五种布局： FrameLayout 、 LinearLayout 、 AbsoluteLayout 、 RelativeLayout 、 TableLayout 全都继承自ViewGroup，各自特点及绘制效率对比。

FrameLayout(框架布局)

此布局是五中布局中最简单的布局，Android中并没有对child view的摆布进行控制，这个布局中所有的控件都会默认出现在视图的左上角，我们可以使用android:layout_margin，android:layout_gravity等属性去控制子控件相对布局的位置。

LinearLayout(线性布局)

一行只控制一个控件的线性布局，所以当有很多控件需要在一个界面中列出时，可以用LinearLayout布局。 此布局有一个需要格外注意的属性:android:orientation=“horizontal|vertical。
当android:orientation="horizontal时，*说明你希望将水平方向的布局交给LinearLayout *，其子元素的android:layout_gravity="right|left" 等控制水平

方向的gravity值都是被忽略的，此时LinearLayout中的子元素都是默认的按照水平从左向右来排，我们可以用android:layout_gravity="top|bottom"等gravity值来控制垂直展示。

反之，可以知道 当android:orientation="vertical时，LinearLayout对其子元素展示上的的处理方式。
AbsoluteLayout(绝对布局)

可以放置多个控件，并且可以自己定义控件的x,y位置

RelativeLayout(相对布局)

这个布局也是相对自由的布局，Android 对该布局的child view的 水平layout& 垂直layout做了解析，由此我们可以FrameLayout的基础上使用标签或者Java代码对垂直方向 以及 水平方向 布局中的views任意的控制.

相关属性：

　　android:layout_centerInParent="true|false"
　　android:layout_centerHorizontal="true|false"
　　android:layout_alignParentRight="true|false"
　　
TableLayout(表格布局)

将子元素的位置分配到行或列中，一个TableLayout由许多的TableRow组成

Activity生命周期。

启动Activity: onCreate()—>onStart()—>onResume()，Activity进入运行状态。

Activity退居后台: 当前Activity转到新的Activity界面或按Home键回到主屏： onPause()—>onStop()，进入停滞状态。

Activity返回前台: onRestart()—>onStart()—>onResume()，再次回到运行状态。

Activity退居后台，且系统内存不足， 系统会杀死这个后台状态的Activity（此时这个Activity引用仍然处在任务栈中，只是这个时候引用指向的对象已经为null），若再次回到这个Activity,则会走onCreate()–>onStart()—>onResume()(将重新走一次Activity的初始化生命周期)

锁定屏与解锁屏幕 只会调用onPause()，而不会调用onStop()方法，开屏后则调用onResume()

----------------------------------------------------------------------------------------------------------------------------------------
通过Acitivty的xml标签来改变任务栈的默认行为

使用android:launchMode="standard|singleInstance|singleTask|singleTop"来控制Acivity任务栈。

任务栈是一种后进先出的结构。位于栈顶的Activity处于焦点状态,当按下back按钮的时候,栈内的Activity会一个一个的出栈,并且调用其onDestory()方法。如果栈内没有Activity,那么系统就会回收这个栈,每个APP默认只有一个栈,以APP的包名来命名.

standard : 标准模式,每次启动Activity都会创建一个新的Activity实例,并且将其压入任务栈栈顶,而不管这个Activity是否已经存在。Activity的启动三回调(onCreate()->onStart()->onResume())都会执行。

singleTop : 栈顶复用模式.这种模式下,如果新Activity已经位于任务栈的栈顶,那么此Activity不会被重新创建,所以它的启动三回调就不会执行,同时Activity的onNewIntent()方法会被回调.如果Activity已经存在但是不在栈顶,那么作用于standard模式一样.

singleTask: 栈内复用模式.创建这样的Activity的时候,系统会先确认它所需任务栈已经创建,否则先创建任务栈.然后放入Activity,如果栈中已经有一个Activity实例,那么这个Activity就会被调到栈顶,onNewIntent(),并且singleTask会清理在当前Activity上面的所有Activity.(clear top)

singleInstance : 加强版的singleTask模式,这种模式的Activity只能单独位于一个任务栈内,由于栈内复用的特性,后续请求均不会创建新的Activity,除非这个独特的任务栈被系统销毁了

Activity的堆栈管理以ActivityRecord为单位,所有的ActivityRecord都放在一个List里面.可以认为一个ActivityRecord就是一个Activity栈

----------------------------------------------------------------------------------------------------------------------------------------

Activity缓存方法。

有a、b两个Activity，当从a进入b之后一段时间，可能系统会把a回收，这时候按back，执行的不是a的onRestart而是onCreate方法，a被重新创建一次，这是a中的临时数据和状态可能就丢失了。

可以用Activity中的onSaveInstanceState()回调方法保存临时数据和状态，这个方法一定会在活动被回收之前调用。方法中有一个Bundle参数，putString()、putInt()等方法需要传入两个参数，一个键一个值。数据保存之后会在onCreate中恢复，onCreate也有一个Bundle类型的参数。

示例代码：

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //这里，当Acivity第一次被创建的时候为空
        //所以我们需要判断一下
        if( savedInstanceState != null ){
            savedInstanceState.getString("anAnt");
        }
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);

        outState.putString("anAnt","Android");

    }

一、onSaveInstanceState (Bundle outState)

当某个activity变得“容易”被系统销毁时，该activity的onSaveInstanceState就会被执行，除非该activity是被用户主动销毁的，例如当用户按BACK键的时候。

注意上面的双引号，何为“容易”？言下之意就是该activity还没有被销毁，而仅仅是一种可能性。这种可能性有哪些？通过重写一个activity的所有生命周期的onXXX方法，包括onSaveInstanceState和onRestoreInstanceState方法，我们可以清楚地知道当某个activity（假定为activity A）显示在当前task的最上层时，其onSaveInstanceState方法会在什么时候被执行，有这么几种情况：

1、当用户按下HOME键时。

这是显而易见的，系统不知道你按下HOME后要运行多少其他的程序，自然也不知道activity A是否会被销毁，故系统会调用onSaveInstanceState，让用户有机会保存某些非永久性的数据。以下几种情况的分析都遵循该原则

2、长按HOME键，选择运行其他的程序时。

3、按下电源按键（关闭屏幕显示）时。

4、从activity A中启动一个新的activity时。

5、屏幕方向切换时，例如从竖屏切换到横屏时。（如果不指定configchange属性） 在屏幕切换之前，系统会销毁activity A，在屏幕切换之后系统又会自动地创建activity A，所以onSaveInstanceState一定会被执行

总而言之，onSaveInstanceState的调用遵循一个重要原则，即当系统“未经你许可”时销毁了你的activity，则onSaveInstanceState会被系统调用，这是系统的责任，因为它必须要提供一个机会让你保存你的数据（当然你不保存那就随便你了）。另外，需要注意的几点：

1.布局中的每一个View默认实现了onSaveInstanceState()方法，这样的话，这个UI的任何改变都会自动的存储和在activity重新创建的时候自动的恢复。但是这种情况只有在你为这个UI提供了唯一的ID之后才起作用，如果没有提供ID，将不会存储它的状态。

2.由于默认的onSaveInstanceState()方法的实现帮助UI存储它的状态，所以如果你需要覆盖这个方法去存储额外的状态信息时，你应该在执行任何代码之前都调用父类的onSaveInstanceState()方法（super.onSaveInstanceState()）。 既然有现成的可用，那么我们到底还要不要自己实现onSaveInstanceState()?这得看情况了，如果你自己的派生类中有变量影响到UI，或你程序的行为，当然就要把这个变量也保存了，那么就需要自己实现，否则就不需要。

3.由于onSaveInstanceState()方法调用的不确定性，你应该只使用这个方法去记录activity的瞬间状态（UI的状态）。不应该用这个方法去存储持久化数据。当用户离开这个activity的时候应该在onPause()方法中存储持久化数据（例如应该被存储到数据库中的数据）。

4.onSaveInstanceState()如果被调用，这个方法会在onStop()前被触发，但系统并不保证是否在onPause()之前或者之后触发。

二、onRestoreInstanceState (Bundle outState)

至于onRestoreInstanceState方法，需要注意的是，onSaveInstanceState方法和onRestoreInstanceState方法“不一定”是成对的被调用的，（本人注：我昨晚调试时就发现原来不一定成对被调用的！）

onRestoreInstanceState被调用的前提是，activity A“确实”被系统销毁了，而如果仅仅是停留在有这种可能性的情况下，则该方法不会被调用，例如，当正在显示activity A的时候，用户按下HOME键回到主界面，然后用户紧接着又返回到activity A，这种情况下activity A一般不会因为内存的原因被系统销毁，故activity A的onRestoreInstanceState方法不会被执行

另外，onRestoreInstanceState的bundle参数也会传递到onCreate方法中，你也可以选择在onCreate方法中做数据还原。 还有onRestoreInstanceState在onstart之后执行。 至于这两个函数的使用，给出示范代码（留意自定义代码在调用super的前或后）：

@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
        savedInstanceState.putBoolean("MyBoolean", true);
        savedInstanceState.putDouble("myDouble", 1.9);
        savedInstanceState.putInt("MyInt", 1);
        savedInstanceState.putString("MyString", "Welcome back to Android");
        // etc.
        super.onSaveInstanceState(savedInstanceState);
}

@Override
public void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);

        boolean myBoolean = savedInstanceState.getBoolean("MyBoolean");
        double myDouble = savedInstanceState.getDouble("myDouble");
        int myInt = savedInstanceState.getInt("MyInt");
        String myString = savedInstanceState.getString("MyString");
}


----------------------------------------------------------------------------------------------------------------------------------------

为什么在Service中创建子线程而不是Activity中

这是因为Activity很难对Thread进行控制，当Activity被销毁之后，就没有任何其它的办法可以再重新获取到之前创建的子线程的实例。而且在一个Activity中创建的子线程，另一个Activity无法对其进行操作。但是Service就不同了，所有的Activity都可以与Service进行关联，然后可以很方便地操作其中的方法，即使Activity被销毁了，之后只要重新与Service建立关联，就又能够获取到原有的Service中Binder的实例。因此，使用Service来处理后台任务，Activity就可以放心地finish，完全不需要担心无法对后台任务进行控制的情况。

----------------------------------------------------------------------------------------------------------------------------------------

Intent的使用方法，可以传递哪些数据类型。

通过查询Intent/Bundle的API文档，我们可以获知，Intent/Bundle支持传递基本类型的数据和基本类型的数组数据，以及String/CharSequence类型的数据和String/CharSequence类型的数组数据。而对于其它类型的数据貌似无能为力，其实不然，我们可以在Intent/Bundle的API中看到Intent/Bundle还可以传递Parcelable（包裹化，邮包）和Serializable（序列化）类型的数据，以及它们的数组/列表数据。

所以要让非基本类型和非String/CharSequence类型的数据通过Intent/Bundle来进行传输，我们就需要在数据类型中实现Parcelable接口或是Serializable接口。

----------------------------------------------------------------------------------------------------------------------------------------

Service的两种启动方法，有什么区别 1.在Context中通过public boolean bindService(Intent service,ServiceConnection conn,int flags) 方法来进行Service与Context的关联并启动，并且Service的生命周期依附于Context(不求同时同分同秒生！但求同时同分同秒屎！！)。

2.通过public ComponentName startService(Intent service)方法去启动一个Service，此时Service的生命周期与启动它的Context无关。

3.要注意的是，whatever，都需要在xml里注册你的Service，就像这样:

<service
        android:name=".packnameName.youServiceName"
        android:enabled="true" />
广播(Boardcast Receiver)的两种动态注册和静态注册有什么区别。

静态注册：在AndroidManifest.xml文件中进行注册，当App退出后，Receiver仍然可以接收到广播并且进行相应的处理
动态注册：在代码中动态注册，当App退出后，也就没办法再接受广播了

----------------------------------------------------------------------------------------------------------------------------------------
ContentProvider使用方法

http://blog.csdn.net/juetion/article/details/17481039

----------------------------------------------------------------------------------------------------------------------------------------

目前能否保证service不被杀死

Service设置成START_STICKY

kill 后会被重启（等待5秒左右），重传Intent，保持与重启前一样
提升service优先级

在AndroidManifest.xml文件中对于intent-filter可以通过android:priority = "1000"这个属性设置最高优先级，1000是最高值，如果数字越小则优先级越低，同时适用于广播。
【结论】目前看来，priority这个属性貌似只适用于broadcast，对于Service来说可能无效
提升service进程优先级

Android中的进程是托管的，当系统进程空间紧张的时候，会依照优先级自动进行进程的回收
当service运行在低内存的环境时，将会kill掉一些存在的进程。因此进程的优先级将会很重要，可以在startForeground()使用startForeground()将service放到前台状态。这样在低内存时被kill的几率会低一些。
【结论】如果在极度极度低内存的压力下，该service还是会被kill掉，并且不一定会restart()
onDestroy方法里重启service

service +broadcast 方式，就是当service走ondestory()的时候，发送一个自定义的广播，当收到广播的时候，重新启动service
也可以直接在onDestroy()里startService
【结论】当使用类似口口管家等第三方应用或是在setting里-应用-强制停止时，APP进程可能就直接被干掉了，onDestroy方法都进不来，所以还是无法保证
监听系统广播判断Service状态

通过系统的一些广播，比如：手机重启、界面唤醒、应用状态改变等等监听并捕获到，然后判断我们的Service是否还存活，别忘记加权限
【结论】这也能算是一种措施，不过感觉监听多了会导致Service很混乱，带来诸多不便
在JNI层,用C代码fork一个进程出来

这样产生的进程,会被系统认为是两个不同的进程.但是Android5.0之后可能不行
root之后放到system/app变成系统级应用

大招: 放一个像素在前台(手机QQ)

----------------------------------------------------------------------------------------------------------------------------------------

动画有哪两类，各有什么特点？三种动画的区别

tween 补间动画。通过指定View的初末状态和变化时间、方式，对View的内容完成一系列的图形变换来实现动画效果。 Alpha Scale Translate Rotate。

frame 帧动画 AnimationDrawable 控制 animation-list xml布局

PropertyAnimation 属性动画

----------------------------------------------------------------------------------------------------------------------------------------

Android的数据存储形式。

SQLite：SQLite是一个轻量级的数据库，支持基本的SQL语法，是常被采用的一种数据存储方式。 Android为此数据库提供了一个名为SQLiteDatabase的类，封装了一些操作数据库的api

SharedPreference： 除SQLite数据库外，另一种常用的数据存储方式，其本质就是一个xml文件，常用于存储较简单的参数设置。

File： 即常说的文件（I/O）存储方法，常用语存储大数量的数据，但是缺点是更新数据将是一件困难的事情。

ContentProvider: Android系统中能实现所有应用程序共享的一种数据存储方式，由于数据通常在各应用间的是互相私密的，所以此存储方式较少使用，但是其又是必不可少的一种存储方式。例如音频，视频，图片和通讯录，一般都可以采用此种方式进行存储。每个Content Provider都会对外提供一个公共的URI（包装成Uri对象），如果应用程序有数据需要共享时，就需要使用Content Provider为这些数据定义一个URI，然后其他的应用程序就通过Content Provider传入这个URI来对数据进行操作。

----------------------------------------------------------------------------------------------------------------------------------------

Asset目录与res目录的区别
1.res/raw中的文件会被映射到R.java文件中，访问的时候直接使用资源ID即R.id.filename；assets文件夹下的文件不会被映射到R.java中，所以读取assets目录下的文件必须指定文件的路径，访问的时候需要AssetManager类。
2.res/raw不可以有目录结构，而assets则可以有目录结构，也就是assets目录下可以再建立文件夹
相同点：两者目录下的文件在打包后会原封不动的保存在apk包中，不会被编译成二进制

----------------------------------------------------------------------------------------------------------------------------------------
View事件分发机制

Activity→Viewgroup→view

1、基础知识

(1) 所有 Touch 事件都被封装成了 MotionEvent 对象，包括 Touch 的位置、时间、历史记录以及第几个手指(多指触摸)等。

(2) 事件类型分为 ACTION_DOWN, ACTION_UP, ACTION_MOVE, ACTION_POINTER_DOWN, ACTION_POINTER_UP, ACTION_CANCEL，每个事件都是以 ACTION_DOWN 开始 ACTION_UP 结束。

(3) 对事件的处理包括三类，分别为传递——dispatchTouchEvent()函数、拦截——onInterceptTouchEvent()函数、消费——onTouchEvent()函数和 OnTouchListener

2、传递流程

(1) 事件从 Activity.dispatchTouchEvent()开始传递，只要没有被停止或拦截，从最上层的 View(ViewGroup)开始一直往下(子 View)传递。子 View 可以通过 onTouchEvent()对事件进行处理。

(2) 事件由父 View(ViewGroup)传递给子 View，ViewGroup 可以通过 onInterceptTouchEvent()对事件做拦截，停止其往下传递。

(3) 如果事件从上往下传递过程中一直没有被停止，且最底层子 View 没有消费事件，事件会反向往上传递，这时父 View(ViewGroup)可以进行消费，如果还是没有被消费的话，最后会到 Activity 的 onTouchEvent()函数。

(4) 如果 View 没有对 ACTION_DOWN 进行消费，之后的其他事件不会传递过来。

(5) OnTouchListener 优先于 onTouchEvent()对事件进行消费。
上面的消费即表示相应函数返回值为 true。

![](https://github.com/MengZhao2017/mianshi/raw/master/res/touch.png)


----------------------------------------------------------------------------------------------------------------------------------------

Activity的四种启动模式：https://blog.csdn.net/YeeCeeYee/article/details/64958184

众所周知当我们多次启动同一个Activity时，系统会创建多个实例，并把它们按照先进后出的原则一一放入任务栈中，当我们按back键时，就会有一个activity从任务栈顶移除，重复下去，直到任务栈为空，系统就会回收这个任务栈。但是这样以来，系统多次启动同一个Activity时就会重复创建多个实例，这种做法显然不合理，为了能够优化这个问题，Android提供四种启动模式来修改系统这一默认行为。 
       进入正题，Activity的四种启动模式如下： 
       standard、singleTop、singleTask、singleInstance 

standard：
这个模式是默认的启动模式，即标准模式，在不指定启动模式的前提下，系统默认使用该模式启动Activity，每次启动一个Activity都会重写创建一个新的实例，不管这个实例存不存在，这种模式下，谁启动了该模式的Activity，该Activity就属于启动它的Activity的任务栈中。这个Activity它的onCreate()，onStart()，onResume()方法都会被调用。

singleTop：栈顶复用模式
singleTop模式与standard类似，不同的是，当启动的Activity已经位于栈顶时，则直接使用它不创建新的实例。如果启动的Activity没有位于栈顶时，则创建一个新的实例位于栈顶

singleTask：栈内复用模式
如果希望Activity在整个应用程序中只存在一个实例，可以使用singleTask模式，当Activity的启动模式指定为 singleTask，每次启动该Activity时，系统首先会检查栈中是否存在该Activity的实例，如果发现已经存在则直接使用该实例，并将当前Activity之上的所有Activity出栈，如果没有发现则创建一个新的实例、

singleInstance：
在程序开发过程中，如果需要Activity在整个系统中都只有一个实例，这时就需要用到singleInstance模式。不同于以上介绍的三种模式，指定为singleInstance模式的Activity会启动一个新的任务栈来管理这个Activity。

singleInstance模式加载Activity时，无论从哪个任务栈中启动该Activity，只会创建一个Activity实例，并且会使用一个全新的任务栈来装载该Activity实例。采用这种模式启动Activity会费为一下两种情况：
第一种：如果要启动的Activity不存在，系统会创建一个新的任务栈，在创建该Activity的实例，并把该Activity加入栈顶
第二种：如果要启动的Activity已经存在，无论位于哪个应用程序或者哪个任务栈中，系统都会把该Activity所在的任务栈转到前台，从而使该Activity显示出来。

----------------------------------------------------------------------------------------------------------------------------------------
Android中图片的三级缓存
https://blog.csdn.net/sinat_31057219/article/details/71191962

为什么对图片需要三级缓存？
三级缓存策略，最实在的意义就是 减少不必要的流量消耗，增加加载速度 
android默认给每个应用只分配16M的内存，所以如果加载过多的图片，为了防止内存溢出，应该将图片缓存起来。图片的三级缓存分别是：
    
    内存缓存，优先加载，速度最快
    本地缓存，次优先加载，速度快
    网络缓存，最后加载，速度慢，浪费流量

其中，内存缓存应优先加载，它速度最快；本地缓存次优先加载，它速度也快；网络缓存不应该优先加载，它走网络，速度慢且耗流量。（总的来说由快到慢

三级缓存的原理
首次加载的时候通过网络加载，获取图片，然后保存到内存和 SD 卡中。
之后运行 APP 时，优先访问内存中的图片缓存。
如果内存没有，则加载本地 SD 卡中的图片。
具体的缓存策略可以是这样的：内存作为一级缓存，本地作为二级缓存，网络加载为最后。其中，内存使用 LruCache ，其内部通过 LinkedhashMap 来持有外界缓存对象的强引用；对于本地缓存，使用 DiskLruCache。加载图片的时候，首先使用 LRU 方式进行寻找，找不到指定内容，按照三级缓存的方式，进行本地搜索，还没有就网络加载。

----------------------------------------------------------------------------------------------------------------------------------------

Handler使用原理

handler是什么呢？

handler是Android给我们提供来更新UI的一套机制，也是一套消息处理机制，我们可以发送消息，也可以通过它来处理消息。

Android在设计的时候，就封装了一套这样消息创建 传递 处理机制，如果 不遵循这样的机制就没有办法更新UI信息，就会抛出异常。

handler的用法：

1.传递Message，用于接受子线程发送的数据，并用此数据配合主线程更新UI，有一下的方法：

        sendMessage(Message)
        
        sendMessageAtTime(Message,long)
        
        sendMessageDelayed(Message,long)
        
2.传递Runnable对象，用于通过Handler绑定的消息队列，安排不同操作的执行顺序，主要有一下方法：

        post(Runnable)
        
        postAtTime(Runnable, long)
        
        postDelayed(Runnable.long)
        
3.传递Callback对象，CallBack用于截获handler发送的消息，如果返回true。就截获成功，不会向下传递。

Android为什么要设计只能通过handler机制更新UI呢？

最根本的目的就是解决多线程并发的问题。

假设在一个Activity中，有多个线程去更新UI，并且没有枷锁机制，那么会产生什么样的问题呢？那就是更新界面混乱，如果对更新的UI的操作枷锁的话，又会导致性能下降。handler通过消息队列，保证了消息处理的先后有序性。

鉴于以上问题的考虑，Android给我们提供了一套更新UI的机制，我们只要使用一套机制就好，所有的更新UI的操作都是在主线程轮询处理。

handler原理是什么呢？

（1）Handler封装了消息的发送（主要包括消息发送给谁）

        通过Handler的构造函数，绑定一个loop对象，loop.looper()。
        
        Looper:
        
        (a)内部包含一个消息队列，也就是MessageQueue，所有Handler发送的消息都走向这个队列。
        
        (b)Looper.loop()方法就是一个for死循环，不断的从MessageQueue取消息，如果有消息就处理消息，没有消息就阻塞消息
        
（2）MessageQueue，就是一个消息队列，可以添加消息，处理消息。

（3）Handler也不难，比较简单，在构造Handler的时候，内部会跟Looper进行关联，通过Looper.mylooper(）获取到Looper，找到了Looper也就找到MessageQueue，在Handler中发送消息，其实就是向MessageQueue队列中发送消息。

总结：

Handler负责发送消息，Looper负责接收handler发送的消息，并直接把消息回传给自己，MessageQueue就是一个存储消息的容器。
一个线程中只有一个Looper实例，一个MessageQueue实例，可以有多个Handler实例。


----------------------------------------------------------------------------------------------------------------------------------------

 请描述一下Activity 生命周期
 
七个周期函数，按顺序分别是: onCreate(), onStart(), onRestart(), onResume(), onPause(),onStop(), onDestroy()。

onCreate(): 创建Activity时调用，设置在该方法中，还以Bundle的形式提供对以前存储的任何状态的访问。

onStart(): Activity变为在屏幕上对用户可见时调用。

onResume(): Activity开始与用户交互时调用(无论是启动还是重新启动一个活动，该方法总是被调用。

onPause(): Activity被暂停或收回cpu和其他资源时调用，该方法用户保护活动状态的，也是保护现场。

onStop(): Activity被停止并转为不可见阶段及后续的生命周期事件时调用。

onRestart(): Activity被重新启动时调用。该活动仍然在栈中，而不是启动新的Activity。

onDestroy():Activity被销毁时调用。

1、完整生命周期: 即从一个Activity从出现到消失，对应的周期方法是从onCreate()到onDestroy()。

2、可见生命周期: 当Activity处于可以用户看见的状态，但不一定能与用户交互时，将多次执行从onStart()到onStop()。

3、前景生命周期: 当Activity处于Activity栈最顶端，能够与其他用户进行交互时，将多次执行从onResume()到onPause()。
 
----------------------------------------------------------------------------------------------------------------------------------------

两个Activity之间跳转时必然会执行的是哪几个方法

答: 两个Activity之间跳转必然会执行的是下面几个方法。

onCreate()//在Activity生命周期开始时调用。

onRestoreInstanceState()//用来恢复UI状态。

onRestart()//当Activity重新启动时调用。

onStart()//当Activity对用户即将可见时调用。

onResume()//当Activity与用户交互时，绘制界面。

onSaveInstanceState()//即将移出栈顶保留UI状态时调用。

onPause()//暂停当前活动Activity，提交持久数据的改变，停止动画或其他占用GPU资源的东西，由于下一个Activity在这个方法返回之前不会resume，所以这个方法的代码执行要快。

onStop()//Activity不再可见时调用。

onDestroy()//Activity销毁栈时被调用的最后一个方法。

----------------------------------------------------------------------------------------------------------------------------------------

service介绍

1.Android Service是运行在后台的代码，不能与用户交互，可以运行在自己的进程，也可以运行在其他应用程序进程的上下文里。

2.service启动方式：Context.startService()和Context.bindService()

使用startService()启动service的时候，访问者与service没有关联，即访问者退出，service依然在运行。而使用bindService()的时候，访问者与service是关联绑定在一起的，访问者一旦退出，service就停止运行。

3.service生命周期：

Android Service只继承了onCreate(), onStart(),onDestroy()三个方法，当我们第一次启动Service时，先后调用onCreate(), onStart()这两个方法，当停止Service时，则执行onDestroy()方法时。如果Service已经启动了，当我们再次启动Service时，不会再执行onCreate()方法，而是直接执行onStart()方法。

4.service的使用：

第一种startServic()

  Intent intent =new Intent(this,Service1.class);//Service1是我们创建好的service类，通过这句，来启动对应的service
  startService（intent）;
  
第二种bindService()

  Intent intent =new Intent(this,Service1.class);//Service1是我们创建好的service类，通过这句，来启动对应的service
  bindService（intent）;
  
    1、activity能进行绑定得益于Serviece的接口。为了支持Service的绑定，实现onBind方法。

    2、Service和Activity的连接可以用ServiceConnection来实现。需要实现一个新的ServiceConnection，重现onServiceConnected和OnServiceDisconnected方法，一旦连接建立，就能得到Service实例的引用。

    3、执行绑定，调用bindService方法，传入一个选择了要绑定的Service的Intent(显示或隐式)和一个你实现了的ServiceConnection的实例
 
5.service 通信

如果service和访问者之间需要进行通信，应该调用bindService()绑定 service与访问者，通信结束后，再调用unbindservice()解除绑定，退出service。

绑定service之后，service类中的IBinder onbind( Intent intent)方法的返回值，将传递给在访问者类中声明的serviceConnection的onServiceConnected(ComponentName name,IBinder service)方法中的参数，这样访问者就可以通过Inbind对象，实现与service之间的通信了。

----------------------------------------------------------------------------------------------------------------------------------------

AIDL跨进程通信  https://blog.csdn.net/u011974987/article/details/51243539

AIDL是一种接口定义语言

1.首先，我们就在AS里面新建一个aidl文件(ps:现在AS建aidl不要求和java包名相同了)：

    package aidl;
    interface IMyInterface {
        String getInfor(String s);
    }

2.接着你sync project一下就可以在app/generated/source/aidl/debug/aidl里面发现由aidl文件生成的java文件了

3.在service中实现，然后就看看Service:

    public class MyService extends Service { 

    public final static String TAG = "MyService";

    private IBinder binder = new IMyInterface.Stub() {
            @Override       
            public String getInfor(String s) throws RemoteException { 
                Log.i(TAG, s); 
                return "我是 Service 返回的字符串"; 
           }
        };
    @Overrid
    public void onCreate() {
        super.onCreate(); 
        Log.i(TAG, "onCreat");    
    }       
    @Override    
    public IBinder onBind(Intent intent) { 
        return binder;  
        }
    }

这里我们写了一个Service，看一下也比较简单。先new了一IMyInterface.Stub()并把它向上转型成了IBinder，最后在onBind方法中返回回去。可能你注意到了，在IMyInterface.Stub()的内部我们重写getInfor(String s) 方法，没错这就是我们 aidl文件中定义的接口。 

4.实现activity也就是访问者

        public class MainActivity extends AppCompatActivity {
            public final static String TAG = "MainActivity";
            private IMyInterface myInterface;
            private ServiceConnection serviceConnection = new ServiceConnection() {
                @Override
                public void onServiceConnected(ComponentName name, IBinder service) {
                    myInterface = IMyInterface.Stub.asInterface(service);
                    Log.i(TAG, "连接Service 成功");
                    try {
                        String s = myInterface.getInfor("我是Activity传来的字符串");
                        Log.i(TAG, "从Service得到的字符串：" + s);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                }
                @Override
                public void onServiceDisconnected(ComponentName name) {
                    Log.e(TAG, "连接Service失败");
                }
            };
            @Override
            protected void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.activity_main);
                startAndBindService();
            }
            private void startAndBindService() {
                Intent service = new Intent(MainActivity.this, MyService.class);
                //startService(service);
                bindService(service, serviceConnection, Context.BIND_AUTO_CREATE);
            }
        }

  上面的过程就实现了service和activity之间的通信
  
AIDL的理解：
  
  1.Service中的IBinder 
还记得我们在MyService中利用new IMyInterface.Stub()向上转型成了IBinder然后在onBind方法中返回的。那我们就看看IMyInterface.Stub吧：

    public static abstract class Stub extends android.os.Binder implements aidl.IMyInterface {
    ..........
    }

可以看到，Stub是IMyInterface中的一个静态抽象类，继承了Binder，并且实现了IMyInterface接口。这也就解释了我们定义IMyInterface.Stub的时候为什么需要实现IMyInterface中的方法了，也说明了为什么我们可以把IMyInterface.Stub向上转型成IBinder了。
  
  2.Activity中的IMyInterface 
在Activity中，通过ServiceConnection连接MyService并成功回调onServiceConnected中我们把传回来的IBinder通IMyInterface.Stub.asInterface(service)转换成为IMyInterface
  
----------------------------------------------------------------------------------------------------------------------------------------
请描述一下Intent 和 Intent Filter。

Intent在Android中被翻译为”意图”，他是三种应用程序基本组件-Activity，Service和broadcast receiver之间相互激活的手段。

在调用Intent名称时使用ComponentName也就是类的全名时为显示调用。这种方式一般用于应用程序的内部调用，因为你不一定会知道别人写的类的全名。而Intent Filter是指意图过滤，不出现在代码中，而是出现在android Manifest文件中，以<intent-filter>的形式。（有一个例外是broadcast receiver的intent
filter是使用Context.registerReceiver()来动态设定的，其中intent filter也是在代码中创建的）

一个intent有action，data，category等字段。一个隐式intent为了能够被某个intent filter接收，必须通过3个测试，一个intent为了被某个组件接收，则必须通过它所有的intent filter中的一个。

----------------------------------------------------------------------------------------------------------------------------------------

 请描述一下BroadcastReceiver。

Broadcast Receiver用于接收并处理广播通知(broadcast announcements)。多数的广播是系统发起的，如地域变换、电量不足、来电短信等。程序也可以播放一个广播。程序可以有任意数量的broadcast receivers来响应它觉得重要的通知。Broadcast receiver可以通过多种方式通知用户: 启动activity、使用NotificationManager、开启背景灯、振动设备、播放声音等，最典型的是在状态栏显示一个图标，这样用户就可以点它打开看通知内容。通常我们的某个应用或系统本身在某些事件(电池电量不足、来电短信)来临时会广播一个Intent出去，我们利用注册一个broadcast
receiver来监听这些Intent并获取Intent中的数据。

----------------------------------------------------------------------------------------------------------------------------------------

 在manifest和代码中如何注册和使用 broadcast receiver 。

答: 在android的manifest中注册

 
    <receiver android: name =“Receiver1”>

       <intent-filter>

         <!----和Intent中的action对应--->

         <actionandroid: name=“com.forrest.action.mybroadcast”/>

     </intent-filter>

    </receiver>
在代码中注册

1、 IntentFilter filter = new IntentFilter(“com.forrest.action.mybroadcast”);//和广播中Intent的action对应;

2、 MyBroadcastReceiver br= new MyBroadcastReceiver();

3、 registerReceiver(br, filter);

----------------------------------------------------------------------------------------------------------------------------------------

请介绍下Android的数据存储方式。

Android提供了5中存储数据的方式，分别是以下几种

1、使用Shared Preferences存储数据，用来存储key-value，pairs格式的数据，它是一个轻量级的键值存储机制，只可以存储基本数据类型。

2、使用文件存储数据，通过FileInputStream和FileOutputStream对文件进行操作。在Android中，文件是一个应用程序私有的，一个应用程序无法读写其他应用程序的文件。

3、使用SQLite数据库存储数据，Android提供的一个标准数据库，支持SQL语句。

4、使用Content Provider存储数据，是所有应用程序之间数据存储和检索的一个桥梁，它的作用就是使得各个应用程序之间实现数据共享。它是一个特殊的存储数据的类型，它提供了一套标准的接口用来获取数据，操作数据。系统也提供了音频、视频、图像和个人信息等几个常用的Content Provider。如果你想公开自己的私有数据，可以创建自己的Content Provider类，或者当你对这些数据拥有控制写入的权限时，将这些数据添加到Content Provider中实现共享。外部访问通过Content Resolver去访问并操作这些被暴露的数据。

5、使用网络存储数据

----------------------------------------------------------------------------------------------------------------------------------------

ListView如何提高其效率？

1.使用分页加载，不要一次性加载所有的数据。

2.优化加载布局：复用convertview，在getItem()中判断convertview是否为空，不为空就复用。即判断是否加载了布局，不用每次都要加载布局文件

3.优化加载空间：ViewHoder的使用，会将控件的实例存放在viewholder里，然后调用settag()，将viewholder对象存储到view里。

----------------------------------------------------------------------------------------------------------------------------------------

Android系统架构

Linux内核层→运行库→应用程序框架层→应用程序层
![](https://github.com/MengZhao2017/mianshi/raw/master/res/framework.png)

----------------------------------------------------------------------------------------------------------------------------------------

Android中 内存泄漏 https://www.jianshu.com/p/bf159a9c391a

https://www.jianshu.com/p/ac00e370f83d?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io

1.Static Activities

在类中定义了静态Activity变量，把当前运行的Activity实例赋值于这个静态变量。
如果这个静态变量在Activity生命周期结束后没有清空，就导致内存泄漏。因为static变量是贯穿这个应用的生命周期的，所以被泄漏的Activity就会一直存在于应用的进程中，不会被垃圾回收器回收。

2.Inner Classes

假设Activity中有个内部类，这样做可以提高可读性和封装性。将如我们创建一个内部类，而且持有一个静态变量的引用，恭喜，内存泄漏就离你不远了。
内部类的优势之一就是可以访问外部类，不幸的是，导致内存泄漏的原因，就是内部类持有外部类实例的强引用。

3.Handler

同样道理，定义匿名的Runnable，用匿名类Handler执行。Runnable内部类会持有外部类的隐式引用，被传递到Handler的消息队列MessageQueue中，在Message消息没有被处理之前，Activity实例不会被销毁了，于是导致内存泄漏。

4.Threads

只要是匿名类的实例，不管是不是在工作线程，都会持有Activity的引用，导致内存泄漏。

5.Sensor Manager

最后，通过Context.getSystemService(int name)可以获取系统服务。这些服务工作在各自的进程中，帮助应用处理后台任务，处理硬件交互。如果需要使用这些服务，可以注册监听器，这会导致服务持有了Context的引用，如果在Activity销毁的时候没有注销这些监听器，会导致内存泄漏

6.没有及时释放资源。

----------------------------------------------------------------------------------------------------------------------------------------

Recyclerview介绍

使用LayoutManager来确定每一个Item的排列方式，为增加和删除提供了默认的动画效果。

LayoutManager：LinearLayoutManager，GridLayoutManager，StageredGridLayoutManager.

与Listview的区别：
1.自带ItemAnimation的效果
2.使用LayoutManager来管理每一个Item
3.封装了ViewHolder的回收利用


----------------------------------------------------------------------------------------------------------------------------------------

Android中进程分类：

前台进程、可视进程、服务进程、后台进程

----------------------------------------------------------------------------------------------------------------------------------------

Android中几种动画

补间动画：rotate，scale，translate

帧动画：连续的GIF效果

属性动画：不停改变对象的属性

----------------------------------------------------------------------------------------------------------------------------------------

httpclient和httpURLconnection

HttpClient

DefaultHttpClient和它的兄弟AndroidHttpClient都是HttpClient具体的实现类，它们都拥有众多的API，而且实现比较稳定，bug数量也很少。
但同时也由于HttpClient的API数量过多，使得我们很难在不破坏兼容性的情况下对它进行升级和扩展，所以目前Android团队在提升和优化HttpClient方面的工作态度并不积极。

简单来说，用HttpClient发送请求、接收响应都很简单，只需要五大步骤即可：（要牢记）

1、创建代表客户端的HttpClient对象。

2、创建代表请求的对象，如果需要发送GET请求，则创建HttpGet对象，如果需要发送POST请求，则创建HttpPost对象。注：对于发送请求的参数，GET和POST使用的方式不同，GET方式可以使用拼接字符串的方式，把参数拼接在URL结尾；POST方式需要使用setEntity(HttpEntity entity)方法来设置请求参数。

3、调用HttpClient对象的execute（HttpUriRequest request）发送请求，执行该方法后，将获得服务器返回的HttpResponse对象。服务器发还给我们的数据就在这个HttpResponse相应当中。调用HttpResponse的对应方法获取服务器的响应头、响应内容等。

4、检查相应状态是否正常。服务器发给客户端的相应，有一个相应码：相应码为200，正常；相应码为404，客户端错误；相应码为505，服务器端错误。

5、获得相应对象当中的数据

HttpURLconnection的使用：

创建一个URL对象： URL url = new URL(http://www.baidu.com);

调用URL对象的openConnection( )来获取HttpURLConnection对象实例： HttpURLConnection conn = (HttpURLConnection) url.openConnection();

设置HTTP请求使用的方法:GET或者POST，或者其他请求方式比如：PUT conn.setRequestMethod("GET");

设置连接超时，读取超时的毫秒数，以及服务器希望得到的一些消息头 conn.setConnectTimeout(6*1000); conn.setReadTimeout(6 * 1000);

调用getInputStream()方法获得服务器返回的输入流，然后输入流进行读取了 InputStream in = conn.getInputStream();

最后调用disconnect()方法将HTTP连接关掉 conn.disconnect();

----------------------------------------------------------------------------------------------------------------------------------------

#### Android中MVC框架   https://blog.csdn.net/feiduclear_up/article/details/46363207

MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。其中M层处理数据，业务逻辑等；V层处理界面的显示结果；C层起到桥梁的作用，来控制V层和M层通信以此来达到分离视图显示和业务逻辑层。说了这么多，听着感觉很抽象，废话不多说，我们来看看MVC在Android开发中是怎么应用的吧

![](https://github.com/MengZhao2017/mianshi/raw/master/res/mvc1.png)

MVC for Android

在Android开发中，比较流行的开发框架模式采用的是MVC框架模式，采用MVC模式的好处是便于UI界面部分的显示和业务逻辑，数据处理分开。那么Android项目中哪些代码来充当M,V,C角色呢？

M层：适合做一些业务逻辑处理，比如数据库存取操作，网络操作，复杂的算法，耗时的任务等都在model层处理。

V层：应用层中处理数据显示的部分，XML布局可以视为V层，显示Model层的数据结果。

C层：在Android中，Activity处理用户交互问题，因此可以认为Activity是控制器，Activity读取V视图层的数据（eg.读取当前EditText控件的数据），控制用户输入（eg.EditText控件数据的输入），并向Model发送数据请求（eg.发起网络请求等）。

MVC使用总结

利用MVC设计模式，使得这个天气预报小项目有了很好的可扩展和维护性，当需要改变UI显示的时候，无需修改Contronller（控制器）Activity的代码和Model（模型）WeatherModel模型中的业务逻辑代码，很好的将业务逻辑和界面显示分离。

在Android项目中，业务逻辑，数据处理等担任了Model（模型）角色，XML界面显示等担任了View（视图）角色，Activity担任了Contronller（控制器）角色。contronller（控制器）是一个中间桥梁的作用，通过接口通信来协同 View（视图）和Model（模型）工作，起到了两者之间的通信作用。

什么时候适合使用MVC设计模式？当然一个小的项目且无需频繁修改需求就不用MVC框架来设计了，那样反而觉得代码过度设计，代码臃肿。一般在大的项目中，且业务逻辑处理复杂，页面显示比较多，需要模块化设计的项目使用MVC就有足够的优势了。

4.在MVC模式中我们发现，其实控制器Activity主要是起到解耦作用，将View视图和Model模型分离，虽然Activity起到交互作用，但是找Activity中有很多关于视图UI的显示代码，因此View视图和Activity控制器并不是完全分离的，也就是说一部分View视图和Contronller控制器Activity是绑定在一个类中的。

MVC的优点：

(1)耦合性低。所谓耦合性就是模块代码之间的关联程度。利用MVC框架使得View（视图）层和Model（模型）层可以很好的分离，这样就达到了解耦的目的，所以耦合性低，减少模块代码之间的相互影响。

(2)可扩展性好。由于耦合性低，添加需求，扩展代码就可以减少修改之前的代码，降低bug的出现率。

(3)模块职责划分明确。主要划分层M,V,C三个模块，利于代码的维护。

----------------------------------------------------------------------------------------------------------------------------------------

#### Android中的MVP  https://blog.csdn.net/jdsjlzx/article/details/51174396

MVC还有一个重要的缺陷，大家看上面那幅图，view层和model层是相互可知的，这意味着两层之间存在耦合，耦合对于一个大型程序来说是非常致命的，因为这表示开发，测试，维护都需要花大量的精力。

MVP作为MVC的演化，解决了MVC不少的缺点，对于Android来说，MVP的model层相对于MVC是一样的，而activity和fragment不再是controller层，而是纯粹的view层，所有关于用户事件的转发全部交由presenter层处理。下面还是让我们看图

![](https://github.com/MengZhao2017/mianshi/raw/master/res/mvp.png)

从图中就可以看出，最明显的差别就是view层和model层不再相互可知，完全的解耦，取而代之的presenter层充当了桥梁的作用，用于操作view层发出的事件传递到presenter层中，presenter层去操作model层，并且将数据返回给view层，整个过程中view层和model层完全没有联系。看到这里大家可能会问，虽然view层和model层解耦了，但是view层和presenter层不是耦合在一起了吗？其实不是的，对于view层和presenter层的通信，我们是可以通过接口实现的，具体的意思就是说我们的activity，fragment可以去实现实现定义好的接口，而在对应的presenter中通过接口调用方法。不仅如此，我们还可以编写测试用的View，模拟用户的各种操作，从而实现对Presenter的测试。这就解决了MVC模式中测试，维护难的问题。

----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------




----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------





