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
frameId : The base link frame id
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
frameId : The map frame id
```
##### GPS to UTM converter node
```odom : Output odometry name
frame_id : The base link frame id
fix : Input GPS topic that publishes by the plugin
```

### Visual Odometry
Visual Odometry is the process of determining the position and orientation of a robot by analyzing the associated camera images.</br>
We can use mono_odometer or stereo_odometer node in viso2_ros package. The robot has one camera so I use mono_odometer node. The other node is used when we have two or more image sensor.</br>
The mono_odometer node is called in p3at_VO launch file.

#### Parameters
```sensor_frame_id : The input camera frame
mono : The camera topic root
image : The camera topic that publish image data. In this case, its name is rgb
base_link_frame_id : The base link frame id
```

### EKF Setup
In p3at_ekf launch file, the p3at_utm_odometry and the p3at_VO launch files are called.</br>
The EKF config is set in the localization.yaml file.</br>

The p3at_ekf is called in spawn launch file.

#### Parameters in the yaml file
```map_frame : The map frame id
odom_frame : The odometry frame id
base_link_frame : The base link frame id
world_frame : The same odom_frame
two_d_mode : Set it true if you want to the EKF use 3D informations
frequency : The publishing rate
print_diagnostics : Set it true if you want to publish logs into the /diagnostics topic
debug_out_file : The output file to keep EKF debug logs
debug : Set it true if you want to publish debug logs into the debug_out_file file.
publish_tf : Set it true if you want to publish the /tf topic
publish_acceleration : Set it true if you want to publish acceleration of the robot
```
For each input sensors (odom or imu) for EKF, these parameters should be set:
```<odom/imu>N : The name of the topic that the data is published in it.
<odom/imu>N_config : Set true or false to setup the EKF use which data that published from this sensor. The order of the boolean values are (X,Y,Z,roll,pitch,yaw,X˙,Y˙,Z˙,roll˙,pitch˙,yaw˙,X¨,Y¨,Z¨)
<odom/imu>N_differential : By setting this to true for a given sensor, all pose (position and orientation) data is converted to a velocity by calculating the change in the measurement value between two consecutive time steps. The general rule of thumb for the _differential parameter is that if a give robot has only one source of orientation data, then the differential parameter should be set to false. If there are N sources, users can set the _differential parameter to true for N−1 of them, or simply ensure that the covariance values are large enough to eliminate oscillations.
<imu>N_remove_gravitational_acceleration :  When you use accelerations and set the remove_gravitational_acceleration flag to true for that sensor, the filter uses the orientation reported by the IMU to remove acceleration due to gravity. For example, if your IMU was rolled over on its side, we'd need to subtract or add acceleration due to gravity to the Y axis instead of Z. Since your IMU doesn't report orientation, the filter can't compute the opposing vector, and the measurement doesn't get fused.
