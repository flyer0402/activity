activity
========

activity
Activity分享
1、	Activity的管理

//http://www.cnblogs.com/franksunny/archive/2012/04/17/2453403.html
//http://blog.chinaunix.net/uid-13164110-id-2936963.html
//http://book.51cto.com/art/201207/348730.htm
// http://blog.csdn.net/yueliangniao1/article/details/7227165
Activity、Task、应用和进程:
ActivityManagerServiceActivityStackActivityRecordTaskRecord
ActivityStack 存放和管理ActivityRecord
ActivityRecord每个Activity对应一个ActivityRecord
Aplication对应于一个应用
Task用户角度的概念 对应于TaskRecord
进程对应与 ProcessRecord

2、	启动
//http://blog.csdn.net/luoshengyang/article/details/6685853

 Step 1. 无论是通过Launcher来启动Activity，还是通过Activity内部调用startActivity接口来启动新的 Activity，都通过Binder进程间通信进入到ActivityManagerService进程中，并且调用 ActivityManagerService.startActivity接口； 

Step 2. ActivityManagerService调用ActivityStack.startActivityMayWait来做准备要启动的Activity的相关信息；

 Step 3. ActivityStack通知ApplicationThread要进行Activity启动调度了，这里的ApplicationThread 代表的是调用ActivityManagerService.startActivity接口的进程，对于通过点击应用程序图标的情景来说，这个进程就是 Launcher了，而对于通过在Activity内部调用startActivity的情景来说，这个进程就是这个Activity所在的进程了；

 Step 4. ApplicationThread不执行真正的启动操作，它通过调用 ActivityManagerService.activityPaused接口进入到ActivityManagerService进程中，看看是否 需要创建新的进程来启动Activity；

 Step 5. 对于通过点击应用程序图标来启动Activity的情景来说，ActivityManagerService在这一步中，会调用 startProcessLocked来创建一个新的进程，而对于通过在Activity内部调用startActivity来启动新的Activity 来说，这一步是不需要执行的，因为新的Activity就在原来的Activity所在的进程中进行启动；

Step 6. ActivityManagerServic调用ApplicationThread.scheduleLaunchActivity接口，通知相应的进程执行启动Activity的操作；
       
 Step 7. ApplicationThread把这个启动Activity的操作转发给ActivityThread，ActivityThread通过ClassLoader导入相应的Activity类，然后把它启动起来。

3、	Activity缓存机制
Listview 中的convertView，主要用到的是一个Recycler将没有正在显示的item放进RecycleBin。
