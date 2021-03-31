- [Computer Architecture](#computer-architecture)
  - [Term Reference](#term-reference)
  - [1. Introduction](#1-introduction)
    - [CPU Performance](#cpu-performance)
    - [Power Wall](#power-wall)
    - [Paralism](#paralism)
  - [2. Instructions](#2-instructions)
  - [3. Encoding & Arithmetic](#3-encoding--arithmetic)
  - [4. Processor](#4-processor)
    - [Pipeline & Hazard](#pipeline--hazard)
    - [指令级并行](#指令级并行)
    - [其他](#其他)
  - [5. Cache](#5-cache)
    - [各类储存器及其工艺](#各类储存器及其工艺)
    - [储存器架构](#储存器架构)
    - [Cache Mapping](#cache-mapping)
    - [Cache Replacement Policy](#cache-replacement-policy)
    - [Cache Writing](#cache-writing)
    - [Cache Performance](#cache-performance)
    - [Multi-Level Cache](#multi-level-cache)
    - [虚拟内存](#虚拟内存)
# Computer Architecture
## Term Reference
- ISA: Instruction Set of Architecture
- MIPS: Millon Instructions per Second
- CPI: (Average) Clock cycles of a Instruction
- Response Time
- Throughput: 吞吐量

## 1. Introduction
### CPU Performance
0. 一般我们说的Performance主要是指：
- CPU Time 响应时间：完成*一个*任务所需要得时间
- Throughput 吞吐量：单位时间内完成的*任务数量*
- CPU Time = Execution Time = Response Time 只取决于速度；Throughput与速度、并行度都有关系
1. **CPU Time formula**
  
    $$
        \text{CPU Time} = \frac{\text{Instr. Count}\times \text{CPI}}{\text{Clock rate}}
    $$

    由公式可知，*一个程序*在*单个Core*上的CPU Time取决于三个因素：
    1. Instruction Count
    
        更少指令数的程序运行更

    2. CPI
    
        不同的指令所需的cycle数不一样。使用所需cycle较少的指令（如加法）会快于一些大CPI的指令（如读写内存）
    3. Clock rate
        
        更高的主频带来更短的CPU Time
    
    注意：这三个参数是**相互关联**的，改变其中一个也有可能改变其他几个。也即：一般不能指望通过简单地提升其中一个指标来调高总体性能

2. Cycle Time, Clock Rate
  
    Cycle Time: time per clock cycle
    $$
        \text{Clock Rate} = \frac{1}{\text{Cycle Time}}
    $$

3. **MIPS**
    $$
        \text{MIPS} = \frac{\text{Instr. Count}}{\text{Execution Time} \times 10^6} = \frac{\text{Clock rate}}{\text{CPI}\times 10^6}
    $$

    由上述公式可知，MIPS只与CPU Clock rate和
    - Compiler's Performance Impact
    
    编译器通过减少指令数来让程序得CPU Time更少
4. 实际应用中的性能问题
  
    实际上，衡量一台机器的性能是复杂的。影响性能的因素有：
    1. CPI
    2. Clock rate
    
    但这些只是衡量单个CPU的MIPS。除此之外还有：

    3. Instruction Count
        
        不同的程序具有不同的指令数

    4. Compiler
        
        编译器通过影响程序指令数来影响CPU Time
    
    5. Core数
        
        具有更多的Core时可以提高Throughput，也可以通过并行来减少单个程序的总CPU Time
    
    6. 专用硬件
        
        某些为特定任务设计的硬件会比通用处理器性能更佳（如显卡的渲染管线、DSP等）

    7. 等等
    
    综上所述，我们难以简单地通过直接使用硬件参数计算比较机器地性能。真正可行的性能比较方法是**使用Benchmark**比较性能（类似通俗意义上的“跑分”）。*Benchmark*通过对机器运行标准的工作负载，比较实际运行时间来比较相对性能。专用的硬件可以有专用的Benchmark

    一个权威的Benchmark是SPEC组织

5.  Factors
  
    宏观而言，*一个程序*的性能与多个因素有关系：
    1. 算法
        
        通过减少指令数、使用CPI较低的指令来提高性能

    2. Compiler
        
       通过减少指令数、使用CPI较低的指令来提高性能
    
    3. ISA
       
       影响CPI
    
    4. Hardware Architecture
       
       影响CPI,Clock rate以及Multi-Core影响Throughput等

6. Pitfalls:
  
     1. MIPS与程序的指令数**无关**
     2. MIPS高不等于CPU Time更少 
       
        同一个程序，并不是在MIPS高得CPU上就有更短得CPU time。这是因为可能同一程序在不同CPU上所需指令数不同
     3. 严格来说，增加CPU Core只能提高Throughput不能减少Response Time（由于单个Core的速度没有提高）

### Power Wall
1. Why not higher clock rate
   
   更高的主频->更高的电压->平方增长的功耗

   最初的20年，厂商在提升主频的同时在想办法降低电压来降低功耗

   但现在，已经没有办法再降低电压了

2. 为什么不继续缩小晶体管

   一般来说，芯片的主体工艺是CMOS。这种工艺加工的晶体管即使在不工作的时候，只要通电，就会有漏电流（Leaky Current）。随着晶体管继续缩小，晶体管之间的距离不断缩小，漏电流将急剧增大，导致功耗增加

综上，只要功耗维持在合理范围内，就无法提高主频，也无法继续缩小晶体管，**单核性能难以再提高**。这就是为什么要讨论*多核*，*多处理器*，*异构处理器*
### Paralism
（待完善）

## 2. Instructions
0. A bit of history
   
   Von Neumann Architecture 冯诺依曼架构
   - 指令只是一种特殊的数据
   - 指令与普通数据共享内存、总线和地址空间
   - 优点：简单，性价比高

   Harvard Architecture 
   - 指令与数据分开储存
   - 优点：防止了修改内存中指令的常见hack；可以**并行取指令和取数**，性能更高
   - 缺点：更复杂，更昂贵，*依赖软硬件设计者去权衡指令和数据问题*

1. What is instructions
   
   *instructions* are the primitive operations of CPU

   ISA是“Instruction Set Architecture”是硬件和软件的界面（interface）

2. Registers
   
   1. names
   
      `$16-$23 ->$s0-$s7` for variable

      `$8-$15 -> $t0-$t7` for temporary

      `$a0 -> $a3` for process argument

      `$sp` stack pointer

      `$fp` frame pointer 

      `$gp` for global memory access

      `$v0-$v2` for process return value

      `$ra` for caller address
   
   2. register have no types.
      
      Operations you apply determine how interpret the data.
   
3. MIPS Instruction Format
   
   1. R-type for register operations
   2. I-type for immediate operations and data transfer
   3. J-type for jump operations
4. Arithmatic Operations
   
   - `add/addu/sub/subu`
   - `and/or/nor/`
   - `sll/slr`
   - Immediate Operations
   - 32-bits Immediate Number
     - `lui`
   - Multiply
     - `mult`
     - `mfhi/mflo`
   - Divide
     - `div`
     - also `mfhi/mflo`

5. Big Endian vs. Little Endian
   
   端序讨论的是对一个*字*中字节的地址顺序

   我们规定*大端序(Big Endian)*是指一个字的*最左边(leftmost)*的字节为最低地址

   反之，*小端序(Little Endian)*是指一个字的*最右边(rightmost)*的字节为最低地址

   所谓“最左边”，是指对应我们的“阿拉伯数字书写顺序”的最左边，也就是说**大端序的字最高位是最低地址**

   本质上，端序问题的根源是：
   - 计算机往往以字为单位处理数据，但往往以字节为最小寻址单位
   - 阿拉伯数字低位到高位的顺序是从右到左，但我们的书写顺序确是从左到右
   
   第一条迫使我们约定字节编址顺序；

   第二条则导致我们有两种合理的约定方式：
   - 如果认为一个字是一个字节的流，那从左到右摆放的字节自然是从左到右地址递增（大端序）
   - 但若按照阿拉伯数字的习惯数字当然是从右到左地址递增（小端序）
   
   大端序要注意：
   字的“最低位字节”与“地址最低字节”不一样！

   小端序要注意：
   连续的比特流中，字内字节顺序需要“反转”！

   （综上，我觉得还是小端序比较反人类）

6. Data transfer between reg. and mem.
   
   - Basic: `lw/sw` 按字取/存
   - `lh/sh;lb/sb` 按半字/字节做存取
   - Byte Alignment
     
     在按字/半字存取时，必须注意*字节对齐(Byte Alignment)*，（以32机器为例）也即：

     **按字存取时地址必须是4的倍数；按半字存取时地址必须是2的倍数**

7. Branch and Jump Operations
   - `beq/bne`
   - `slt`
   - `j`
   - `jr/jal/jalr`
     
     - `jal $reg`  jump to the address specified by reg. and store `PC+4` in `$ra` (mostly used to call process)



## 3. Encoding & Arithmetic
1. 补码
   - 利用有限位加法和溢出实现“化减为加”
   - 补码=反码+1
   - 虽然加减法效率非常高，**但导致了两套不同的比较、溢出机制**
     
     Unsigned Addition:

     > Carry-out(进位) indicates overflow, i.e.:

     $$ (1101)_2 + (0100)_2 = (1 0001)_2\quad \mathrm{overflow}\\
        (0111)_2 + (0001)_2 = (1000)_2 \quad \text{not overflow}
     $$

     Signed Addition:

     >Two operands with the same sign and produce a opposite signed indicates overflow
     >
     >Two operands with different sign **never overflow**

     $$ (1101)_2 + (0100)_2 = (0001)_2\Rightarrow (-3 + 4 = 1) \quad\text{not overflow}\\
        (0111)_2 + (0001)_2 = (1000)_2 \Rightarrow (7+1=-8)\quad \mathrm{overflow}
     $$ 

     Unsigned Comparison:

     $$
     (1111)_2 > (0001)_2 \leftrightarrow 15 > 1
     $$

     Signed Comparison:

     $$
     (1111)_2 < (0001)_2 \leftrightarrow -1 < 1
     $$

2. 浮点数
   - IEEE 754 单精度：
     
     32位宽
     
     格式：符号位(S, 1bit) | 指数(E, 8bit) | 尾数(Significand, 23bit)

     公式：$Value = (-1)^S \times (1.\mathrm{Significand})^{E-127}$

     指数取值范围：1~255（注意0不是常规指数）

     尾数取值范围：1~$2^{23}-1$（注意若位数为0，则指数必须相应为0）

     表示范围：$\pm \times  2



## 4. Processor
### Pipeline & Hazard
Pipeline：
-  定义：
   
   考虑多周期CPU的五个阶段，显然当一条指令开始译码时，下一条指令的取指已经可以进行了；同理一条指令在ALU计算，下一条指令已经可以译码了。

   利用指令的分阶段特性并行执行多条指令，从而在无法提升频率的情况下调高吞吐率进而最终提高性能的手法就是Pipeline
- 现代CPU全部采用Pipeline技术来实现、加速指令级并行
- Pipeline的stage数决定Pipeline的throughput的上限，即一般流水线越深，吞吐率越高；但流水线越深，流水寄存器就越多。所以在空间允许的情况下，流水线越深越好。
- Pipeline processor的频率上限的瓶颈是delay最大的stage（一般是MEM），故好的Pipeline应该均衡每个stage所需的时间

流水线面临三种Hazard：
1. Structure Hazard:
   - 同一cycle，多条指令的不同stage都在进行，使得多个不同的stage不再能够复用硬件
   - 如果按照多周期CPU的设计，不同指令所需的stage数目不同，那Pipeline会导致一些指令在同一cycle需要进行同一个stage，从而导致两条指令竞争同一套硬件
   
   解决方案如下：
   - 为每一个stage单独准备一套硬件，不再复用
   - 将所有指令规整为运行相同的stage数
   
   由流水线的特性，在warmup之后，每一个cycle都有一条指令完成，因此为规整化增加的stage最终不会影响throughput

2. Data Hazard
   - "Read and write"，即在同一个cycle读写同一个寄存器。
   - "Read after write"(RAW)，即在上一条R-Type指令还没有Write Back寄存器之前，后面的指令已经读取同一个的寄存器的值进行ALU运算了
   - "Load and use"(LU)，一条load指令之后立刻使用load的Rt寄存器进行运算，是RAW的一种

   解决技术：
   - Half-cycle：cycle一分为二，前半个周期写有效，后半个周期读有效
   - Forwarding（转发）：对于RAW Hazard, 通过增加`EX/MEM->ID/EX,MEM/WB->ID/EX`转发旁路，加上转发控制信号，可以在检测到Hazard时将正确的结果转发回`ID/EX`寄存器
   - Stalling（阻塞）：对于LU Hazard，由于load指令在MEM阶段才出结果，此时下一条指令的EX阶段已经执行完毕了，Forwarding无论如何都解决不了了，必须要阻塞至少一个cycle，然后再转发，才能解决。硬件阻塞需要加入适当的控制信号使得阻塞的部件正确锁存/清理所有的信息；软件则只需要插入一条NOP空指令即可
   
   考虑R-Type指令：若不做任何处理，一条指令的写至多与后续的三条指令的读产生数据冒险，也即在写指令的WB完成之前处在ID(Reg),EXE,WB的三条指令。
   - "Half-cycle"方法解决掉ID冲突。前半个周期WB写入寄存器堆，后半周期ID读寄存器堆解码出正确的值。引入此法后写只与后续两条指令的读竞争
   - "Forwarding"解决掉剩余的EXE,WB的竞争。这是由于R-Type指令的结果最早在EXE阶段结束即可得到。至此R-Type数据冒险全部消除
   
   考虑Load指令：若不做任何处理，一条指令的写至多与后续的三条指令冲突。
   - "Half-cycle"同理
   - "Forwarding"只能解决掉
    
   
   此外，编译器可以在不改变代码逻辑的情况下恰当地重排指令来避免stall（实验表明编译器重排指令可以减少33%以上的stall）

3. Control Hazard（待完善）
   
   分支预测：Pipeline不等待分支指令完成，预测分支是否被触发，然后按照预测往下执行，若预测错误，则清楚已经执行的

   编译器调度
   

### 指令级并行
主要技术“多发射”，也即多条指令真正在同一时刻开始执行，依赖多条流水线来实现
- 静态多发射技术，也称VLIW
  
  VLIW指一个指令字中包含同时执行的多条指令。**由编译器进行调度打包指令、消除所有冒险**。

  好处是由编译器解决一切，硬件简单；

  缺点是生成的汇编指令与硬件实现高度耦合；同时在多发射中产生新的数据冒险和控制冒险会导致部分性能的损失

- 动态多发射技术，也称超标量（Superscalar）
  
  由硬件完成指令的并行打包译码和消除冒险。

  > 为什么明明依靠编译器调度就足以消除冒险，还需要硬件处理冒险呢？
  > 
  > 因为并非所有阻塞都是可以预测的，如Cache Miss阻塞，这些需要硬件处理才能发挥出全部性能

  进一步的，由于实际上不同指令所需的周期是不一样的，多发射会导致“后发先至”（也就是后快开始的指令先完成）。在保证没有竞争之后，我们可以允许这种情况的发生，也即“乱序执行乱序完成”

- 无论哪种多发射技术，都需要额外的运算单元、端口、和寄存器去同时处理多条流水线同时进行

- 现代CPU，特别是Intel，在单个核心中采用了上述所有的高级流水线技术：
  - 10+级流水线
  - 动态分支预测
  - 超标量
  - 乱序执行

### 其他
无论是pipeline中重排指令减少stall，还是多流水线中循环展开，都依赖编译器采用正确的优化策略。可见，编译器跟程序的性能息息相关；想要有好的编译器优化策略，必须充分理解计算机底层的架构。

## 5. Cache
### 各类储存器及其工艺
- 寄存器
- SRAM
- DRAM
- Flash
- 磁盘1
### 储存器架构
三级架构：缓存-主存-辅存

缓存：高速SRAM

主存：DRAM

辅存：低速Flash/磁盘

### Cache Mapping
- 直接映射
- 全相联
- 多路映射
（待完善）
### Cache Replacement Policy
- LRU
- FIFO
- Random
（待完善）
### Cache Writing
- Write-Through
- Write-Back
（待完善）

### Cache Performance
- Basic
  
  - Miss Rate：缺失率，缓存不命中的几率
  - Miss Penalty：缺失代价，缓存不命中需要访问主存带来的时间开销

- AMAT
  
  全称Average Memory Access Time

  $$
   \text{AMAT} = T_{hit} + Miss Rate \times T_{miss}
  $$

- CPU Time
  不考虑访存延迟（即Cache全命中）：
  $$
   \text{CPU Time} = ()
  $$
  考虑储存器架构后的CPU Time修正为：
  $$
   \text{CPU Time} = 
  $$
- 待完善

### Multi-Level Cache
在三层架构的基础上，继续对Cache分层，分L1、L2甚至L3

对于具有L1、L2的Cache，L1往往容量较小，但速度几乎与CPU同级；L2则比较大，足以覆盖大部分的主存访问。

多级缓存架构对缓存性能的提升在于通过二级缓存减小了L1 Miss Penalty

### 虚拟内存
- 虚拟地址与段页式内存管理
  - 虚拟地址的结构：逻辑页号:段号:段内地址
  - 一言以蔽之，页号需要*映射*，段号用于*偏移*
  - 页式内存管理实质上是将主存作为虚拟存储器系统的高速缓冲部分。其管理方式可以类比Cache：
    - 将整个虚拟存储器的空间划分为许多相同大小的页
    - 通常主存只会保留部分的页，余下的页会放在容量更大的辅存当中
    - 由于主存只保留部分页，故工作在虚拟存储器上的逻辑页号与用于寻址的物理页号无法构成简单的一一对应关系。必须维护一个映射表来将映射为主存的物理页号用以寻址
    - 使用页表来实现这一映射
    - 由于页表存在内存中，查询页表的性能是不高的，我们引入TLB（块表）来加速
      - TLB表存放在Cache中，本质上是对页表空间进行Cache加速
  - 段式内存管理是源自冯氏计算机程序的分段技术，使用简单的偏移来完成映射
    - 冯诺依曼结构下的程序会人为地划分程序段和数据段，这些段在段内连续且段不等长
    - 段式管理实质上是对存储空间进行“分区”，段号的对应是简单的偏移
    - 相比页式管理意图用于管理虚拟存储器系统，段式管理更多聚焦于为多道程序提供相互独立的虚拟地址空间
    - 其缺点是会导致较多的内存碎片和跨页段导致无法与虚拟存储器协同工作
    - 段式管理同样需要一个段表来查询段号对应的偏移量
  - 段式与页式的协同工作方式是：先分页，再在页内分段