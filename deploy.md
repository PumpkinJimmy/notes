## 部署
### Django + uwsgi + Nginx
- Nginx作为web服务器，它承担的任务：是接受Web请求、自动处理静态文件、转发动态请求到后端Web应用程序、负载均衡
- Nginx在负责转发请求到后端是担当的是“反向代理”（对客户端透明，充当服务器的代理）
- Nginx无法直接与Python通信，需要uwsgi来担当Nginx与Django之间的桥梁。uwsgi将低效的HTTP请求转化为高效的内部二进制请求，然后利用WSGI协议与Django通信
- uwsgi通过运行一个服务来监听来自Nginx的请求，通信方式可以是常规HTTP，也可以是Unix Socket
#### 步骤
1. 配置Django
   - 在`settings.py`中关闭Debug模式
   - 在`settings.py`中设置`ALLOW_HOST`来允许反向代理
   - 收集静态文件
     - 开发阶段，静态文件请求由Django测试服务器处理；在生产环境中，静态文件交给Nginx处理，故需要收集静态文件到合适的位置
     - 先在`settings.py`中设置`STATIC_ROOT`，指定静态文件存放的位置
     - 运行`manage.py collectstatic`
2. 配置uwsgi
   - 安装uwsgi的python插件：`sudo apt install uwsgi-plugin-python3`
   - 编写配置文件（以ini配置文件为例）
     一个实例：
     ```ini
     # mysite_uwsgi.ini file
     [uwsgi]
     # Django-related settings
     # the base directory (full path)
     chdir           = /home/ubuntu/mysite
     module          = mysite.wsgi

     # process-related settings
     # eable master process
     master          = True
     # maximum number of worker processes
     processes       = 10
     # the socket (use the full path to be safe
     socket          = /home/ubuntu/blogsite/blogsite.sock
     # with appropriate permissions
     # 664 may be more safe but user group config required
     chmod-socket    = 666
     # clear environment on exit
     vacuum          = true

     # save the pid of the master (for operating the uwsgi daemon)
     pidfile = blogsite_uwsgi.pid

     # set log
     daemonize = blogsite_uwsgi.log

     # python plugin
     plugins = python3
     ```
   - 运行uwsgi（以ini配置文件为例）
     `uwsgi --ini blogsite_uwsgi.ini`
     后台运行：
     `uwsgi -d --ini blogsite_uwsgi.ini`
     停止服务：
     `uwsgi --stop blogsite_uwsgi.pid`
     重载：
     `uwsgi --reload blogsite_uwsgi.pid`
3. 配置Nginx
   - 在`/etc/nginx/site-available`中编写`blogsite.conf`
     ```nginx
     # blogsite_nginx.conf

     # the upstream component nginx needs to connect to
     upstream django {
        server unix:///home/ubuntu/blogsite/blogsite.sock; # for a file socket
        # server 127.0.0.1:8001; # for a web port socket (we'll use this first)
     }

     # configuration of the server
     server {
        # the port your site will be served on
        listen      8000;
        # the domain name it will serve for
        server_name 111.229.242.198; # substitute your machine's IP address or FQDN
        charset     utf-8;

        # max upload size
        client_max_body_size 75M;   # adjust to taste

        # Django media
        location /media  {
            alias /home/ubuntu/blogsite/media;  # your Django project's media files - amend as required
        }

        location /static {
            alias /home/ubuntu/blogsite/static; # your Django project's static files - amend as required
        }

        # Finally, send all non-media requests to the Django server.
        location / {
            uwsgi_pass  django;
            include     /etc/nginx/uwsgi_params; # the uwsgi_params file you installed
        }
     }
     ```
   - 链接到`/etc/nginx/site-enable`: `ln -s mysite.conf /etc/nginx/site-enable`（必须用绝对路径）
   - 检查配置文件是否正确： `sudo service nginx configtest`
   - 重启服务： `sudo service nginx restart`