# WE UPDATED SOME PARAMETERS AND SCRIPTS FOR PERSONAL USE
# TL;DR
```
git clone https://github.com/Dynamics-Learning-Workshop/unitree_ros.git
sudo apt-get install ros-noetic-controller-interface  ros-noetic-gazebo-ros-control ros-noetic-joint-state-controller ros-noetic-effort-controllers ros-noetic-joint-trajectory-controller ros-noetic-move-base-msgs
```

And open the file `unitree_gazebo/worlds/stairs.world`. At the end of the file modify your own path:
```
<include>
    <uri>model:///root/leg_ws/src/unitree_ros/unitree_gazebo/worlds/building_editor_models/stairs</uri> 
</include>
```
```
git submodule update --init --recursive
```
```
cd ~/leg_ws
catkin_make
./leg_ws/src/unitree_ros/sim.sh
```

# Introduction
Here are the ROS simulation packages for Unitree robots, You can load robots and joint controllers in Gazebo, so you can perform low-level control (control the torque, position and angular velocity) of the robot joints. Please be aware that the Gazebo simulation cannot do high-level control, namely walking. Aside from these simulation functions, you can also control your real robots in ROS with the [unitree_ros_to_real](https://github.com/unitreerobotics/unitree_ros_to_real) packages. For real robots, you can do high-level and low-level control using our ROS packages.

## Packages:
Robot description: `go1_description`, `a1_description`, `aliengo_description`, `laikago_description`, `z1_description`

Robot and joints controller: `unitree_controller`, `z1_controller`

Simulation related: `unitree_gazebo`, `unitree_legged_control`

# Dependencies
* [ROS](https://www.ros.org/) Melodic or ROS Kinetic (has not been tested)
* [Gazebo8](http://gazebosim.org/)
* [unitree_legged_msgs](https://github.com/unitreerobotics/unitree_ros_to_real): `unitree_legged_msgs` is a package under [unitree_ros_to_real](https://github.com/unitreerobotics/unitree_ros_to_real).
# Build
<!-- If you would like to fully compile the `unitree_ros`, please run the following command to install relative packages. -->

For ROS Melodic:
```
sudo apt-get install ros-melodic-controller-interface  ros-melodic-gazebo-ros-control ros-melodic-joint-state-controller ros-melodic-effort-controllers ros-melodic-joint-trajectory-controller
```
For ROS Kinetic:
```
sudo apt-get install ros-kinetic-controller-manager ros-kinetic-ros-control ros-kinetic-ros-controllers ros-kinetic-joint-state-controller ros-kinetic-effort-controllers ros-kinetic-velocity-controllers ros-kinetic-position-controllers ros-kinetic-robot-controllers ros-kinetic-robot-state-publisher ros-kinetic-gazebo8-ros ros-kinetic-gazebo8-ros-control ros-kinetic-gazebo8-ros-pkgs ros-kinetic-gazebo8-ros-dev
```

And open the file `unitree_gazebo/worlds/stairs.world`. At the end of the file:
```
<include>
    <uri>model:///home/unitree/catkin_ws/src/unitree_ros/unitree_gazebo/worlds/building_editor_models/stairs</uri>
</include>
```
Please change the path of `building_editor_models/stairs` to the real path on your PC.

```
git submodule update --init --recursive
```

Then you can use catkin_make to build:
```
cd ~/catkin_ws
catkin_make
```

If you face a dependency problem, you can just run `catkin_make` again.

# Detail of Packages
## unitree_legged_control:
It contains the joints controllers for Gazebo simulation, which allows users to control joints with position, velocity and torque. Refer to "[unitree_ros/unitree_controller/src/servo.cpp](https://github.com/unitreerobotics/unitree_ros/blob/master/unitree_controller/src/servo.cpp)" for joint control examples in different modes.

## The description of robots:
Namely the description of Go1, A1, Aliengo and Laikago. Each package includes mesh, urdf and xacro files of robot. Take Laikago for example, you can check the model in Rviz by:
```
roslaunch laikago_description laikago_rviz.launch
```

## unitree_gazebo & unitree_controller:
You can launch the Gazebo simulation with the following command:
```
roslaunch unitree_gazebo normal.launch rname:=a1 wname:=stairs
```
Where the `rname` means robot name, which can be `laikago`, `aliengo`, `a1` or `go1`. The `wname` means world name, which can be `earth`, `space` or `stairs`. And the default value of `rname` is `laikago`, while the default value of `wname` is `earth`. In Gazebo, the robot should be lying on the ground with joints not activated.

### 1. Stand controller
After launching the gazebo simulation, you can start to control the robot:
```
rosrun unitree_controller unitree_servo
```

And you can add external disturbances, like a push or a kick:
```
rosrun unitree_controller unitree_external_force
```
### 2. Position and pose publisher
Here we demonstrated how to control the position and pose of robot without a controller, which should be useful in SLAM or visual development.

Then run the position and pose publisher in another terminal:
```
rosrun unitree_controller unitree_move_kinetic
```
The robot will turn around the origin, which is the movement under the world coordinate frame. And inside of the source file [move_publisher.cpp](https://github.com/unitreerobotics/unitree_ros/blob/master/unitree_controller/src/move_publisher.cpp), we also provide the method to move using the robot coordinate frame. You can change the value of `def_frame` to `coord::ROBOT` and run the catkin_make again, then the `unitree_move_publisher` will move robot under its own coordinate frame.

## z1_controller

You can launch the z1 Gazebo simulation with the following command:

```
roslaunch unitree_gazebo z1.launch
```

After launching the gazebo simulation, you can start to control the z1 robot by z1_sdk.  
see [z1_documentation](https://dev-z1.unitree.com)
