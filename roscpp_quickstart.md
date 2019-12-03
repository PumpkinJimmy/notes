## ROS C++ 速查手册
1. 创建工作空间
   ```shell
   mkdir catkin_ws
   cd catkin_ws
   mkdir src
   catkin_make
   source devel/setup.bash
   ```
   由此配置好工作环境
2. 创建package
   在workspace根目录下
   ```shell
   cd src
   catkin_create_pkg pkg
   cd ..
   catkin_make
   ```
   注意必须在workspace工作目录下执行catkin_make
3. 写东西
   在package的目录下创建include, src等等，在文件夹里放东西，便于管理
4. C++写节点
   ```cpp
   #include "ros/ros.h"
   int main(int argc, char** argv)
   {
   	ros::init(argc, argv, "NodeName");
	ros::NodeHandle nh;
	nh.spin();
   }
   ```
5. 编译
   1. 修改CMakeLists.txt
      - 开include_directories
      - 写find_package（常用的有roscpp, rospy, std_msgs, tf，自定义消息加message_runtime, message_generation）
      - add_executable(tg src)
      - tar_link_libraries
   2. 去workspace根目录catkin_make
6. 跑
   roscore
   rosrun pkg executable

