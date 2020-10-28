## Nginx Note

### 坑
- `root` vs. `alias`

  这两个指令都用在location环境里面，且都用于转发路径，但行为有区别：

  ```nginx
  location /static/images/ {
      root /home/ubuntu; # (注意无斜杠)
  } # /static/images/i.gif -> /home/ubuntu/static/images/i.gif 
  location /static/images/ {
      alias /home/ubuntu/static/images/; # （注意有斜杠）
  } # /static/images/i.gif -> /home/ubuntu/static/images/i.gif
  ```