## 部署
### Django + uwsgi + Nginx
- Nginx作为web服务器，它承担的任务：是接受Web请求、自动处理静态文件、转发动态请求到后端Web应用程序、负载均衡
- Nginx在负责转发请求到后端是担当的是“反向代理”（对客户端透明，充当服务器的代理）
- Nginx无法直接与Python通信，需要uwsgi来担当Nginx与Django之间的桥梁。uwsgi将低效的HTTP请求转化为高效的内部二进制请求，然后利用WSGI协议与Django通信
- uwsgi通过运行一个服务来监听来自Nginx的请求，通信方式可以是常规HTTP，也可以是Unix Socket
#### 步骤
1. 配置Django
   - 在`settings.py`中关闭Debug模式
   - 收集静态文件
     - 开发阶段，静态文件请求由Django测试服务器处理；在生产环境中，静态文件交给Nginx处理，故需要收集静态文件到合适的位置
     - 先在`settings.py`中设置`STATIC_ROOT`，指定静态文件存放的位置
     - 运行`manage.py collectstatic`
2. 配置uwsgi
   - 编写配置文件（以ini配置文件为例）
     （待完善）
   - 运行uwsgi（以ini配置文件为例）
     `uwsgi --ini mysite_uwsgi.ini`
3. 配置Nginx
   - 在`/etc/nginx/site-available`中编写`mysite.conf`
     （待完善）
   - 链接到`/etc/nginx/site-enable`: `ln -s mysite.conf /etc/nginx/site-enable`
   - 检查配置文件是否正确： `sudo service nginx configtest`
   - 重启服务： `sudo service nginx restart`