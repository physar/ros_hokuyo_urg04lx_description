<img src=https://www.hokuyo-aut.jp/assets/img/common/h_logo_fx.png alt="Hokuyo">

# ros_hokuyo_urg04lx_description

This unofficial package creates a ros node which  publishes the shape and coordinate frames of [the Hokoyu URG04LX laser scanner](https://www.hokuyo-aut.jp/search/single.php?serial=165).

To my knowledge, such a description was never made officially available by Hokuyo, although such a description is very useful when you want to visualise measurements in RVIZ or simulate the sensor in Gazebo.

The URG04LX laser scanner was the first wide-spread lidar sensor and available in nearly any robotics lab. Note that the sensor is no longer in production, but still in use at many institutes.

<img src=https://staff.fnwi.uva.nl/a.visser/research/atwork/2022/HokuyoRobotModel.png alt="Hokuyo URG04LX description in RVIZ" style="display:block; margin-right: 10px; ext-align=center;" width="800">
     
### Known issues

* The description does not contain all coordinate frame transitions needed by RVIZ (yet)
* The software is currently tested on one computer, more tests are on the way.

## Installation

### Prerequisites

* [Ubuntu 20:04 ( Focal Fossa)](http://releases.ubuntu.com/focal/)

* [ROS1 Noetic Ninjemys](http://wiki.ros.org/noetic/Installation/Ubuntu)

If the ros-noetic is not already in your package list, add it

```bash
sudo apt update && sudo apt install curl gnupg2 lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/ros-latest.list > /dev/null

sudo apt update
```

This ros-node is using the following ros-packages:


```bash
sudo apt install ros-noetic-ros-base
sudo apt install ros-noetic-rviz
sudo apt install ros-noetic-xacro
sudo apt install ros-noetic-robot-state-publisher
sudo apt install ros-noetic-joint-state-publisher-gui
```


This ros-node is using the following ros-packages:


```bash
sudo apt install ros-noetic-urg-node
```

This node enables to communicate with all Hokuyo sensors that are complaint with [the SCIP 2.2 protocol](https://www.robotshop.com/media/files/pdf/communication-protocol-utm-30lx-ew.pdf) (or [earlier](http://library.isr.ist.utl.pt/docs/roswiki/attachments/hokuyo_node/URG-Series_SCIP2_Compatible_Communication_Specification_ENG.pdf)).

## Hardware connection

The Hokuyo URG04LX has two connectors: RS232 and USB. To connect the RS232 to a modern PC you need a USB-Serial converter:

<img src=https://sourceforge.net/p/urgnetwork/wiki/image/attachment/urg_connectSerial.jpg>

You could also directly use a USB-micro connector, but notice that the power should still be provided over the RS232 connection. When the URG04LX is connected via the USB-micro connector, no data is send over the RS232 interface:

<img src=https://sourceforge.net/p/urgnetwork/wiki/image/attachment/urg_connectUsb.jpg>

Other Hokuyo sensors have other power needs, see [the URG Network tutorial](https://sourceforge.net/p/urgnetwork/wiki/tutorial_en/) for more details.


## Software connection

When you connect the URG04LX sensor to your Linux computer, you could check what happens at device level with:

```bash
dmesg | tail
```

The sensor could be connected to /dev/ttyACM0 or /dev/ttyUSB0. We assume in this case /dev/ttyUSB0. Your could check if you have access to this device with 

```bash
rosrun urg_node getID /dev/ttyUSB0
```

The expected response is Device at /dev/ttyUSB0 has ID #. If not, make sure that you have the rights to read the device with

```bash
sudo chmod o+rw /dev/ttyUSB0
```

## Build the package

To install the **ros_hokuyo_urg04lx_description**, clone the package from github and build it:


```bash
cd ~/catkin_ws/src #use your current ros1 workspace folder
git clone https://github.com/physar/ros_hokuyo_urg04lx_description.git
cd ~/catkin_ws/
catkin_make
```

Now you could start a node which publishes the laser scans with 

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg_lidar_serial.launch
```

In another terminal you start the description with

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg-04lx.launch
```

In a third terminal you could start a transformation from the map to the location of the sensor:

```bash
rosrun tf2_ros static_transform_publisher 0 0 0 0 0 0 map laser
```

In a fourth terminal you could start the visualisation with 

```bash
rosrun rviz rviz -d rviz/urg_scan_and_model.rviz
```

The result should be the visualisation of both the LaserScan and the RobotModel 

<img src=https://staff.fnwi.uva.nl/a.visser/research/atwork/2022/Urg04lxModelRosNoetic.png alt="Hokuyo URG04LX description in ROS Noetic's RVIZ" style="display:block; margin-right: 10px; ext-align=center;" width="800">

### Acknowledgement

The mesh used in this description originates from [this Pioneer P3DX-gazebo project](https://github.com/akademikbilisim/ab2016-ros-gazebo)

### Prerequisites when for older versions of Ubuntu / ROS

* [Ubuntu 18:04 ( Bionic Beaver )](http://releases.ubuntu.com/bionic/)

* [ROS1 Melodic Morenia](http://wiki.ros.org/melodic/Installation/Ubuntu)

If the ros-melodic is not already in your package list, add it

```bash
sudo apt update && sudo apt install curl gnupg2 lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key  -o /usr/share/keyrings/ros-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/ros-latest.list > /dev/null

sudo apt update
```

This ros-node is using the following ros-packages:


```bash
sudo apt install ros-melodic-ros-base
sudo apt install ros-melodic-rviz
sudo apt install ros-melodic-xacro
sudo apt install ros-melodic-robot-state-publisher
sudo apt install ros-melodic-joint-state-publisher-gui
```


This ros-node is using the following ros-packages:
