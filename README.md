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

Clone the repo into your catkin workspace and build the package:
```shell
mkdir -p yolov8_ws/src
cd yolov8_ws/src
git clone https://github.com/JINtaeung/YOLOv8_ros2
pip3 install -r YOLOv8_ros2/requirements.txt
```

Following ROS packages are required:
- [geometry_msgs](http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Point32.html)
```shell
sudo apt install python3-geometry-msgs -y
```

- [vision_msgs](http://wiki.ros.org/vision_msgs)
```shell
cd yolov8_ws/src/
git clone https://github.com/ros-perception/vision_msgs.git
```

Download usb_cam packages are option:
- [usb_cam](https://github.com/ros-drivers/usb_cam/tree/ros2)
```shell
cd yolov8_ws/src/
git clone -b ros2 --single-branch https://github.com/ros-drivers/usb_cam.git
```

Build
```shell
cd ~/yolov8_ws
rosdep install --from-paths src --ignore-src -y -r
colcon build
echo "source ~/yolov8_ws/install/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## :clipboard: Usage
Before you launch the node, adjust the parameters in the [launch file](launch/yolov7.launch).   
For example, you need to set the path to your YOLOv7 weights and the image topic to which this node should listen to.   
The launch file also contains a description for each parameter.   

- [yolov8_ros/yolov8_node.py](https://github.com/JINtaeung/YOLOv8_ros2/blob/main/yolov8_ros/yolov8_ros/yolov8_node.py) for developer
1. params ="model" value="사용할 가중치 파일"
2. params ="device" value="cuda:0" or "cpu"
3. topics ="img_topic" value="subscribe할 rostopic 경로"
- 값 변경 후 다시 build
```shell
cd ~/yolov8_ws
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
- Center pose using the [geometry_msgs/Point32](http://docs.ros.org/en/melodic/api/geometry_msgs/html/msg/Point32.html) message type.
