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



## 3. Arithmetic
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


