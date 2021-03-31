# ELF文件
## 使用readelf
```bash
readelf -h # elf header
readelf -S # Section Header Table
readelf -s # Symbol Table
readelf -l # Program Table
readelf -r # Relocatable Table
```

## ELF文件结构
ELF = Executable and Linkable File 是一种可链接可执行文件格式，可以在Linux下执行

ELF由如下四部分组成：
- ELF Header
  
  ELF文件头包含各种元信息，比如：
  - 机器架构
  - 端格式
  - ABI
  - Type (Relocatable/Executable/Static Lib/Dynamic Lib)
  - **Program Header Table Offset, Program Header Table Size**
  - **Section Header Table Offset, Section Header Table Size**
  - 等

- Program Headers
  
  用于**指导Loader创建进程映像**，对于Relocatable（Linker的输入）文件是可选的

- Sections
  
  文件主体，包括所有的代码端、数据段、Symbol Table等

- Section Headers

  用于**指导Linker生成Executable**，对于Executable是可选的

## 使用objdump
```shell
objdump -d # 反汇编所有可执行节区
objdump -x # 显示所有内容的16进制信息
```