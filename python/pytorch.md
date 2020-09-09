## PyTorch笔记
### 快速上手
- `import torch`
- torch.Tensor相当于numpy.ndarray
- torch具备numpy的许多创建ndarray的函数如ones, zeros, arange等
- Tensor兼容许多的ndarray接口
  但像mean,argmax等聚合的函数指定轴时使用关键字dim而非axis
- `tensor.storage()`访问储存（可用作扁平化）
- `tensor.to(type)`转换dtype（相当于ndarray.astype(type))
  `tensor.dtype()` 同样可以转换dtype，"dtype"指代具体的数据类型如int,float,double
- `tensor.numpy()`返回ndarray（共享储存）; `tensor.from_numpy(arr)`从ndarray创建tensor（共享储存）
- `torch.save(obj, f)`储存tensor到文件，使用的是专用二进制格式
- `torch.load(f)`从文件加载tensor（文件是专用格式）
- 创建tensor时指定关键字参数device='cuda'
  或者用tensor.to(device='cuda')
  或者tensor.cuda()
  以上三种方法可以在GPU上使用tensor
- 凡是结尾为下划线的数学运算方法，都表示inplace操作（如cos()与cos_()）
- 自动求导
  - 指定关键字参数requires_grad=True开启
  - 运算会被记录
  - 调用tensor.backward()反向传播（可以传入期望的输出向量）
  - 检查tensor.grad得到梯度
  - 注意：对表示需要求导的变量的tensor开启requires_grad，对表示loss的tensor调用backward()，对需要求导的变量的tensor取grad（不是对loss）

### 常用的包
- torch 张量运算
- torch.nn 包含构建神经网络的常用组件（如全连接、卷积层、池化、CrossEntropyLoss、MSELoss）
- torch.nn.functional 常用的激活函数如relu, leaky_relu, sigmoid
- torch.autograd 所有操作的自动求导方法
- torch.optim 包含优化算法，如SGD,AdaGrad,RMSProp,Adam
- torch.utils.data 加载数据

### torch.nn
torch.nn构建神经网络的核心是Module
- 一个神经网络是一个Module对象，它在使用上相当于一个可调用的对象（函数）。
- 一个Module对象下可以嵌套多个submodule
- 所有的神经网络中的功能单元（如卷积层、激活函数等）都被抽象为Module
- nn包含很多常见的层如
  - Conv2d 卷积
  - AvgPool2d 平均池化
  - Linear 线性，即全连接
- nn包含很多常用的激活函数Module如
  - Softmax/LogSoftmax
  - Sigmoid
  - Tanh
  - ReLU
  - LeakyReLU
- Sequential可以把多个层按顺序连接起来，传入
- nn包含很多常用的损失函数Module
  - MSELoss
  - BCELoss 二值交叉熵损失函数
  - CrossEntropyLoss 交叉熵损失函数（这个用起来可以干脆省掉写softmax，但要注意提供的类别tensor必须是long类型的）

- `Module.parameters()`方法可以返回网络参数的生成器，可以用于开放给optim的算法进行操作
- `Module.modules()`方法获取网络下注册的所有模块
- `Module.state_dict()`
  
  将module的所有参数保存为一个字典

  其中字典的键名取决于submodule绑定到module的属性名
- 区别`Sequence`和`ModuleList`
  
  前者将一个Module序列串成一个Module，获得一个将输入输出依次串联的Module

  后者单纯地把一组Module有序收纳起来

  使用`ModuleList`而不是`list`的理由：我们使用`Module.parameters/modules/state_dict`之类的方法时，下属的子模块必须要注册到父模块，这些方法才能工作。若不是直接把子模块绑定到父模块的属性上，就需要`ModuleList`来绑定了

- 保存模型到文件：
  
  标准操作：
  `torch.save(net.state_dict(), 'my_module.pth')`
  - `Module.state_dict()`方法返回一个保存所有参数的字典
  - `torch.save`相当于`pickle.dump`，可以直接把state_dict保存到文件。
  
  注1：可以用`torch.save`直接保存整个网络到文件，但考虑到*可移植性*（比如说把旧版PyTorch上训练的网络参数载入到新版的网络里边），使用`state_dict()`更佳

### torch.optim
torch.optim提供了一堆优化算法类如SGD, AdaGrad, RMSProp, Adam
接口与使用大致是一致的：
- 构造optimizer时提供要优化的参数tensor
- 调用loss.backward()之前，调用optimizer.zero_grad()
- 调用loss.backward()之后，调用optimizer.step()执行梯度下降
他们的

### cuda加速
- 安装的时候必须正确安装
- torch.cuda.is_available()检验是否可以使用
- Tensor,网络（Module）都可以采用.cuda()写法获得cuda版本
- 要获得加速效果，务必保证网络跟表示数据的tensor都使用了cuda
- 在多GPU环境下使用DataParallel获得多GPU加速
### 奇技淫巧
- tensor.view/reshape简写，可以只指出某些维度大小然后其余写-1即可自动推断
  如1000 * 28 * 28变成10000个向量可以写成view(10000, -1)

### 坑点
- tensor数值运算对数据类型要求非常严格，两个相互运算的tensor很可能要求类型完全一致（甚至float和double都不行）

### Dataloader
来自`torch.utils.data.Dataloader`类用于从数据集载入batch
- ```python
  Dataloader(
    dataset= # 数据集
    batch_size= # batch大小
    shuffle= # 是否打乱
    num_worker= # 多线程载入（注意Windows下只能是0）
  )
  ```
- `Dataloader`必须迭代使用。每次迭代返回一个2元素元组，分别是数据样本（一个大Tensor）和一个标签数组
- `Dataloader`需配合`Dataset`使用。
- `Dataset`类
  - `torchvision.datasets`内置了许多常用数据集，如`MNIST`，也有方便的类如`ImageFolder`
  - 自己写的话需要实现接口`__setitem__`，用于返回单条数据样本
  - 传入`transform`参数来转换样本

### 多GPU训练
多GPU训练实质上是把一个大的batch均衡到各个GPU上进行计算，然后求出合梯度
- 先把数据载进主GPU：`net.cuda()`
- 然后再并行：`net = nn.DataParallel(net)`

注意：DataParallel并行训练出来的模型也必须载入到DataParallel的网络里