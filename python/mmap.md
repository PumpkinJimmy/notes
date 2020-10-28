## mmap
文件内存映射

mmap对象兼具文件和bytearray的接口。

既可以将正常文件当作bytearray处理，也可以用于**IPC（Inter Process Communication）**，且这种方法可以用于非父子进程通信