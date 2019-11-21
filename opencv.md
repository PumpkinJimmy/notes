## OpenCV笔记
### 基本安装
参见[OpenCV官网的教程](https://docs.opencv.org/4.1.2/d7/d9f/tutorial_linux_install.html)
两种方式：APT或源码编译安装
APT胜在简单，但不一定安装到最新版
下面总结源码安装大致步骤
1. git clone
2. 进入目录，新建build文件夹，然后cmake ..
   可以指定编译选项
   CMAKE_BUILD_TYPE=Release
   CMAKE_INSTALL_PREFIX=/usr/local
   OPENCV_GENERATE_PKGCONFIG=ON
   选项顾名思义，每个-D后面跟一个选项
3. make -j7 开始正式编译，-j7表示启用7个线程同时编译（小心炸鸡内存不够）
4. sudo make install

### 编译
- 编译时包含opencv的包含路径，注意文件夹“opencv2”指的是第二代API，不是OpenCV2
- 链接时使用-L和-l，链接库的名字就是opencv_ + 模块名，-L给出OpenCV库目录
### 基本操作
- imread 读取图像
- imshow 显示图像
- waitKey 等待按键
- namedWindow 开窗口
- imwrite 保存图像
- Mat.at<*type*> 访问像素
