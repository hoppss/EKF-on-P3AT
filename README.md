# robot-test

## EKF Localization
I use EKF to estimate robot covariance with GPS, IMU and Visual Odometry.

To use EKF you must run p3at_ekf launch file in the p3at_localization package, that called in p3at_spawn launch file.

### IMU
IMU create by Hector IMU plugin that used in pioneer3at_body.xacro and publish Odometry.

#### Parameters
robotNamespace : The namespace of the robot that set in robotname parameter in the launch file
bodyName : The name of the base link of the robot, set in pioneer3at_body.xacro
topicName : The output topic of the IMU sensor
frameId : The body name tf id

### GPS
GPS create by Hector GPS plugin that used in pioneer3at_body.xacro.
The Hector GPS plugin publishes NavSatFix data. To convert NavSatFix to odometry that has UTM position, we use gps_conv_utm node in the gps_common message. It's called in p3at_utm_odometry launch file in p3at_localization package.

#### Parameters
##### Hector plugin
robotNamespace : The namespace of the robot that set in robotname parameter in the launch file
bodyName : The name of the base link of the robot, set in pioneer3at_body.xacro
topicName : The output topic of the GPS sensor
velocityTopicName : The output topic of the GPS sensor that shows the velocity of the robot
frameId : The map tf id
##### GPS to UTM converter node
odom : Output odometry name
frame_id : The body name tf id
fix : Input GPS topic
