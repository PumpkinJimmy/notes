## multiprocessing
多进程库

### 坑
注意使用`Process`是，如果传入一个bound method的话，注意子进程的绑定的self和父进程的object**不是同一个对象**

理由：多进程资源不共享，且bound method实质上是绑定了self到method上的，所以虽然传入的只是method，但在子进程中运行时已经将绑定的对象复制了一份