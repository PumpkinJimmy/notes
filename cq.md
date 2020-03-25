# 酷Q框架笔记
酷Q是一个QQ机器人的应用框架，提供大量API与QQ交互，且提供多个语言的插件开发SDK
## CPPSDK
### 引入
（待完善）
### QuickStart
（待完善）
### 使用
（待完善）
### 杂项
- 项目构建工具是CMake，可以很好地引入支持CMake的库
- 这个库的特点是文档约等于不存在，但注释还可以，看接口直接翻代码就可以了
- 常翻看的代码主要是`event.cpp` `api.cpp` `event_callback.cpp`
- 这个库的很多调用其实是对CoolQ API的封装

## cqhttp
这是一个酷Q插件，使得用户可以通过HTTP/WebSocket与酷Q通信，从而给出语言无关、平台无关的通用的API。
`cqhttp`支持HTTP通信（类似常见的HTTP API）、WebSocket通信（适用于JavaScript浏览器脚本）、WebSocket反向代理（插件充当客户端，主动去访问处理业务逻辑的服务）
