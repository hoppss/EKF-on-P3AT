cmake_minimum_required(VERSION 2.8.3)
project(p3at_localization)


find_package(catkin REQUIRED COMPONENTS COMPONENTS geometry_msgs tf nav_msgs gazebo_ros gazebo_plugins gazebo_msgs gazebo_ros_control)


###################################
## catkin specific configuration ##
###################################

catkin_package(
  CATKIN_DEPENDS geometry_msgs nav_msgs tf
  LIBRARIES lib
)

###########
## Build ##
###########


include_directories(
    ${catkin_INCLUDE_DIRS}
)


