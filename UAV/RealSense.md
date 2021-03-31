## 使用Realsense-ROS
1. `source ~/catkin_ws/devel/setup.bash` （因为realsense2_camera没有安装到系统目录）
2. `roslaunch realsense2_camera rs_camera.launch`（启动相机节点）
   - 为了使用Aligned Depth，需要使用另一个launch文件，或者使用参数（详见官网实例）
   - 为了启用PointCloud，需要编写launch文件设置`enable_pointcloud=true`，或者传参`filter:=pointcloud`（详见官网示例）
   - 启用点云后，订阅`/camera/depth/color/points`来获得格式为`sensor_msg/PointCloud2`的点云数据