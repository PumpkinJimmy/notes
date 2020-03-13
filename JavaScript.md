## JavaScript
### 获取尺寸
- `ele.clientHeight` `ele.clientWidth` 含边框的宽和高
- `ele.offsetHeight` `ele.offsetWidth` 不含边框的宽和高
- `ele.scrollHeight` `ele.scrollWidth` 连带上超出视口范围需要滚动才能看到的部分的全体的宽和高
- 可以把`ele`替换成`document.body`来获取整个页面的尺寸（陷阱：**浮动脱离文档流**)
### 获取定位信息
- `ele.offsetTop` `ele.offsetLeft`
- `pageXOffset` `pageYOffset`
### 杂项
- 获取计算出来的样式值：`getComputedStyle(ele)`，然后当作`ele.style`来使用就可以了