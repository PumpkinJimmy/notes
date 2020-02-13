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
### 坑点
常见坑点:
- width属性对应**内容区**的宽度而非框的宽度
