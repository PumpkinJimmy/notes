## Python爬虫从入门到入土
### urllib
这个标准库是一个基础的库。常用：
- `urllib.parse`
  实用工具包含`urlsplit`分解URL，`urljoin`连接，`quote`URL编码，`unquote`URL解码
- `urllib.request`
  `urlopen`爬虫HelloWorld,`urlretrieve`下载东西

### requests
比`urllib.request`更高层次抽象的http客户端，更好用
*不要漏s*

`requests.get(url)`系列是主要的工具：
- 有`get(),post(),head(),option()`等，对应HTTP的`GET,POST,HEAD,OPTION`等方法。
- 必须的参数只有请求的url
- `headers=`指定请求头
- `cookies=`指定cookie，注意所需的结构是键-值对应的字典
- `encoding=`指定编码
- `json=`指定请求的json的内容（会自动处理转换和`Content-Type`）
- 返回一个Response

Response:
- `resp.text`代表返回的内容的`str`，一般能根据响应头部来正确解码，用于文本数据；`resp.content`代表返回的二进制数据（即形式解码）
- `resp.headers`响应头部
- `resp.status_code`状态码
- `resp.json`返回的json

### bs4.BeautifulSoup
非常方便的HTML DOM解析
- 直接使用点记法访问子节点（名字采用子节点的标签名）
- 字典记法访问attribute
- `soup.select(selector)/soup.select_one(selector)`通吃的工具，使用CSS选择器选择元素，前者返回一个列表包含所有符合条件的节点，后者只返回其中一个
- `tag.text`访问Tag内部的所有文本（不靠谱）
- `tag.children()`子节点迭代器

### selenium.webdriver
爬虫的最终兵器，直接调用webdriver来运行整个页面，可以对付高度动态的页面，详见其他笔记。