## Ncurses
一个帮助在终端文本模式下开发UI的库，一个可以压榨终端全部潜能的库。
核心思想：将终端界面当成是一个坐标系，以一个字符的空间作为一个最小处理单元来绘图，可以响应键盘与鼠标。
### Quick Start
头文件ncurses.h
```cpp
initscr(); // 初始化Ncurses
cbreak(); // 关闭行缓冲，每个字符立刻读取
noecho(); // 关闭输入回显
keypad(stdscr, TRUE); // 可以处理非打印字符输入
start_color(); // 启动彩色功能
/* 在这搞事 */
endwin(); // 退出Ncurses
```
### 基本使用
- `cbreak()` `raw()` 用于关闭行缓冲，区别是cbreak自动处理ctrl+c输入中断程序，而raw不会
- `clear()` 清屏
- `addch` `printw` ncurses版的putchar和printf
- `getch` `scanw` ncurses版的getchar和scanf，**注意，由于要处理额外信息如方向键，返回类型不是char而是chtype**
- mv系列表示移动到指定位置再操作，如`mvaddch` `mvprintw`用于移动到指定位置再输出，**注意，坐标是（行数，列数）的形式**
  测试1
  测试2：一段话
- `attron` `attroff` 用于开关特性，特性包括粗体字、不可见、闪烁字、反色、彩色等
- `chgat` 改变后续若干字符特性和颜色，-1表示改变一整行，有对应的mv版本`mvchgat`
- `curs_set(0)` 隐藏光标
- `refresh` 拥有刷新缓冲区（没错ncurses是双缓冲的）

### 彩色
- 一共八种颜色，统一格式是COLOR_开头
- 使用`init_color(colorid, r, g, b)`来改变具体配色，但必须用那八种，也就是屏幕上最多同时只能由八种颜色，且rgb值的范围是**0到1000**
- 由于终端的特性，颜色总是成对使用，用`init_pair(pairid, fgcolor, bgcolor)`配置特定的pair
- `COLOR_PAIR(pairid)`是一种特性，可以结合特性相关函数使用

### 窗口
重要抽象，通过把一定区域抽象为一个*窗口(window)*，可以只刷新部分屏幕来加快速度，且允许使用相对坐标系
- `WINDOW`结构体用于表示窗口
- `newwin(height, width, row, col) -> WINDOW*` 新建窗口
- `delwin(win)` 回收窗口**注意窗口只是逻辑上的，delwin不能清空window里的内容**
- stdscr是初始自动创建的窗口，也是所有函数默认作用的窗口
- w系列的函数需要指定作用的窗口，如`waddch` `wprintw` `wrefresh`，可以与mv组合得到mvw系列的函数如`mvwaddch`
- `box(win, v, h)` 为窗口绘制外框，可以指定水平与垂直线使用的字符（默认是`ACS_VLINE` `ACS_HLINE`)
- `wborder(win, ...)` 详细指定绘制的边框（可以指定四个角落和四边用的字符）

### 杂项
- `ACS_`开头的字符利用的是ascii拓展字符集，可以用来画一些很炫的东西（如严格的无缝隙的表格，Pi）
- `KEY_LEFT`表示左箭头，其他箭头以此类推（要开keypad）
- `KEY_F(n)`表示F1～F12的控制键（要开keypad）

### 坑点
- 使用自定义的窗口前麻烦先refresh一下（不知道为什么）
- 由于要处理额外的信息（颜色、特殊输入等），getch系列得到的都是chtype（比char要宽）而不是char
