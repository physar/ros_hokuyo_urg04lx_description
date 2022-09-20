cmake_minimum_required(VERSION 2.8.3)
project(ros_hokuyo_urg04lx_description)

find_package(catkin REQUIRED COMPONENTS
  std_msgs
)


catkin_package(
   LIBRARIES ros_hokuyo_urg04lx_description
   CATKIN_DEPENDS std_msgs 
)

set(AVAILABLE_ROS_VERSIONS "melodic;lunar;kinetic;jade;indigo;noetic")

foreach(version ${AVAILABLE_ROS_VERSIONS})
	find_path(ROS_LIBS libroscpp.so PATHS /opt/ros/${version}/lib)
endforeach()

link_directories(
        ${ROS_LIBS}
     
)


set(CATKIN_LIBS  python2.7.so tinyxml2.so pthread.so)


find_package(PkgConfig REQUIRED)

install(DIRECTORY launch meshes urdf xacro
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
