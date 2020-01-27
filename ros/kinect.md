## 3D摄像头Kinect
这款微软出品的摄像头可以看2D图像的同时借助红外检测深度，得到3D视觉。
ros有现成的驱动
步骤
1. 连接摄像头的usb，确认设备安装成功
2. 安装驱动：freenect（新）或者openni
3. 启动turtlebot
4. 启动freenect或openni来启动大批摄像头相关节点
5. 订阅得到图像和深度数据（如/camera/rgb/image_color）
6. 显示图像，如image_view包、rviz（使用工具的话与上一步是一起的）
