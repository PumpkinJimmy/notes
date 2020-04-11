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
### open的换行处理
不同平台上的换行符各不相同（`\n, \r\n, \r`），Python open使用newline关键字参数来指定行为
注意：所有讨论都基于文本模式打开文件（二进制模式不对换行符做处理）
- `newline=None` 默认设置，自动根据平台处理，输入时统一为`\n`，输出时相应地对`\n`转换（即Windows下会转换成`\r\n`）
- `newline=''` 自动根据平台进行分行，但不做处理地返回换行符 
### csv
标准库csv模块提供轻量级的csv文件读写功能

#### csv QuickStart
```python
csv.register_dialect('mydialect', deliminator=':', linetermintor='#') # 自定义规则
with open("mycsv.csv", newline='') as f: # 防止换行符被转换
  reader = csv.Reader(f)
  for row in reader:
    print(row)

with open("mycsv.csv", newline='') as f: # 防止换行符被转换
  writer = csv.Writer(f, dialect='mydialect')
  writer.writeheader()
  writer.writerow(row)

with open("mycsv.csv", newline='') as f: # 防止换行符被转换
  writer = csv.DictWriter(f, fieldnames=header, dialect='mydialect') # 对应字段写行
  writer.writeheader()
  writer.writerow(dick_row)
```

### shelve
如果不像用复杂的SQL数据库，`shelve`是个持久化数据的好选择
#### QuickStart
```python
shelf = shelve.open("my.db")
shelf['name'] = bbpumpkin # 储存
print(shelf['name']) # 访问
shelf.close() # 关闭
```
#### 说明
- `shelve.open`
  - `shelve.open`返回`Shelf`一个对象，`Shelf`是一个类字典对象，支持基本的字典操作
  - `shelve.open`可以指定`flag=`
    - `flag=c`（默认）如果db不存在，则新建一个，否则载入
    - `flag=r`只读
    - `flag=w`可读写
  - `keyencoding=`指定键的编码
  - `writeback=`默认是False
- Shelf的键只能是str
- Shelf对象在每次设置键值时都会自动保存数据到文件。注意，受限于Python实现，**对Shelf下的mutable对象（如list,dict）进行修改无法保存到文件**，例如：
  ```python
  shelf['list1'] = [0, 1, 2]
  shelf['list1'].append(3) # 无效！！shelf['list']仍是[0, 1, 2]
  ```
  解决方案是指定`writeback=True`
- `writeback`指明了Shelf的行为
  - 若`writeback=False`（默认），则每次设置键值时都写入文件，但无法处理mutable字典值的内部修改
  - 若`writeback=True`，则整个Shelf字典保存在内存，所有修改都可以进行，但必须显式调用`shelf.sync()/shelf.close()`才会保存到文件，且每次都写入整个字典（由于不知道改了哪里），导致保存不及时，以及占用内存多，关闭慢

### logging
标准日志模块，提供灵活的多级别的线程安全的日记记录功能。
核心类层次：`Logger->Handler->Formatter`
`Logger`就是日志记录器，包含`info(), warning()`等方法共记录日志。
一个`Logger`可拥有多个`Handler`，每个`Handler`可以有自己的记录level，自己的输出，自己的格式（也就是`Formatter` ），`Formatter`用于指定记录的格式
常用等级：
- debug
- info
- warning
- error
- critical
常用符号：
- `%(levelno)s`: 打印日志级别的数值
- `%(levelname)s`: 打印日志级别名称
- `%(pathname)s`: 打印当前执行程序的路径，其实就是`sys.argv[0]`
- `%(filename)s`: 打印当前执行程序名
- `%(funcName)s`: 打印日志的当前函数
- `%(lineno)d`: 打印日志的当前行号
- `%(asctime)s`: 打印日志的时间
- `%(thread)d`: 打印线程ID
- `%(threadName)s`: 打印线程名称
- `%(process)d`: 打印进程ID
- `%(message)s`: 打印日志信息
- `%()`格式可以用于自定义的字段以及类型，只要在log时传入一个字典到关键字参数extra
注意区分`logging.Logger`与`logging.getLogger`：
- 前者新建一个给定名字的`Logger`
- 后者获取一个给定名字的`Logger`，若不存在则新建，若不提供名字则返还根Logger，且**多次使用同一个名字`getLogger`得到同一个Logger**，而自己新建则没有这样的效果

### Python异步编程
#### Quick Start
```python
import asyncio
async def mytask(tid): # 定义协程
    print(f"Task{tid} start")
    await asyncio.sleep(2)
    print(f"Task{tid} finish")

loop = asyncio.get_event_loop() # 获得异步调用的事件循环
loop.run_until_complete(asyncio.wait([mytask(i) for i in range(1, 6)])) # 调用协程并等待结束
```

#### 基础
Python标准库采用协程来实现异步。
在早期Python中使用生成器、yield和yield from关键字可以实现简单的异步编程。这是由于在生成器使用yield时生成器会挂起等待下一次`next(generator)/generator.send()`的调用来唤醒。这一机制正是实现协程所需要的。Python给出了asyncio库实现基本的异步组件。Python3.5后陆续加入关键字`async def` `await`以及`async for` `async yield` `async with` `__aite__` `__await__` 等特性来提供异步编程的语言级支持
- `async def`用于定义一个协程
  - 语法就是在普通函数定义的`def`前面加上`async`关键字
  - 定义为协程的“函数”调用以后返回一个协程对象（coroutine），协程对象的接口类似生成器，调用`send(None)`唤醒协程，协程运行结束时会抛出`StopIteration`异常
- `await`只在使用了`async def`的块中有效，其后要跟一个`Awaitable`，一般就是一个协程对象。关键字的作用是调用协程，等待协程运行结束返回结果。但这一等待是异步的，也就是可以挂起切换出去，在那个协程运行结束之后再切回来。其行为与以前的`yield from`相同
只有这几个关键字还不够，使用协程还必须有`asyncio`提供的基础设施。
- `asyncio.get_event_loop()`返回一个事件循环，它相当于异步程序的引擎（或者说Reactor）
- `asyncio.wait(coroutines)`输入一个协程或`Future`的序列，返回一个协程，返回的协程的行为就是调用输入的所有协程。这用于并发多个协程。
- `loop.run_until_complete(coroutine)`用于运行一个协程，返回结果。为了并发多个协程，通常要配合`asyncio.wait`