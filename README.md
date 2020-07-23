# rosbot_description #

URDF model for Gazebo integrated with ROS.

## Installation ## 

We assume that you are working on Ubuntu 16.04 and already have installed ROS Kinetic. If not, follow the [ROS install guide](http://wiki.ros.org/kinetic/Installation/Ubuntu)

Prepare the repository:
```
cd ~
mkdir ros_workspace
mkdir ros_workspace/src
cd ~/ros_workspace/src
catkin_init_workspace
cd ~/ros_workspace
catkin_make
```

Above commands should execute without any warnings or errors.


Clone this repository to your workspace:

```
cd ~/ros_workspace/src
git clone https://github.com/husarion/rosbot_description.git
```

Install depencencies:

```
cd ~/ros_workspace
rosdep install --from-paths src --ignore-src -r -y
```

Build the workspace:

```
cd ~/ros_workspace
catkin_make
```

From this moment you can use rosbot simulations. Please remember that each time, when you open new terminal window, you will need to load system variables:

```
source ~/ros_workspace/devel/setup.sh
```

## How to use ##

### Simulation ###

In Terminal 1, launch the Gazebo simulation:

```
roslaunch rosbot_description rosbot.launch
```

If Rviz is needed, then run the following instead:

```
roslaunch rosbot_description rosbot_rviz.launch
```

### Real Robot ###

In Terminal 1, launch sensors:

```
roslaunch rosbot_description rosbot_hardware.launch
```

In Terminal 2, launch mobile base and estimator:

```
roslaunch rosbot_ekf all.launch rosbot_pro:=true
```

### Keyboard Teleop ###

In addition to previous steps (in Simulation or Real Robot), open a new terminal and run:

```
roslaunch rosbot_navigation keyboard_teleop.launch
```

## APIs ##

According to [ROSbot manual](https://husarion.com/manuals/rosbot-manual/#ros-api), we have the following APIs available.

<table>
<thead>
<tr><th>Topic</th><th>Message type</th><th>Direction</th><th>Node</th><th>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Description&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</th></tr>
</thead>
<tbody>
<tr><td><code>/mpu9250</code></td><td><code>rosbot_ekf/Imu</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Raw IMU data in custom message type</td></tr>
<tr><td><code>/range/fl</code></td><td><code>sensor_msgs/Range</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Front left range sensor raw data</td></tr>
<tr><td><code>/range/fr</code></td><td><code>sensor_msgs/Range</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Front right range sensor raw data</td></tr>
<tr><td><code>/range/rl</code></td><td><code>sensor_msgs/Range</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Rear left range sensor raw data</td></tr>
<tr><td><code>/range/rr</code></td><td><code>sensor_msgs/Range</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Rear right range sensor raw data</td></tr>
<tr><td><code>/joint_states</code></td><td><code>sensor_msgs/JointState</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Wheels rotation angle</td></tr>
<tr><td><code>/battery</code></td><td><code>sensor_msgs/BatteryState</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Battery voltage</td></tr>
<tr><td><code>/buttons</code></td><td><code>std_msgs/UInt8</code></td><td>publisher</td><td><code>/serial_node</code></td><td>User buttons state, details in <a href="#user-buttons">User buttons</a> section</td></tr>
<tr><td><code>/pose</code></td><td><code>geometry_msgs/PoseStamped</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Position based on encoders</td></tr>
<tr><td><code>/odom/wheel</code></td><td><code>nav_msgs/Odometry</code></td><td>publisher</td><td><code>/msgs_conversion</code></td><td>Odometry based on wheel encoders</td></tr>
<tr><td><code>/velocity</code></td><td><code>geometry_msgs/Twist</code></td><td>publisher</td><td><code>/serial_node</code></td><td>Odometry based on encoders</td></tr>
<tr><td><code>/imu</code></td><td><code>sensor_msgs/Imu</code></td><td>publisher</td><td><code>/msgs_conversion</code></td><td>IMU data wrapped in standard ROS message type</td></tr>
<tr><td><code>/odom</code></td><td><code>nav_msgs/Odometry</code></td><td>publisher</td><td><code>/rosbot_ekf</code></td><td>Odometry based on sensor fusion</td></tr>
<tr><td><code>/tf</code></td><td><code>tf2_msgs/TFMessage</code></td><td>publisher</td><td><code>/rosbot_ekf</code></td><td>ROSbot position based on sensor fusion</td></tr>
<tr><td><code>/set_pose</code></td><td><code>geometry_msgs/</code> <code>PoseWithCovarianceStamped</code></td><td>subscriber</td><td><code>/rosbot_ekf</code></td><td>Allow to set custom state of EKF</td></tr>
<tr><td><code>/cmd_vel</code></td><td><code>geometry_msgs/Twist</code></td><td>subscriber</td><td><code>/serial_node</code></td><td>Velocity commands</td></tr>
<tr><td><code>/config</code></td><td><code>rosbot_ekf/Configuration</code></td><td>service server</td><td><code>/serial_node</code></td><td>Allow to control behaviour of CORE2 board, detaild in <a href="#core2-config">CORE2 config</a> section</td></tr>
</tbody>
</table>
