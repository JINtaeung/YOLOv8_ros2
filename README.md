<div align="center">

# YOLOv8 with ROS2

![Ubuntu 20.04](https://img.shields.io/badge/Ubuntu-20.04-blue?style=flat-square&logo=Ubuntu&logoColor=FFFFFF)
![Ros foxy](https://img.shields.io/badge/Ros-foxy-blue?style=flat-square&logo=ROS)
![Python 3.8.10](https://img.shields.io/badge/Python-3.8.10-blue?style=flat-square&logo=Python&logoColor=FFFFFF)

</div>

<font size=2>

> **Note** <br>
> This project if forked from <br>
> [ultralytics/ultralytics](https://github.com/ultralytics/ultralytics) <br>
> [mgonzs13/yolov8_ros](https://github.com/mgonzs13/yolov8_ros)

</font>

<font size=2>

## :rocket: install
Following ROS packages are required:
- [geometry_msgs](https://docs.ros2.org/latest/api/geometry_msgs/index-msg.html)

- [vision_msgs](https://github.com/ros-perception/vision_msgs)

Following usb_cam packages are option for test:
- [usb_cam](https://github.com/ros-drivers/usb_cam/tree/ros2)

Clone the repo into your catkin workspace and build the package:
```shell
git clone https://github.com/JINtaeung/YOLOv8_ros2
pip3 install -r YOLOv8_ros2/requirements.txt
```

Build
```shell
cd YOLOv8_ros2
rosdep install --from-paths src --ignore-src -y -r
colcon build
echo "source ~/YOLOv8_ros2/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## :clipboard: Usage
Before you launch the node, adjust the parameters in the [launch file](https://github.com/JINtaeung/YOLOv8_ros2/blob/main/yolov8_bringup/launch/yolov8.launch.py).

For example, you need to set the path to your YOLOv7 weights and the image topic to which this node should listen to.   
The launch file also contains a description for each parameter.   

- [launch/yolov7.launch](https://github.com/JINtaeung/YOLOv8_ros2/blob/main/yolov8_bringup/launch/yolov8.launch.py) for developer
1. params ="model" value="사용할 가중치 파일(heavy: [yolov8n.pt] < yolov8n.pt < yolov8m.pt < yolov8l.pt < yolov8x.pt)"
2. params ="device" value="cuda:0" or "cpu"
3. topics ="img_topic" value="subscribe할 rostopic 경로"
- 값 변경 후 다시 build
```shell
cd ~/YOLOv8_ros2
colcon build
```

## :white_check_mark: Test
- usb_cam 실행
```shell
ros2 launch usb_cam demo_launch.py
```
- YOLOv8 실행
```shell
ros2 launch yolov8_bringup yolov8.launch.py
```

## :satellite: Output Rostopic
- Center pose of Bounding box
```shell
ros2 topic echo /yolo/center
```
- [yolov8_bringup/yolov8.launch.py](https://github.com/JINtaeung/YOLOv8_ros2/blob/main/yolov8_bringup/launch/yolov8.launch.py) input_image_topic = "`/image_raw`" ➡️ subscribe topic name
- [yolov8_ros/yolov8_node.py](https://github.com/JINtaeung/YOLOv8_ros2/blob/main/yolov8_ros/yolov8_ros/yolov8_node.py) topics:self._dbg_pub=self.create_publisher = "`result`" ➡️ publish topic name
- Detection using the [vision_msgs/Detection2D](https://docs.ros.org/en/api/vision_msgs/html/msg/Detection2D.html) message type.
- Center pose using the [geometry_msgs/Point32](https://docs.ros2.org/latest/api/geometry_msgs/msg/Point32.html) message type.
