# pthread
## compile option
`-lpthread -D REENTRANT`
## API Quick
```c
pthread_create() // create a thread
pthread_exit() // end current thread
pthread_join() // wait & sync
pthread_mutex_lock() // lock
pthread_mutex_unlock() // unlock
pthread_cond_wait() // wait for a condition variable
pthread_cond_signal() // activate condition variable
```

## 为什么有了互斥锁，还需要条件变量

理论上来说，有了互斥锁其实已经可以保护线程间共享资源了。使用条件变量是性能考虑。

考虑一个例子：线程1,2负责累加一个整数变量，线程3在变量达到1000000时清零并打印消息。

由于变量是线程共享的，三个线程访问变量时都需要获得互斥锁。问题是变量在大部分时候都达不到1000000，线程3反复获得锁只为了检查取值，浪费CPU时间。条件变量解决了这一问题。线程3平时挂起不运行，等待条件变量唤醒，线程1，2负责在变量达到1000000时利用条件变量唤醒线程3并可以传递一些信息。如此可以减少线程3反复获得锁。

注意：
1. 条件变量作为共享资源总是需要配合互斥锁使用
2. 条件变量其实是操作系统的资源管理模型的软件形式：

   操作系统总是管理着所有的系统资源，阻塞所有需要同一硬件资源的进程，保证同一时间只有一个进程访问到该资源。此外，操作系统还负责调度，在某一硬件资源的上一个使用者用完之后从等待队列中调入下一个进程使用资源。

   条件变量充当了“资源”，等待条件变量的线程挂起，只不过使用完需要使用者取唤醒挂起的线程，而不是由操作系统自动完成。