## Verilog笔记
Verilog是一种*硬件描述语言*（HDL）。它与常规编程语言的不同在于它往往需要将代码与底层的逻辑门电路对应起来。

### QuickStart
```Verilog
// 跑马灯程序
module race_led(
input CLK,
input SW_in,
output reg[15:0] led); 
reg [26:0] count; 
parameter T1MS=50000;
initial begin
    led<=16'b1;
end
always@(posedge CLK) 
begin 
    count<=count+1;
    if (count==300*T1MS)
    begin
        count<=0;
        if (SW_in==0)
        begin
            led <= led << 1;
            if (led == 16'b1000_0000_0000_0000)
            begin
                led<=16'b1;
            end
        end
        else
        begin
            led <= led >> 1;
            if (led == 16'b1)
            begin
                led<=16'b1000_0000_0000_0000;
            end
        end
    end
end
endmodule
```

### Verilog三种编写方式
1. 使用*原语*描述逻辑门器件的连接
   
   `and u1(out,a,b);`

   `not #1 u2(out,in);`

   使用代码直接定义了一些逻辑门器件及它们的端口连接

   注：`#1`表示时序上延迟一个单位的时间。因为这些器件虽然描述顺序有先后，但它们本身没有前后关系。

2. 使用assign描述组合逻辑
   
   `assign out = select==1?b:a;`

   类似这样的赋值语句描述了一个二路选择器元件。

   这种描述方式用常规编程的表达式来描述功能逻辑，常用于表达组合逻辑

   可以采用连续赋值：

   `assign {cout,sum} = a + b + cin` 描述了带进位的加法器逻辑

3. 使用always块
   
   ```Verilog
   always @(posedge CLK) begin
    // do someting
   end
   ```

   与时钟、时序相关的逻辑可以写在alwasy块里面。

   在always块里面：
   - 可以使用形如`if,else`等分支语句
   - 可以使用`a<=b`执行非阻塞赋值
   - 可以使用`# sometime`提供延迟
   - 可以使用各种表达式进行运算
   - **（注意）**一个变量不能在多个`always`块里面被更改
   - **（注意）**多个`alwasy`块是**并行**运行的，没有先后顺序

    其中，在`always`块中编写是最接近一般编程的。

    但如果要写入到FPGA硬件中执行，三种编写方式都需要转化成第一种形式（即门的描述）。这一转化过程称为*综合（synthesis)*

### Module
（待完善）