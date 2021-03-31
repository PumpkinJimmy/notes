## 2-22报告
### Event Camera Intro
事件相机，又称脉冲相机

事件相机优点
- High Dynamic Range
- Low Latency (10ms -> 1us)
- Low Data Rate
- Low Power Consumption (1%)
- 最高精确到微秒microsecond

缺点
- async：事件与事件不是等时间隔的（区别传统摄像机的等时采样）
- sparse：在特征空间上高度稀疏，无法直接使用CNN处理

### ICCV 2019 "event sequence embedding" 笔记
- event sequence
  
  event:一个四元组$(x, y, t, p)$，表示在$t$时刻，在viewport的$(x,y)$处发生了亮度的增强/减弱（对应$p=1或p=-1$

  event sequence就是一个event的序列

- Stereo Matching
  
  即对于一个viewport中获得的点，在另一个viewport中找到与之在物理上匹配的点
- Current Difficulties
  - frame-based vs. event-based
    - frame-based是稠密的2d图像，适合使用CNN处理
    - event-based是稀疏的点，需要做变换才能输入到CNN中
    - 朴素的对事件的timestamp进行binning离散化会导致非常大的时间维度
    - 现有方法常把event-sequence转换成frame-based的表示，这一frame-based表示即是**event image**
    - 常用的转换方法是手工处理
  - RNN/LSTM
    
    鉴于事件的序列性，一个自然的想法是使用RNN。但RNN丢失了空间信息，而空间信息在某些任务中是至关重要的（如Stereo Matching）
  
  - SNN、专门设计的网络
    
    难以求导，难以实现，难以在流行的GPU为后端的框架下训练、运行

- Proposed Method
  
  使用一对水平对齐的事件相机作为双目摄像头，双目对应的event sequence分别表示为$E^l, E^r$，则问题表示为求estimated disparity tensor $\hat{D}$：

  $$
    \hat{D} = Net(E^l, E^r| \Theta) \in [0, dmax]^{h\times w}
  $$

  其中$\hat{D}$是一个与viewport大小相同的tensor，表示左目视图位于$(x,y)$处的pixel与右目视图位于$(x - \hat{D}_{y,x}, y)$的pixel相匹配（因此称其为disparity tensor）

  $E^l, E^r$表示双目摄像头采集的事件序列，$\Theta$代表网络参数

  而$Net$则是论文所提出的网络

- Network Architecture
  
  基本上就是三个模块：embedding->matching->regularization

  本文重点在embedding策略

- Embedding
  - embedding可以理解为将输入表示为合适的descriptor的过程。DL里的embedding是可学习的
  - 对于稀疏的event sequence，论文提出$f_S(f_\tau(E))$形式的模型
  - $f_\tau$
    - $f_\tau$的输入是位置相同时间不同的事件序列的子集，输出一个向量表示。
    - 该过程也称为temporal aggregation。论文提出了一种temporal aggregation策略，并比较了多种策略，证实了提出的策略的优越性
    - 对于每个位置都可以计算出一个temporal aggregation表示，故$f_\tau$最终可以产生*event image* $I$, $I \in R^{c\times h \times w}$
  - $f_S$
    - $f_S$是spatial aggregation
    - $f_S$的输入是event image，输出最终的descriptorr
    - 论文选择的是一种平移不变函数$f_S$，比如空间域滤波器，也即**标准卷积运算**
  - 使用$f_\tau$计算event image时，论文没有使用给定坐标上的全部事件，而是给定时间点发生的事件子集。此法利用时间上的局部性，可以适配高速移动和低速移动的场景。

  - Temporal Fully-connected Layer (CFC)
    
    本文的创新点，用以解决event sequence的非等时采样问题。

    令$p_i,t_i$表示第$i$个事件的polarity和timestamp，$d$表示输出的descriptor，$\sigma$表示某非线性变换

    相比于标准的FC:
    $$
      d = \sigma\left(\sum w_i p_i + b\right)
    $$

    CFC使用如下形式：
    $$
      d = \sigma\left(\sum w(t_i)p_i + b\right)
    $$

    其中$w(t_i)$是$t_i$时刻对应的权重。

    CFC考虑到了事件的timestamp是非等时的连续值，故利用一个可学习的MLP计算$t_i$对应的$w_i$

    论文中考虑到要和hand-craft方案做对比，而hand-craft方案是直接统计polarity为+1和-1的个数从而产生2维向量作为descriptor，故令CFC也产生2维向量

