[TOC]

<span>
$ROS_variable test_code stuff $
  </span>

## Reflex SF

#### ROS packages from Reflex SF
http://docs.righthandrobotics.com/main:reflex-sf

__Note__:
* The reflex-sf packages under iml-internal/Ebolabot/Hardware/ros_ws have expired, due to that ROS kinetic uses a different matrix calculation function. 
* Check out the ros package from Reflex SF website, which is consistent with both ROS indigo and kinetic

__Note__: 
* This package has been updated and thus not fully compatile with TRINA.
* Still need to link the rospackages for hand under "iml-internal/Ebolabot/Hardware/ros_ws" to "ros_ws", for compiling

#### 3. Error messages and solutions

##### cannot find "Hand.h" or "JointState.h"

__Error__:

/home/trina/ros_ws/src/reflex-ros-pkg/reflex_driver/reflex_driver_node.cpp:21:30: fatal error: reflex_msgs/Hand.h: No such file or directory

__Solution__:
* Complie older ros package for hands (reflex-sf-pkg, reflex-sf-apc) --> Compile should fail (since the math function used by Hand.h doesn't exist in ROS kinetic), but will generate reflex_msgs in /ros_ws/devel/include, which has the header files needed for Ebolabot folder to be compiled.
* Check out the new ros package for reflex sf hand from website -->  this can be compiled successfully in /ros_ws. __Remember__ to move "reflex-ros-pkg", and "reflex-sf-apc" out of "ros_ws/src" to avoid conflict
* Check if the header files exist in ros_ws/devel/include

## Hstar

Refer to: http://hstartech.org/amp#/install/hydro

#### 1. Install system dependency
```
sudo apt-get install build-essential wget libboost-all-dev pkg-config
sudo apt-get install mercurial git
sudo apt-git clone https://github.com/RightHandRobotics/reflex-sf-ros-pkg.gitget install python-rosdep python-rosinstall python-pip python-serial python-setuptools
sudo apt-get install arduino-core gcc-avr avr-libc
sudo apt-get install vim
sudo apt-get install setserial
sudo pip install ino
```

#### 2. Install ROS Package Dependencies

__Replace "hydro" with kinetic__
```
sudo apt-get install ros-kinetic-joystick-drivers
sudo apt-get install ros-kinetic-laser-*
sudo apt-get install ros-kinetic-gmapping
sudo apt-get install ros-kinetic-hokuyo-node
sudo apt-get install ros-kinetic-serial
sudo apt-get install ros-kinetic-rosserial
sudo apt-get install ros-kinetic-rosserial-arduino
sudo apt-get install ros-kinetic-dynamixel-controllers
sudo apt-get install ros-kinetic-openni-*
sudo apt-get install ros-kinetic-openni2-*
```
#### 3. Error messages and solutions

##### ros/ros.h
__Error__: 
/home/trina/ros_ws/src/hstar_amp/src/global_pose_publisher.cpp:9:21: fatal error: ros/ros.h: No such file or directory

__Solution__:
- In linked Hstar ROS package, uncomment the following lines: 
```
include_directories(
  ${catkin_INCLUDE_DIRS}
)
```

Otherwise, cannot find ros/ros.h, which belongs to roscpp

##### cannot find "Hand.h" or "JointState.h"

__Error__:

/home/trina/ros_ws/src/reflex-ros-pkg/reflex_driver/reflex_driver_node.cpp:21:30: fatal error: reflex_msgs/Hand.h: No such file or directory

__Solution__:
* Complie older ros package for hands (reflex-sf-pkg, reflex-sf-apc) --> Compile should fail (since the math function used by Hand.h doesn't exist in ROS kinetic), but will generate reflex_msgs in /ros_ws/devel/include, which has the header files needed for Ebolabot folder to be compiled.
* Check out the new ros package for reflex sf hand from website ---> this can be compiled successfully in /ros_ws
* Check if the header files exist in ros_ws/devel/include

## Iml-Internal/Ebolabot

#### Modify CMakeList.txt
* Replace "/home/motion/" with "/home/trina" (or your current computer name) 
* LIBJPEG, LIBPNG can not  be found without the following changes```=

  FIND_PACKAGE(JPEG)
  FIND_PACKAGE(PNG)
* Comment out "FFmpeg", or refer to https://gist.github.com/royshil/6318407 for a fix
* Add to INCLUDE_DIRECTORIES

```
Add "${ROS_WS}/devel/include" to the "INCLUDE_DIRECTORIES" of  "ebolabot library: real robot". 
Due to the updated reflex sf ros package, "reflex_msgs/Command.h" only exists in "${ROS_WS}/devel/include"
```

#### Compile SSPP
__Path__ = /home/trina/iml-internal/SSPP
__Compile__:

```
$ cmake; make;
```

__Installation__ = refer to README.txt

You can simply copy the Python/sspp directory wherever you need it, or
install it to your python distribution by entering

```
python setup.py install
```

or on Linux systems,
```
sudo python setup.py install
```

in the ./Python directory.

#### Errors and solution


##### fatal error: reflex_msgs/Command.h: No such file or directory

__Solution__:

```
Add ${ROS_WS}/devel/include" to the "INCLUDE_DIRECTORIES" of  "ebolabot library: real robot". 

Due to the updated reflex sf ros package, "reflex_msgs/Command.h" only exists in "${ROS_WS}/devel/include"
```



##### Error with "motion_physical.cpp"

```
/home/trina/iml-internal/Ebolabot/Motion/motion_physical.cpp:697:9: error: ‘baxter_core_msgs::HeadPanCommand {aka struct baxter_core_msgs::HeadPanCommand_<std::allocator<void> >}’ has no member named ‘speed’
```
__Solution__:

The problem is caused:
```
msg.speed = int(robotState.head.panSpeed*100);
```

"(int)msg.speed" has been replaced by "(double)msg.speed_ratio"

Change it to:
```
msg.speed_ratio = double(robotState.head.panSpeed);
```

