## ROS笔记2(test)
1. 指令
   rostopic
   - rostopic echo *topic* 显示topic收到的信息
   - rostopic list [-v] 列出所有现有topic
   - rostopic pub -1 *topic* *msg_type* *data* 向topic发送一次类型为msg_type的数据data
   - rostopic pub -r 1 *msg_type* *data* 向topic以1Hz发送类型为msg_type的数据data
   - rostopic type *topic* 显示topic的消息类型
2. Publisher
   ```cpp
   ros::Publisher pub = nh.advertise<msg_type>(topic_name, queue_size);
   msg_type msg;
   pub.publish(msg)
   ```
   ros::Rate用于sleep节点以控制publish频率
3. Sbuscriber
   ```cpp
   ros::Subscriber sub = nh.subscribe(topic, queue_size, callback);
   ros::spin() //开始等待消息的循环
   ```
   (callback要求接受一个msg_type::ConstPtr类型的msg)
   Subscribeer析构时自动停止订阅
