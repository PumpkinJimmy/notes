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
- `copy2(src,dst)` 把src文件拷到dst路径，dst可以是文件，也可以是目录
- `rmtree(dir)` 删除整个目录


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
1. 核心类层次：`Logger->Handler->Formatter`
   
   `Logger`就是日志记录器，包含`info(), warning()`等方法供记录日志。

   一个`Logger`可拥有多个`Handler`，每个`Handler`可以有自己的记录level。每个`Handler`有一个`Formatter`。`Formatter`用于指定记录的格式

2. 常用Handler：
- `StreamHandler` 输出到流，常用于输出到标准输出等。默认是输出到stderr
- `FileHandler` 输出到文件
- `RotatingFileHandler` 滚动文件日志（超出最大日志大小后把旧的日志顶掉，可以设置备份）
- `SocketHandler/HTTPHandler` 支持套接字/HTTP协议的网络日志

3. 常用等级：
   - debug
   - info
   - warning
   - error
   - critical

4. 常用Format符号：
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
   - `%(name)s`: 打印Logger的名称
   - `%(message)s`: 打印日志信息
   - `%()`格式可以用于自定义的字段以及类型，只要在log时传入一个字典到关键字参数extra
5. 其他
   - 注意区分`logging.Logger`与`logging.getLogger`：
     - 前者新建一个给定名字的`Logger`
     - 后者获取一个给定名字的`Logger`，若不存在则新建，若不提供名字则返还根Logger，且**多次使用同一个名字`getLogger`得到同一个Logger**，而自己新建则没有这样的效果
   - 多次调用`Logger.addHandler`添加同一个Handler的行为会导致日志记录重复
   - **（坑）**考虑到`getLogger`获取日志的同一性，以及重复添加Handler的行为，再考虑到可能的多线程工作，有如下结论：
     
     使用`Logger`而非`getLogger`。

     如无必要，不要使用`getLogger`获取指定名称的Logger并初始化，这会导致重复初始化Logger，重复日志

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
- `await`只在使用了`async def`的块中有效，其后要跟一个`Awaitable`，可以是一个协程对象、或是`Task/Future`。关键字的作用是调用协程（如果是一个协程），等待协程运行结束返回结果。但这一等待是异步的，也就是可以挂起切换出去，在那个协程运行结束之后再切回来。其行为与以前的`yield from`相同

只有这几个关键字还不够，使用协程还必须有`asyncio`提供的基础设施。
#### asyncio高级API
```python
Future # 一个低级对象，awaitable，await future相当于等待协程结束并返回future.result()
Task # 一个包装了Future的高层对象，awaitable，行为同Future

run(coro) # 创建一个事件循环并启动一个协程（一般用于main()之类的）
create_task(coro) # 将协程打包为Task并立刻启动协程，返回Task对象，不阻塞，可用于并发

### 以下函数都不阻塞，返回的都是协程，在await/create_task之前协程都是未启动的，真正的返回值都要await获取
sleep(delay) # 非阻塞休眠
ensure_future # 同create_task，Python3.7之前
wait_for(aw, timeout) # 等待aw结束，若触发超时则抛出asyncio.TimeoutError并cancel掉aw
gather(*aws) # （注意星号）并发运行协程并等待结束，返回结果的序列
wait(aws, timeout=None) # 并发运行aws中的协程，等待至结束或超时。区别wait_for，不会抛出异常，也不会中断协程。返回两个集合：done和pending，分别放置了超时时已完成和未完成的Task
###

as_completed(aws) # 不阻塞，返回一个生成器，所有协程在第一次next后并发运行，每次next该生成器按协程结束的顺序生成已经出结果的协程（next时会阻塞等待）（也就是要await一下得到最终结果）
```
#### asyncio低级API
- `asyncio.get_event_loop()`返回一个事件循环，它相当于异步程序的引擎（或者说Reactor）
- `loop.run_until_complete(coroutine)`用于运行一个协程，返回结果。为了并发多个协程，通常要配合`asyncio.wait`
（待完善）

