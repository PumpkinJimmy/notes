## makefile笔记
- 底层的构建工具，通过写makefile来实现自动编译构建
- 通过target: dependency声明指明依赖结构，其中target跟dependency都是一般的文件路径
- 默认认为最先出现的命令为依赖的树根
- 通过每一条声明下面的缩进块的内容调用命令（包括调用编译器）
- 在Makefile所在目录下使用make（或使用make -f *MakefileName*）
- make后面可以跟target的名称，指明要执行的target，这就是make install跟make clean的由来——指定clean/install目标
- Makefile支持变量、通配符跟某些函数
- 由于要调用具体编译器，Makefile无法跨平台
- Makefile常常配合configure工作（其实就是一个用于配置的Linux shell脚本）
- cmake更牛逼，跨平台，更间接
### 关于PHONY
由于依赖树的根是第一个target，若以诸如clean/install这种没有被依赖的target在make的时候不会执行，只有在显式使用make clean/install时才会执行——这正是我们想要的。
但若Makefile目录下恰好有一个名为clean或者install的文件，而这个文件又不被修改，那目标clean/install就一直是最新的，命令不会执行。解决方案就是PHONY。语法：
```Makefile
.PHONY: clean
	some cleanup
```
这样就ok了,make知道了这是一个虚假的目标。.PHONY后面可以跟很多个这种虚假的target。一般会用到的有clean（清理中间文件）/install（安装）/all（编译全部）
