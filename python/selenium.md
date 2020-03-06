## Selenium
爬虫的最终兵器，直接调用webdriver来运行整个页面，可以对付高度动态的页面
### 安装
你需要一个浏览器本体（Chrome,Firefox都可以），以及一个webdriver（另外下载，注意要对应版本）
然后保证浏览器本体是找得到的

### Quick Start
（以Chrome为例）
```python
from selenium import webdriver
options = webdriver.ChromeOptions()
# Headless模式，即不打开实际的浏览器窗口，调试的时候可以不要这一条
options.add_argument("--headless") 
dr = webdriver.Chrome(executable_path="your/driver/path/", options=options)

dr.get("https://yourtarget/") # 请求页面，不可以省略协议
img = dr.find_element_by_tag_name("img")
print(img.get_atrribute('src'))
dr.close()
```

### 基本使用，DOM访问
- `dr.get(url)`打开页面；`dr.close()`关闭浏览器；`dr.refresh()`刷新页面
- `dr.page_source`页面源代码
- `find_element_by_*/find_elements_by_*`系列用于访问DOM，区别在于前者返回一个，后者返回列表（若找不到则返回空列表）
  - 常规操作：`find_element_by_class_name/ _tag_name / _id / _css_selector`
  - 比较特别，通过链接文字来匹配：`find_element_by_link_text/find_element_by_partial_link_text`
  - XPath也可以：`find_element_by_xpath`
- 节点的常用属性：
  - `tag.location` tag在屏幕中的位置
  - `tag.text` tag内的所有文本

### Cookie
在需要模拟登录的情景下操作Cookie非常重要
- `dr.get_cookies()`返回所有cookie；`dr.get_cookie(key)`返回指定的cookie；`dr.add_cookie(cookie_dict)`添加cookie；注意cookie的数据结构是形如：
  ```json
  {
      "name": "login_ever",
      "value": "yes",
      "expire": 1232.3253
  }
  ```
  的字典，且**不同浏览器的key还不同（非常坑）**，必要时直接删掉不正确的key就行了

### 模拟交互
Selenium最厉害的地方就在与其可以模拟一些用户的UI操作
- `tag.click()`模拟点击；`tag.submit()`在表单中执行submit操作
- `tag.send_keys()`发送模拟按键的操作
- `tag.clear()`清空（通常是input）

### 执行JS代码
默认webdriver会正确运行页面的代码
- `dr.exeute_script(code)`直接运行JavaScript代码

### wait机制（待补充）
三种等待机制实现正确的等待、阻塞操作
1. 标准库time.sleep（少用）
2. 隐式等待，指在每一个需要等待的操作中阻塞至多某个时间
3. 显式等待，指在某需要操作中至多等待某个事件

### 杂项
- 某些页面被设计为只有进入窗口才加载资源，解决方案是把元素“滚动”到窗口内：
  `dr.exec_script('window.scrollIntoView(arguments[0])', tag)`
  （也就是用JavaScirpt BOM曲线救国）