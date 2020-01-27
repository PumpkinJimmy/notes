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
注：时间戳是指用一个数表示的一个时间点，这个数的值一般等于从1970年1月1日0点整到这个时间点经过的秒数。
常用：
- struct_time 相当于一个结构体，拥有关于事件的几个基本的数据成员（年月日时分秒等）
- localtime([stamp]) 把时间戳转换为struct_time；不提供参数则采用当前时间
- mktime([struct]) 把struct_time转换为时间戳
- strftime(format[, tuple]) 按照规定格式把struct_time或元组转换为一个描述时间的字符串
  若只提供format，则采用当前时间
  常用格式：%Y-%m-%d %H-%M-%S 年月日时分秒
- strptime(str, format) 将字符串按照给定格式解析为struct_time，采用格式符与strftime相同
- asctime 弱化版的strftime，只需提供struct_time，采用默认格式输出格式化时间字符串。
### os
- os.stat(path) 返回指定路径的文件的详细信息（相当于ls -l）
- os.system(cmd) 直接执行系统shell命令
### shutil
高级的文件管理操作库与系统调用库