### 描述符（descriptor）
Python描述符是一类特殊对象，其类通过定义`__get__` `__set__` `__delete__`三个特殊方法来修改默认的访问行为
#### Quick Start
```python
class MyDescriptor:
  def __init__(self, name):
    self.name = name
  def __get__(self, obj, objtype):
    return self.name
  def __set__(self, obj, val):
    self.name = val

class MyClass:
  dspr = MyDescriptor('d')

o = MyClass()
o.dspr # 'd'
o.dspr = 'd2' # dspr.name = 'd2'
```
#### 说明
- 描述符只有在绑定为类的成员而不是具体对象的成员时才能工作
- 描述符协议属于比较底层的机制，旨在简化底层C代码，并未Python提供新机制
- 实际上`property,staticmethod,super`都是基于描述符协议的
- 描述符跟元类一样属于“黑魔法”，谨慎使用
- 某种程度上可以与元类协作（比如Django ORM？）

### `code`对象
以下四类对象内部存在一个code对象
- 函数
- 模块
- 类
- 生成器表达式
使用以函数为例，使用`func.__code__`访问
code对象的常用属性：
- `co_argcount` 参数个数，含self，不含`*args,**kwargs`
- `co_names` 局部变量名称

### json
- `dump` `dumps` `load` `loads` 顾名思义
- 关于中文：
  
  `dump/dumps`的关键字参数`ensure_ascii=True`（默认值）保证了编码得到的json文本一定是ascii字符集的。

  若被编码的对象包含超ascii的字符（比如中文），就会采用JavaScript的Unicode转义记号表示，类似`\\uxxxx`这样的码，非常不便于直接阅读修改，故可以设置`ensure_ascii=False`解决

- json模块的字符串必须是`str`而非`bytes`，如果`str`的结果是

### __new__方法
这个特殊方法定位类似`__init__`，但行为不一样
- `__new__`在该类的对象被创建时执行，**先于__init__被调用**
- `__new__(cls, *args, *kwags)`，注意签名中`cls`表示**正在创建示例的类**，后面的参数是常规的客户代码传给构造器的参数
- **该方法没有`self`参数，但不需要`@staticmethod`装饰器**
- **该方法实质上是对象的构造（工厂）方法**
  
  也就是说，**在`__new__`被调用时对象未创建，内存未分配，返回时需要返回分配好内存的对象**。
  
  一般都需要调用父类的`__new__`来为示例分配内存。

  区别于`__init__`:`__init__`方法在对象内存分配好之后调用，不允许返回值
- `__new__`不会覆盖掉`__init__`。`__new__`执行完以后会接着执行`__init__`，且构造器传入参数也没有影响。
- `__new__`一般适用于希望对实例的创建有更灵活的控制，`__init__`不够用的情况
- `__new__`可以配合*元类*机制使用

### 元类
1. 构造类的类——元类
   
   Python中的所谓*类*的行为与一般的对象基本一致（可以绑定属性、方法，有id），类与一般对象没有本质区别。它只不过是通过内建的`class`语法在程序运行之处被实例化罢了。

   实际上，“类”也有构造它的类，即所谓可以“创建类的类”，我们称为*元类*

2. `type`类
   
   默认情况下，Python中所有类的类统一是所谓`type`类。`type`除了可以作为一个函数求得一个对象的类之外，它可以“构造类”，比如说：
   ```python
   # 常规语法
   class MyClass(MyBase):
       attr1 = "foo"

   # 上面等效于
   MyClass = type("MyClass",(MyBase, ),{'attr1':'foo'})
   ```

   `type(cls_name,bases,attr)`需要3个参数。第一个是构造的类名(cls_name)，第二个构造的类的基类(bases)，第三个是构造的类的成员字典(attr)

   显然，通过合理使用`type`，我们完全可以**自定义类构造行为**

3. 使用元类机制
   
   Python允许修改一个类的元类来方便地自定义类构造行为。Python3的语法：
   ```python
   class MyClass(metaclass=MyMeta):
       pass
   ```

   其中，传递给`metaclass`的东西可以是：
   - 接受`(cls_name,bases,attr)`参数、返回一个类的可调用对象（如函数、callable objct）
   - `type`的派生类
   如果使用`type`的派生类那一般会重写`__new__` `__init__`方法来自定义类构造行为

