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


```bash
sudo apt install ros-melodic-urg-node
```
This node enables to communicate with all Hokuyo sensors that are complaint with [the SCIP 2.2 protocol](https://www.robotshop.com/media/files/pdf/communication-protocol-utm-30lx-ew.pdf) (or [earlier](http://library.isr.ist.utl.pt/docs/roswiki/attachments/hokuyo_node/URG-Series_SCIP2_Compatible_Communication_Specification_ENG.pdf)).

## Hardware connection

The Hokuyo URG04LX has two connectors: RS232 and USB. To connect the RS232 to a modern PC you need a USB-Serial converter:

<img src=https://sourceforge.net/p/urgnetwork/wiki/image/attachment/urg_connectSerial.jpg>

You could also directly use a USB-micro connector, but notice that the power should still be provided over the RS232 connection. When the URG04LX is connected via the USB-micro connector, no data is send over the RS232 interface:

<img src=https://sourceforge.net/p/urgnetwork/wiki/image/attachment/urg_connectUsb.jpg>

Other Hokuyo sensors have other power needs, see [the URG Network tutorial](https://sourceforge.net/p/urgnetwork/wiki/tutorial_en/) for more details.
