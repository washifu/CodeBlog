---
layout: page
title: ROS Installation on Echidna Success
---

## Instructions for [Source Install](http://wiki.ros.org/jade/Installation/Source "ROS") of ROS Jade

First Check the following in Software & Updates
* Canonical-supported free and open-sources software (main)
* Community-maintained free and open-source software (universe)
* Proprietary drivers for devices (restricted)
* Software restricted by copyright or legal issues (multiverse)

Uncheck Source code
Download from: http://mirrors.aliyun.com/ubuntu 

Bootstrap Dependencies to facilitate the download and management of ROS
Making sure to have buid tools like compiler, CMake, etc.

```
sudo pip install -U rosdep rosinstall_generator wstool rosinstall
```

```
sudo pip install --upgrade setuptools
```

Initialize rosdep

```
sudo rosdep init
rosdep update
```

Installing core ROS packages
Create a catkin workspace

```
mkdir ~/ros_catkin_ws
cd ~/ros_catkin_ws
```

Desktop-Full Install: ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception

```
$ rosinstall_generator desktop_full --rosdistro jade --deps --wet-only --tar > jade-desktop-full-wet.rosinstall
$ wstool init -j8 src jade-desktop-full-wet.rosinstall
```

In case of error or termination:

```
wstool update -j 4 -t src
```

Switch to Chinese Mirror to increase speed

```
nano /etc/apt/sources.list.d/ros-latest.list
```

```
#deb http://packages.ros.org/ros/ubuntu trusty main
deb http://mirrors.ustc.edu.cn/ros/ubuntu trusty main
```

Making sure to have all the required dependencies

```
rosdep install --from-paths src --ignore-src --rosdistro jade -y
```

Build the catkin workspace
Invoke catkin\_make\_isolated

```
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
```

ERROR
You may come across an error with cmake not locating ```findEigen3.cmake```
The code is designed for cmake 2.8 but you can resolve this by making a soft link to 2.8's findEigen3

```
sudo ln -s /usr/share/cmake-2.8/Modules/FindEigen3.cmake /usr/share/cmake-3.2/Modules/FindEigen3.cmake
```

Invoke catkin\_make\_isolated

Packages will have now be installed
There should also be a ```setup.bash``` in ```~/ros_catkin_ws/install_isolated```

```
source ~/ros_catkin_ws/install_isolated/setup.bash
```

Maintenance
Move rosinstall file so it doesn't get overwritten and generate and updated version

```
$ mv -i jade-desktop-full-wet.rosinstall jade-desktop-full-wet.rosinstall.old
$ rosinstall_generator desktop_full --rosdistro jade --deps --wet-only --tar > jade-desktop-full-wet.rosinstall
```

Compare new rosintall file with old to see the updated packages

```
diff -u jade-desktop-full-wet.rosinstall jade-desktop-full-wet.rosinstall.old
```

Incorporate the changes

```
$ wstool merge -t src jade-desktop-full-wet.rosinstall
$ wstool update -t src
```

Rebuild workspace

```
./src/catkin/bin/catkin_make_isolated --install
```

Source the files

```
source ~/ros_catkin_ws/install_isolated/setup.bash
```