- Dataset
  
  论文使用的数据集是MVSEC。
  
  MVSEC是目前唯一公开可用的大规模事件序列立体视觉数据集，也是该领域事实上的标准数据集。

  该数据集采用双目事件相机以及一个LIDAR加上一些其他传感器组成的设备来采集数据，采集场景包括白天、晚上、室内、室外，搭载设备的载具包括六轴无人机、汽车等。采集的信息包括事件序列、雷达点云、深度信息等。

  雷达的深度信息为event camera相关研究提供Ground Truth信息

  数据以rosbag格式以及hdf5提供。

  复现结果见附件

### SNN
- 神经科学基础
  - 神经元之间通过突触传递信息
  - 信息在神经元上传递的形式是**脉冲（spike）**，也即短时间（1-2ms）内电位的迅速上升（100mV）再复位。
  - 不同脉冲电位变化曲线基本上是相同的，因此电位变化曲线的形状本身本身不携带信息，是脉冲发生的数量与时机才携带信息
  - **不应期（refractory period）**是指神经元几乎产生脉冲输出的一段时间。两次脉冲之间存在最小的间隔，这一间隔即是不应期
  - 动作电位（action potential）
  - 静息电位（rest potential）
  - 突触后神经元（输出神经元）**对输入脉冲的相应是线性的**。观察发现多个脉冲相继到来的结果是常常是简单的电位叠加。但是**当电位超过阈值会使输出神经元兴奋，产生动作电位**，最后还原至静息电位
  
- Integrate-and-Fire Model (IF)
  - 前提一：神经元动力学过程可以被不精确地近似为求和过程，或者说是*积分过程*，并在超过一定阈值后产生动作电位的机制
  - 前提二：不同脉冲电位变化曲线基本上是相同的，因此电位变化曲线的形状本身本身不携带信息，是脉冲发生的数量与时机才携带信息。脉冲可以被简化为在某一时刻发生的**事件**。这是我们不试图描述电位变化曲线的形态。
  
  基于上述前提，有Integrate-and-Fire模型：

  - 记输出神经元电位为$u(t)$，初始电位为$u_{rest}$，点火阈值$\nu$
  - 与其相连的输入神经元产生的脉冲可以加权线性叠加提高其电位，同时$u(t)$会因为随时间均匀“泄露电荷”（若是随时间泄露则应该称为LIF(Leaky Integrate-and-Fire)模型
  - 当$u(t) > \nu$时输出神经元兴奋并产生脉冲输出。$u(t)$第一次超过$\nu$的时刻就是脉冲产生的时刻
  - 脉冲输出后会有一段事件的不应期，不产生脉冲

- ANN vs SNN
  
  传统神经网络结构中，两个神经元之间只有一条突触连接。

  脉冲神经网络受生物启发，允许两个神经元之间**有多条时延不同权值不同的突触连接**

  脉冲神经网络的脉冲时序相关，可以用于处理event sequence这样非等时采样且稀疏的输入

  脉冲神经网络的前向传播推理是硬件友好的

  脉冲神经网络的神经元传播函数往往是不可微的，故不能直接使用BP训练

### ANN2SNN
- Intro
  
  - BNN硬件友好，但相比Full Precision退化严重，accuracy较低
  - SNN硬件友好，功耗较低，精度较BNN高，且其行为更适合于基于事件的稀疏的输入的推理，但难以训练
  - BNN与SNN本质上是几乎等价的（尤其是IF神经元和ReLU功能几乎是相同的）
  
  故我们采用一种想法：使用常规方法训练一个BNN，然后再转换成SNN

- Bindsnet
  
  这是一个基于Pytorch的SNN仿真库，包含了构建、运行SNN网络、以及将传统ANN转换为SNN的工具。

  注：这一个库的仿真采用的是固定长度时间的Time Stamp迭代计算，与真实的SNN硬件有出入

  `bindsnet.conversion.ann_to_snn` ANN2SNN转换函数

  `bindsnet.encoding` spike编码转换模块

- B-SNN
  
  将BWN转换为B-SNN

  原始网络限制：除了Binary-Weight之外，还不包含bias项和Batch-Norm（因为在转换中acc损失严重）

  也就是说只是修改了标准的Backbone网络的结构，然后直接掉包转换

- voltage-threshold的设置
  
  threshold的设置存在一个trade-off：
  - 若threshold太低，神经元将持续fire
  - 若threshold太高，神经元的latency较大


## CVPR 2019 Unsupervised Optical Flow and Depth Prediction

（待完善）

简单地说就将event sequence在时间上离散化得到常规image然后输入到常规的CNN自编码器中

## EventNet（待完善）