<img src=https://www.hokuyo-aut.jp/assets/img/common/h_logo_fx.png alt="Hokuyo">

# ros_hokuyo_urg04lx_description

This unofficial package creates a ros node which  publishes the shape and coordinate frames of [the Hokoyu URG04LX laser scanner]{https://www.hokuyo-aut.jp/search/single.php?serial=165}.

To my knowledge, such a description was never made officially available by Hokuyo, although such a description is very useful when you want to visualise measurements in RVIZ or simulate the sensor in Gazebo.

The URG04LX laser scanner was the first wide-spread lidar sensor and available in nearly any robotics lab. Note that the sensor is no longer in production, but still in use at many institutes.

<img src="https://staff.fnwi.uva.nl/a.visser/research/atwork/2022/HokuyoRobotModel.png"
     alt="Hokuyo URG04LX description in RVIZ"
     style="display:block; margin-right: 10px; ext-align=center; width="800"/>
     
### Known issues

* The description does not contain all coordinate frame transitions needed by RVIZ (yet)
* The software is currently tested on one computer, more tests are on the way.

## Installation

### Prerequisites

* [Ubuntu 18:04 ( Bionic Beaver )](http://releases.ubuntu.com/bionic/)

* [ROS1 Melodic Morenia](http://wiki.ros.org/melodic/Installation/Ubuntu)
