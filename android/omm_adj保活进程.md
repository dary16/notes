### 了解Android进程的回收机制
出于体验和性能的考虑，在系统内存不足的情况下，系统根据自身的一套进程回收机制来判断要kill掉哪些进程
    什么是oom_adj??它是Linux内核分配给每个系统的一个值，代表进程的优先级。值越大表示优先级越低，越容易被回收，下面通过降低oom_adj的值，尽量保证进程不被系统杀死
| 优先级 | 进程状态 | oom_adj | 特性与场景 |
| :----:| :----: | :----: | :----: |
| 1 | 前台进程 | 0 | 该状态下的进程表示正在与用户交互，除深度定制ROM或系统错误的情况，系统不会杀死该进程。具体场景如下：
 (1) 进程中有一个Activity，该Activity正在与用户交互，其中，“1像素”界面保活就是根据这个原理实现的；
 (2) 进程持有一个Service，该Service与正在交互的Activity绑定；
 (3) 进程持有一个Service，该Service调用startForeground将其置为前台，其中前台Service保活就是根据这个原理实现的；
 (4) 进程持有一个Service，该Service正在执行生命周期的某个方法，如onCreate，其中，循环播放一段无声音乐就是根据这个原理实现的
 (5) 进程持有一个BoardcastReceiver，该BroadcastReceiver正在执行onReceiver;|
| 2 | 可见进程 | 1 | 该状态下的进程表示用户仍然可以被用户看见，但是不能与用户进行交互，具体场景如下：
 (1) 进程中有一个Activity，该Activity调用了onPause方法；
 (2) 进程持有一个Service，该Service与可见的Service绑定；|
| 3 | 服务进程 | 5 | 该状态下的进程表示进程运行着一个Service，该Service由startService启动且不与任何Activity绑定，也不属于前台进程或可见进程的状态 |
| 4 | 后台进程 | 6 | 该状态下进程表示进程持有一个不可见的Activity，即Activity的onStop方法被调用，但是onDestory方法未被调用 |
| 5 | 空进程 | 9-25 | 该状态下的进程表示进程不包含任何活跃的组件，它只是作为缓存的形式存在，以加快进程启动的速度 |

