## RTMP直播
### Quick Start
1. 安装nginx-rtmp-module，配置nginx.conf：
   ```
   # nginx.conf
   rtmp{
       server{
           listen 1935;
           application myapp{
               live on;
           }
       }
   }
   ```
2. 调用ffmpeg进行推流（此处推的是摄像头，其中`mystream`是可以自己起的名字）
   
   `ffmpeg -i /dev/video0 -f flv rtmp://127.0.0.1   myapp/mystream`
3. 使用ffplay拉流播放
   
   `ffplay rtmp://127.0.0.1/myapp/mystream`

