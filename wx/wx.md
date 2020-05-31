## 微信小程序
### 访问dataset
```html
<view data-id="001"></view>
```
```js
var id = evt.currentTarget.dataset.id; // 也就是访问组件的dataset属性
```
### 坑点
- wxss只支持基本的选择器（类、ID、子、后代、部分伪类），不支持通配符
- wxml不支持html5的`<svg>`标签
- wxss的`@import`引入的文件的后缀名必须是`.wxss`（也就是`.css`不行）
- wxss的`font-face`不允许引入本地/远程字体，但支持base64的字体
  
  若以想用自定义字体（尤其是字体图标），必须用[字体转换](https://transfonter.org/)网站的功能将原来的`.ttf`转换成base64
- 区别`evt.target`/`evt.currentTarget`
  
  由于小程序组件的事件响应是“冒泡”的，也就是说子组件触发的事件会顺着向其父组件传递，触发父组件绑定的事件处理器

  于是，`evt.target`表示原始触发事件的组件，`evt.currentTarget`指当前正在处理事件的组件。常见的坑点是页面上一个复合的元素主体绑定了事件，但用户可以点击其后代元素，则使用`evt.target`往往会出问题