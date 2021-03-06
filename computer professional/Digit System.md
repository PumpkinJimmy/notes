# Digit System

## Technology
- encapsulation of IC
  - SIP
  - DIP
- Scale of IC
  （待完善）
- Programmable IC
  - PLD
  - FPGA


## 半导体：逻辑门
两种典型的工艺：
- TTL
- **CMOS**（主流）

我们关注逻辑门的如下参数：
- 功耗
  
  门部件的功耗

- 延迟

  信号传递的延迟

- 噪声容限
  
  对噪声的容忍能力（与电压有关）

- 负载能力（对TTL）
  
  一个TTL门后面可以连接的（我们称为*驱动*的）门的数量是有限的，我们称之为逻辑门的*负载能力*

  一个逻辑门驱动能力有限的原因是（以*拉电流*为例），随着其驱动的门越多，电路拉电流越大，驱动门的压降越低，导致**高电平过低**，从而门失效

### CMOS

- CMOS电路基本单元：MOSFET（场效应管）

  Two Type
  - P-Channel P沟道
  - N-Channel N沟道

  N-Channel相当于*高有效*，P-Channel相当于*低有效*

  **Three Terminal**
  - G(Gate) 栅极
  - D(Drain) 漏极
  - S(Source) 源极

  其中漏极和源极提供电位的电极，栅极控制场效应管的通/断，相当于电流的“闸门”

  通常，栅极充当输入，漏极充当输出，源极提供电压

- Tri-state Gate(三态门)
  
  除了高,低电平,增加一个高阻抗态(Z).在高阻抗状态下，无论输入是什么，漏极电流都非常的小，想当于门成为了一个电阻非常大的部件。高阻抗状态相当于断路，具有保护内部的作用。

  注意区别低电平和高阻抗。高阻抗状态下输出与Vcc和GND都不相连，几乎没有电流；低电平状态下输出与GND相连。

- Open-Drain
  
  一种源极接地、漏极未连接的构造。这样构造的门不受主体电路电压的驱动，不能直接工作，必须要在漏极附近外接电压Vcc。这样构造的好处是允许单独外接电压，不受输入门的驱动能力的限制。

### TTL
TTL IC的基本部件是**三极管**

三级管的三极：
- 基极
- 集电极
- 发射极

这三极分别对应CMOS场效应管：
- 栅极
- 漏极
- 源极

除了构成逻辑门，三极管还可以构造其他电路（比如放大电路）

三极管存在两路电流：基极电流和集电极电流

## 时序电路
### 锁存器
### 触发器
### One-Shot
### Timer
### Counter
- Counter本质上就是实现分频Clock
- 异步Counter：易级联拓展，不准（组合电路带来的延迟会导致计数器的高位逐渐偏差）
- 同步Counter：准，难级联拓展

### 通用时序电路：状态机

两种状态机模型：
1. 摩尔机
   
   **下一状态只与当前状态有关**

   典型的摩尔机例子：计数器。下一个数的数只与上一个有关。

2. 米利机

   **下一状态与当前状态以及一组输入有关**

   典型的米利机例子：带计数方向的计数器。下一个输出与输入的方向以及上一时刻的数有关。
