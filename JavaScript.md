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

### REST API 跨域访问
- 通常我们会在JavaScript里面使用AJAX访问REST API，此时会涉及域名A下的JavaScript代码访问域名B下的API。这种行为被称为CORS(Cross Origin Resource Sharing)
- 默认情况下CORS是**被禁止的**
- API的提供者能通过在使用`Access-Control-Allow-Origin`头来指定哪些域名能跨域请求API资源