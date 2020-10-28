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
- flex
  - 真正的现代布局方案，非常灵活
  - 旧式浏览器完全不兼容，主要用在手机端上
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
- 动画只是特效，最终的样式还必须要在样式表里给出

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
常见坑
- 默认情况下，width/height属性对应**内容区**的尺寸而非框的尺寸（即不含内边距）
- `keyframe`的初始样式和结束样式都是暂时的，只要动画播放完，元素的样式会回归原来的样子。故如果要动画结束后停留在结束样式，必需相应用js修改元素的样式。
- `display`属性不可以应用过渡动画

- 伪类选择器
  
  区分以下两种写法：
  ```css
  ul:first-child /* ul作为第一个孩子时的样式 */
  ul>:first-child /* ul的第一个孩子的样式*/
  ul :first-child /* ul的所有后代的第一个孩子的样式*/
  ```

  谨记！！css选择器里的空格表示**后代**

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

### Flex布局
Flex布局是真正的现代布局方案，几乎可以完全替代table,float,position
Flex布局的专长是**自动对齐，自适应分配空间**，这之外不一定比传统的好
传统布局的不可替代特性：
- table：显示一个复杂的大表格
- float：文字环绕图片一类的独特效果
- position：相对于视口定位，即position:fixed

#### Quick Start:自动均分div
```css
/* flex.css */
.container{
  display: flex;
  height: 200px;
  width: 300px;
}
.item{
  flex-grow: 1;
}
```
```html
<!--flex.html-->
<div class="container">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
```
如此，每个item都自动占有container的1/3宽度

#### 理论
本质上，flex布局就是传统桌面应用里的Sizer，设置主轴，明确元素的原始的盒模型尺寸，结合对齐方案和空间分配方案来定位、resize其中的元素。然后通过flex的嵌套来实现复杂的布局。

主轴要么水平，要么垂直（其中水平flex可以替代float了）

对齐策略指明了剩余空间如何分配到**元素之间**，空间分配策略指明空间如何分配给**元素本身**，也就是说如果这两条路是有重叠的地方的

对齐策略：如justify-content,align-items分别指明了元素在主轴和交叉轴上的对齐方式

空间分配策略：如flex-grow指定元素占有空间的配比

flex布局的有点是灵活、自适应、较直观：
- flex布局往往不直接指定布局的具体尺寸，只给出布局的方案，使其在不同的屏幕上都可以工作，具有一定的自适应性；在修改增减元素之后依然可以合理布局元素，非常适合动态页面
- flex布局是从整体出发描述布局，不涉及复杂的块格式化，相比float必须小心地考虑脱离文档流的块的行为间接迂回地布局显然更直观更好写

#### 语法
Flex容器属性：
- `display:flex` 指定元素为*flex容器*，它的子元素将会是*flex元素*
- `flex-direction` 指定主轴方向，常用的就是`row` `column`
- `justify-content` 指定主轴上元素的对齐方案
  - `flex-start` 左对齐，后面不留间隔地排；`flex-end` 类似，只是右对齐
  - `center` 居中
  - `space-between` 两端对齐（首元素置于其实位置，末元素置于结束位置），然后将剩余空间平均分到元素之间。这个策略很适合占满容器宽度的双列表格
  - `space-evenly` `space-around` 都是将剩余空间分配到元素之间、以及首元素与首位置之间、末元素与末位置之间，但比例不同。这两个策略很适合将等宽的元素平铺开来的情形
  - 注意到`justify-content`系列都是分配剩余空间到元素之间，但如果flex元素设置了空间配比的属性，则可能*没有剩余空间*，则这五个属性没有区别
- `align-items` 指定元素在交叉轴上的对齐策略
  - `flex-start` `flex-end` 
  - `center` 局中（常用）

Flex元素属性：
- `flex-grow` 指定元素分配剩余空间的配比
  - 默认值是0，表示参与不分配
  - 指定正整数表示分得比例，例如：所有元素都是1则大家均分剩余的空间
- `flex-shrink` 在容器空间不足的情况下元素收缩的比例
  - 注意在`flex-wrap:nowrap`也就是不换行的情况下才有可能用到
  - 默认值为1，即等比例收缩
  - 常见设置为0，即该元素不收缩，这样可以让该元素的尺寸应用其width/height属性而不是接受分配
  
注：
- *主轴*指的是元素在用其中排列的方向，*交叉轴*指的是与主轴垂直的轴；主轴可以是水平或垂直，可以多行，可以反向
- flex元素的尺寸最终由flex布局策略决定，视情况需要可以拉伸和收缩，但元素有一个基本尺寸，从盒模型参数和内容尺寸计算得到
- *剩余空间*指的是容器尺寸减去元素基本尺寸之后剩余的空间
- *不留间隔*指的是元素的**盒模型之间不留间隔**，不是说元素与元素之间没有内外边距
- flex容器操作的直接对象是其*元素盒模型及位置*，如果布局结果中元素盒模型不能合理地容纳元素的子元素，则元素的子元素可能出框
- 在某些布局策略下，flex元素本身也有可能超出flex容器的框

#### 妙用
均分：
```css
.container{
  display: flex;
}
.item{
  flex-grow: 1;
}
```

水平居中且垂直居中
```css
.container{
  display: flex;
  justify-content: center;
  align-items: center;
}
```

主题元素分得所有剩余空间
```css
.container{
  display: flex;
  flex-direction: column;
}

.header{
  height: 20%;
}
.main{
  flex-grow: 1;
}
```

图片轮播
```css
/* 需要结合JS代码，通过改变translateX来移动 */
/* transition使得具有了无论怎么移动都可以有合适的滑动动画 */
.slide-container{
  width: 100%;
  overflow: hidden;
}
.slide{
  display: flex;
  flex-wrap: nowrap;
  width: 100%;
  transform: translateX(0);
  transition: transform 2s;
}
.slide img{
  display:block;
  width: 100%;
  flex-shrink: 0;
}
```

### box-sizing
`box-sizing`属性旨在解决传统CSS盒模型尺寸设置的困境：`width` `height`设置的是*内容区*的宽度，而内容区的宽度是不含内边距的，没有合适的方法直接指定盒子的尺寸——在需要使用边框和背景色的元素上问题尤为明显。通过指定`box-sizing: border-box`，`width` `height` 可以指定元素的盒子尺寸