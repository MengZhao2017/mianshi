
#android

1.五种布局： FrameLayout 、 LinearLayout 、 AbsoluteLayout 、 RelativeLayout 、 TableLayout 全都继承自ViewGroup，各自特点及绘制效率对比。

FrameLayout(框架布局)

此布局是五中布局中最简单的布局，Android中并没有对child view的摆布进行控制，这个布局中所有的控件都会默认出现在视图的左上角，我们可以使用android:layout_margin，android:layout_gravity等属性去控制子控件相对布局的位置。

LinearLayout(线性布局)

一行只控制一个控件的线性布局，所以当有很多控件需要在一个界面中列出时，可以用LinearLayout布局。 此布局有一个需要格外注意的属性:android:orientation=“horizontal|vertical。

当android:orientation="horizontal时，*说明你希望将水平方向的布局交给LinearLayout *，其子元素的android:layout_gravity="right|left" 等控制水平方向的gravity值都是被忽略的，此时LinearLayout中的子元素都是默认的按照水平从左向右来排，我们可以用android:layout_gravity="top|bottom"等gravity值来控制垂直展示。
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






----------------------------------------------------------------------------------------------------------------------------------------

----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------



