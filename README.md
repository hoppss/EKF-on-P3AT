# robot-test

## EKF Localization
I use EKF to estimate robot covariance with GPS, IMU and Visual Odometry.</br>
To use EKF you must run p3at_ekf launch file in the p3at_localization package, that called in p3at_spawn launch file.

### IMU
IMU create by Hector IMU plugin that used in pioneer3at_body.xacro and publish Odometry.

#### Parameters
```
robotNamespace : The namespace of the robot that set in robotname parameter in the launch file
bodyName : The name of the base link of the robot, set in pioneer3at_body.xacro
topicName : The output topic of the IMU sensor
frameId : The base link tf id
```

### GPS
GPS create by Hector GPS plugin that used in pioneer3at_body.xacro.</br>
The Hector GPS plugin publishes NavSatFix data. To convert NavSatFix to odometry that has UTM position, I use gps_conv_utm node in the gps_common package. It's called in p3at_utm_odometry launch file in p3at_localization package.

#### Parameters
##### Hector plugin
```robotNamespace : The namespace of the robot that set in robotname parameter in the launch file
bodyName : The name of the base link of the robot, set in pioneer3at_body.xacro
topicName : The output topic of the GPS sensor
velocityTopicName : The output topic of the GPS sensor that shows the velocity of the robot
frameId : The map tf id
```
##### GPS to UTM converter node
```odom : Output odometry name
frame_id : The body name tf id
fix : Input GPS topic that publishes by the plugin
```

### Visual Odometry
Visual Odometry is the process of determining the position and orientation of a robot by analyzing the associated camera images.</br>
We can use mono_odometer or stereo_odometer node in viso2_ros package. The robot has one camera so I use mono_odometer node. The other node is used when we have two or more image sensor.</br>

#### Parameters
```sensor_frame_id : The input camera frame
mono : The camera topic root
image : The camera topic that publish image data. In this case, its name is rgb
base_link_frame_id : The base link tf id
```
