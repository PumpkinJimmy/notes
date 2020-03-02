## CSS笔记
### 布局
- 传统盒子模型
  - 行为容易控制
  - 不够灵活
- table
  - 用于对齐大量元素
- float
  - 想用position的时候用
  - 行为不容易掌握，在最顶层就要设计好
  - 灵活
### 动画
简单动画用transform变形移位+transition过渡
复杂的用keyframes+animation
- transform: translate/rotate/scale
- keyframes:
  - 不要漏s！
  - 不要手多from后面加冒号
  - 记住keyframes有问题是不会让animation报错的！
  - 格式
    ```
    @keyframes name
    {
    from { ... }
    to { ... }
    }
    ```
- animation最秀的是可以给出动画速度曲线，如ease（默认），ease-in
### 杂项
- display:inline之后仍然可以设置边距
- 水平居中：
  - text-align: center 文字居中
  - width: ..px; margin: 0 auto 定宽的元素居中，用于按钮一类的块级元素
- 垂直居中：
  - line-height: 100% 常用于文本
  - display: table-cell; vertical-align:middle 通用的行内垂直居中（如图片）
- 引用远程字体：@font-face
- `:before``:after`伪类选择器可以配合`content`属性插入内容
### 坑点
常见坑点:
- width属性对应**内容区**的宽度而非框的宽度（即不含内边距）
### Font Awesome
一个简单易用的图标库。原理是自定义字体+CSS。

用法：
- 引入：除了直接用CDN，可以离线使用，注意需要一个CSS文件和一堆字体文件。注意存放的路径，具体请自己看/修改CSS文件开头引用字体文件的路径
- 基本用法：`<i class="fa fa-iconname"></i>`
- `fa-lg` `fa-1x` `fa-2x` `fa-3x` `fa-4x` `fa5x` 都是用来放大图标的
- `fa-fw`使图标在一个固定宽度中，用于对齐一些大小不同的图标
- `fa-li`用于替换默认列表图标
- `fa-spin`使图标变成旋转动画（CSS3），常配合`fa-spinner``fa-refresh``fa-cog`使用
- `fa-rotate-*``fa-flip-*`用于旋转/翻转图标
