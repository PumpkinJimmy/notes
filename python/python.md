## Python笔记
### pip
- pip install 安装 -r 接一个requirements文件
- pip search 搜索
- pip list 列出
- pip uninstall 卸载
- pip freeze > requirements.txt 将当前安装的包的保存到一个requirements文件
- pip show 显示包的信息
- -i 换源下载如https://pypi.douban.com/simple

### time
time用传统的C风格函数处理时间
处理的东西主要有：
1. struct_time
2. 时间戳
3. 格式化时间字符串
注1：时间戳是指用一个数表示的一个时间点，这个数的值一般等于从1970年1月1日0点整到这个时间点经过的秒数。
注2：struct_time 相当于一个结构体，拥有关于事件的几个基本的数据成员（年月日时分秒等）

处理struct_time:
- `mktime([struct])` 把struct_time转换为时间戳
- `strftime(format[, tuple])` 按照规定格式把struct_time或元组转换为一个描述时间的字符串

处理时间戳：
- `time()` 返回当前时间戳
- `localtime([stamp])` 把时间戳转换为struct_time；不提供参数则采用当前时间
  若只提供format，则采用当前时间
  常用格式：%Y-%m-%d %H-%M-%S 年月日时分秒

处理格式化时间字符串：
- `ctime()` 返回当前时间的格式字符串
- `strptime(str, format)` 将字符串按照给定格式解析为struct_time，采用格式符与strftime相同
- `asctime` 其实就是“as ctime”，弱化版的strftime，只需提供struct_time，采用默认格式输出格式化时间字符串。

### os
- os.stat(path) 返回指定路径的文件的详细信息（相当于ls -l）
- os.system(cmd) 直接执行系统shell命令

### shutil
高级的文件管理操作库与系统调用库
### Anaconda
约等于Python + 一堆Data Analysis的库 + virtualenv的发行版
这个发行版很全很好用，有GUI界面，也可以用conda在命令行管理
重点是，有清华镜像的全面支持，安装快呀
但是，要注意换源

### IPython
- 指令统计时间
  1. `%time code` 执行code，计算耗时
  2. `%timeit code` 执行code很多此，计算平均耗时
  3. `%run -t module.py` 执行module.py，计算耗时
- `_` 上一个输出；`__` 上上一个输出；`_x` 第x号输出；`_ix` 第x号输入

### str与bytes
笼统地说，str对标的是C++`std::string`，而bytes对标的是字符数组。
相同之处在与都是immutable的序列，都可以下标访问，都可以指向字符串算法。
不同之处：
- str是Unicode串，每一个字符占4字节，用于统一处理多字节字符，是等长编码的串；bytes是字节序列，1字符1字节
- str专用于处理字符数据；bytes不一定用来处理字符数据，它既可以是经过gbk/utf-8编码的多字节变长字符序列，也可以是一般的二进制数据
- str下标访问得到的是**长度为一的str**，bytes下标访问得到的是**整数**