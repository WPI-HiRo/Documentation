[TOC]



## Hstar

Refer to: http://hstartech.org/amp#/install/hydro

#### 1. Install system dependency

sudo apt-get install build-essential wget libboost-all-dev pkg-config
sudo apt-get install mercurial git
sudo apt-get install python-rosdep python-rosinstall python-pip python-serial python-setuptools
sudo apt-get install arduino-core gcc-avr avr-libc
sudo apt-get install vim
sudo apt-get install setserial
sudo pip install ino

#### 2. Install ROS Package Dependencies

__Replace "hydro" with kinetic__

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