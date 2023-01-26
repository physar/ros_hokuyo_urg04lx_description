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

## Run the package

When you connected via /dev/ttyUSB0 you could start a node which publishes the laser scans with:

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg_lidar_serial.launch
```

Alternatively, when you connected via /dev/ttyACM0 you could start a node which publishes the laser scans with:

```bash
source ~/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg_lidar_usb.launch
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
source ~/catkin_ws/devel/setup.bash
rosrun rviz rviz -d rviz/urg_scan_and_model.rviz
```

The result should be the visualisation of both the LaserScan and the RobotModel 

<img src=https://staff.fnwi.uva.nl/a.visser/research/atwork/2022/Urg04lxModelRosNoetic.png alt="Hokuyo URG04LX description in ROS Noetic's RVIZ" style="display:block; margin-right: 10px; ext-align=center;" width="800">

If not, add at the left a LaserScan and a RobotModel. Specify for the LaserScan topic '\scan' and for the RobotModel the topic '\laser\robot_description'.

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


```bash
sudo apt install ros-melodic-urg-node
```


This approach was tested on a Ubuntu 18.04 and works fine. On the same machine the following alternative method was tested.

### Prerequisites when for newer / older versions of Ubuntu / ROS

* [Ubuntu 18:04 ( Bionic Beaver )](http://releases.ubuntu.com/bionic/)
* [Ubuntu 22:04 ( Jammy Jellyfish ) ](http://releases.ubuntu.com/jammy/)

* [ROS1 Noetic Ninjemys](https://github.com/RoboStack/ros-noetic)

You should know that ros-noetic is only supported for Ubuntu 20:04. To run nos-noetic you need [RoboStack](https://github.com/RoboStack/ros-noetic), which solves all dependencies on a specific version of Ubuntu.

The installation of ros-noetic for RoboStack is as follows:

When needed, install conda following the instructions from [https://github.com/conda-forge/miniforge](miniforge)
```bash
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"
bash Mambaforge-$(uname)-$(uname -m).sh
```

Next install mamba and inside mamba a robostack environment:
```bash
conda install mamba -c conda-forge
mamba create -n robostackenv ros-noetic-desktop python=3.9 -c robostack-staging -c conda-forge --no-channel-priority --override-channels
mamba init
source ~/.bashrc
mamba activate robostackenv
mamba install compilers cmake pkg-config make ninja
mamba install catkin_tools
mamba install mesa-libgl-devel-cos7-x86_64 mesa-dri-drivers-cos7-x86_64 libselinux-cos7-x86_64 libxdamage-cos7-x86_64 libxxf86vm-cos7-x86_64 libxext-cos7-x86_64 xorg-libxfixes
```

Specific for Ubuntu 18:04 is an update to the latest version of libglib with this commands:
```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test 
sudo apt upgrade libstdc++6
```

Unfortunatelly, the Hokuyo related packages cannot be installed via mamba, so they have to be build from source:
```bash
cd ~/mambaforge
mkdir -p catkin_ws/src
cd catkin_ws
catkin init
cd src
git clone https://github.com/ros-drivers/urg_node.git
git clone https://github.com/ros-perception/laser_proc.git
git clone https://github.com/ros-drivers/urg_c.git
git clone git clone https://github.com/physar/ros_hokuyo_urg04lx_description.git
cd ..
catkin build    
source devel/setup.bash
```
Now you could run the four ros-node in this environment in the following way:


When you connected via /dev/ttyUSB0 you could start a node which publishes the laser scans with:

```bash
mamba activate robostackenv
source ~/mambaforge/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg_lidar_serial.launch
```

Alternatively, when you connected via /dev/ttyACM0 you could start a node which publishes the laser scans with:

```bash
mamba activate robostackenv
source ~/mambaforge/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg_lidar_usb.launch
```
n another terminal you start the description with

```bash
mamba activate robostackenv
source ~/mambaforge/catkin_ws/devel/setup.bash
roslaunch ros_hokuyo_urg04lx_description urg-04lx.launch
```

In a third terminal you could start a transformation from the map to the location of the sensor:

```bash
mamba activate robostackenv
rosrun tf2_ros static_transform_publisher 0 0 0 0 0 0 map laser
```

In a fourth terminal you could start the visualisation with

```bash
mamba activate robostackenv
source ~/mambaforge/catkin_ws/devel/setup.bash
rosrun rviz rviz -d rviz/urg_scan_and_model.rviz
```

The result should be the visualisation of both the LaserScan and the RobotModel, as described before.

Note that this RoboStack approach has been tested both for Ubuntu 18:04 and for Ubuntu 22:04.