4. 元类的应用
   
   总的来说，当你需要对*类本身的数据*做工作时，你需要元类。
   
   结合元类和`__new__`使用是常见的元类用法，旨在为类本身添加额外的成员、属性、方法



   一个成功的例子是**Django ORM**。在ORM中，使用一个`model`的派生类来表示一个对象数据模型，通常对应一张表。同时，通过定义类成员来定义数据字段。

   通常**类是业务逻辑的抽象，类的实例是业务处理的实体**，而在这一案例中，**类本身也成为业务处理地实体**时，故使用元类。

   说到底，元类是Python的“黑魔法”。这种晦涩的特性通常用不到，就算用到也往往是有其他的替代方法的。
   
   以ORM的案例为例，其实只要将ORM配置写在平凡的配置文件中，并对表和表的数据分别编写类就可以实现类似的业务功能，此时业务处理的实体就不再是类本身而是类的实例。换言之，元类的黑魔法在此例中的作用是让客户代码更“优雅”，说穿了，就是语法糖。

   > “元类就是深度的魔法，99%的用户应该根本不必为此操心。如果你想搞清楚究竟是否需要用到元类，那么你就不需要 它。那些实际用到元类的人都非常清楚地知道他们需要做什么，而且根本不需要解释为什么要用元类。” —— Python界的领袖 Tim Peters
   

### import机制
1. 导入`module`（即单个文件）
   
   当一个`mymod.py`文件出现在Python搜索路径里面时，`import mymod`的行为是：
   - 若`mymod`未导入，则*执行* `mymod.py`文件且将其中的变量全部归为模块对象`mymod`下的属性
   - 若`mymod`已导入，则不做操作，避免重复导入

2. 导入`package`（即导入文件夹/包）
   
   仅当文件夹目录下有一个`__init__.py`时，该文件夹会被视为`package`，且**行为同导入单个模块，只是模块本体为__init__.py**，也就是说**没有自动导入`package`目录下的`module`的额外行为**

   若想要导入`package`目录下的`module/subpackage`，如下两种方法可用：
   - 在`__init__.py`下导入下属的子模块如`from . import submodule`
     
     这种方法的好处是一键导入，且子模块正确的放在了`package`命名空间中
     缺点是可能一次性导入太多不需要的模块
   - 使用完整路径手动导入
     
     也即`__init__.py`无需做额外处理，客户代码使用时采用`import package.module`的形式单独导入特定子模楷

     此法的优点是用多少导入多少，缺点是麻烦
3. `from import` 导入
   - 可以用于从package中导入module或subpackage
   - 不论模块是否已经导入，必定重新载入

4. 相对路径导入
   
   在组织一个完整的package时，直接import是不可取的。这是因为导入同一文件夹下的模块的行为依赖Python将当前路径导入搜索路径这一前提，在重用package时非常有可能出错。正确做法为：

   ```python
   # pkg/utils.py
   # some useful code


   # pkg/mymod.py
   import .utils
   ```

   在最开始加一个点的记法就是相对路径导入，被省略的部分即使*当前package*

   注1：必须注意当前`mod.py`文件运行的状态：当它作为主程序运行时它是`__main__`而不是`pkg`中的包，相对路径无效。**相对路径必须在`mod.py`以包的形式被导入时才能正常工作**，参见`__name__`属性的含义

5. importlib
   
   标准库模块`importlib`提供了一系列灵活导入的工具。（主要用于交互式解释环境）

   - `reload` 强制重载模块代码，重新导入。主要用于交互式环境。根据Python的import机制，已经导入的模块不会重复导入。但交互环境下可能会有已经导入模块被修改的情况。这时需要reload一下。

   - `import_module` 提供模块名字符串，导入模块

### conda常用指令
- conda create
- conda activate
- conda install
- conda config
（待完善）

### 为什么使用Anaconda
- 多环境管理（常见于为了运行旧代码而搭建的旧环境）
- 自带多个常用包
- 有好用的清华源安装PyTorch
- 自带python-dev

### 坑
#### 多进程
> The __main__ module must be importable by worker subprocesses. This means that ProcessPoolExecutor will not work in the interactive interpreter.

对于Windows，多进程不能用在交互式解释器中跑

但Linux由于神奇的fork机制，是可以的