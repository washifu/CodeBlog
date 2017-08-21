---
layout: page
title: ROS Installation on Echidna Fail
---

### Failure Record

## Instructions for [Source Install](http://wiki.ros.org/jade/Installation/Source "ROS") of ROS Jade [FAIL]

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

Installing core ROS packages <\br>
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

Making sure to have all the required dependencies

```
rosdep install --from-paths src --ignore-src --rosdistro jade -y
```

ERROR

```
Reading state information... Done
E: Unable to locate package ros-jade-rviz
ERROR: the following rosdeps failed to install
  apt: command [sudo -H apt-get install -y ros-jade-rviz] failed
```

NEW ERROR

```
executing command [sudo -H apt-get install -y python-mock]
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Package python-mock is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'python-mock' has no installation candidate
ERROR: the following rosdeps failed to install
  apt: command [sudo -H apt-get install -y python-mock] failed
```

Resolve error by installing mock

```
sudo pip install mock
```

No dice

Upgrade mock

```
sudo apt-get upgrade
```

Build the catkin workspace
Invoke catkin\_make\_isolated

```
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
```

ERROR

```
==> Processing catkin package: 'catkin'
==> cmake /home/wasif/ros_catkin_ws/src/catkin -DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/catkin -DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated -DCMAKE_BUILD_TYPE=Release -G Unix Makefiles in '/home/wasif/ros_catkin_ws/build_isolated/catkin'
-- Using CATKIN_DEVEL_PREFIX: /home/wasif/ros_catkin_ws/devel_isolated/catkin
-- Using CMAKE_PREFIX_PATH: 
-- Using PYTHON_EXECUTABLE: /usr/bin/python
-- Using Debian Python package layout
-- Could NOT find PY_em (missing:  PY_EM) 
CMake Error at cmake/empy.cmake:29 (message):
  Unable to find either executable 'empy' or Python module 'em'...  try
  installing the package 'python-empy'
Call Stack (most recent call first):
  cmake/all.cmake:147 (include)
  CMakeLists.txt:8 (include)


-- Configuring incomplete, errors occurred!
See also "/home/wasif/ros_catkin_ws/build_isolated/catkin/CMakeFiles/CMakeOutput.log".
<== Failed to process package 'catkin': 
  Command '['cmake', '/home/wasif/ros_catkin_ws/src/catkin', '-DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/catkin', '-DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated', '-DCMAKE_BUILD_TYPE=Release', '-G', 'Unix Makefiles']' returned non-zero exit status 1

Reproduce this error by running:
==> cd /home/wasif/ros_catkin_ws/build_isolated/catkin && cmake /home/wasif/ros_catkin_ws/src/catkin -DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/catkin -DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated -DCMAKE_BUILD_TYPE=Release -G 'Unix Makefiles'

Command failed, exiting.
```

Resolve error by installing python-empy

```
sudo apt-get install python-empy
```

Invoke catkin\_make\_isolated
ERROR

```
CMake Error at CMakeLists.txt:6 (find_package):
  By not providing "Findconsole_bridge.cmake" in CMAKE_MODULE_PATH this
  project has asked CMake to find a package configuration file provided by
  "console_bridge", but CMake did not find one.

  Could not find a package configuration file provided by "console_bridge"
  with any of the following names:

    console_bridgeConfig.cmake
    console_bridge-config.cmake

  Add the installation prefix of "console_bridge" to CMAKE_PREFIX_PATH or set
  "console_bridge_DIR" to a directory containing one of the above files.  If
  "console_bridge" provides a separate development package or SDK, be sure it
  has been installed.


-- Configuring incomplete, errors occurred!
See also "/home/wasif/ros_catkin_ws/build_isolated/class_loader/CMakeFiles/CMakeOutput.log".
See also "/home/wasif/ros_catkin_ws/build_isolated/class_loader/CMakeFiles/CMakeError.log".
<== Failed to process package 'class_loader': 
  Command '['/home/wasif/ros_catkin_ws/install_isolated/env.sh', 'cmake', '/home/wasif/ros_catkin_ws/src/class_loader', '-DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/class_loader', '-DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated', '-DCMAKE_BUILD_TYPE=Release', '-G', 'Unix Makefiles']' returned non-zero exit status 1

Reproduce this error by running:
==> cd /home/wasif/ros_catkin_ws/build_isolated/class_loader && /home/wasif/ros_catkin_ws/install_isolated/env.sh cmake /home/wasif/ros_catkin_ws/src/class_loader -DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/class_loader -DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated -DCMAKE_BUILD_TYPE=Release -G 'Unix Makefiles'

Command failed, exiting.
```

Resolve error by installing python-nose

```
sudo apt-get install python-nose
```

~~The "console_bridge" error persists. I am at a loss here.~~
Resolve error by switching Software & Update download server to the US server and use VPN. The packages will then be found.

Manually Install ros-jade-rviz

```
sudo apt-get install ros-jade-rviz
```

Invoke catkin\_make\_isolated
ERROR

```
CMake Error at /home/wasif/ros_catkin_ws/install_isolated/share/catkin/cmake/catkinConfig.cmake:75 (find_package):
  Could not find a package configuration file provided by "rviz" with any of
  the following names:

    rvizConfig.cmake
    rviz-config.cmake

  Add the installation prefix of "rviz" to CMAKE_PREFIX_PATH or set
  "rviz_DIR" to a directory containing one of the above files.  If "rviz"
  provides a separate development package or SDK, be sure it has been
  installed.
Call Stack (most recent call first):
  CMakeLists.txt:7 (find_package)


-- Configuring incomplete, errors occurred!
See also "/home/wasif/ros_catkin_ws/build_isolated/rviz_plugin_tutorials/CMakeFiles/CMakeOutput.log".
See also "/home/wasif/ros_catkin_ws/build_isolated/rviz_plugin_tutorials/CMakeFiles/CMakeError.log".
<== Failed to process package 'rviz_plugin_tutorials': 
  Command '['/home/wasif/ros_catkin_ws/install_isolated/env.sh', 'cmake', '/home/wasif/ros_catkin_ws/src/visualization_tutorials/rviz_plugin_tutorials', '-DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/rviz_plugin_tutorials', '-DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated', '-DCMAKE_BUILD_TYPE=Release', '-G', 'Unix Makefiles']' returned non-zero exit status 1

Reproduce this error by running:
==> cd /home/wasif/ros_catkin_ws/build_isolated/rviz_plugin_tutorials && /home/wasif/ros_catkin_ws/install_isolated/env.sh cmake /home/wasif/ros_catkin_ws/src/visualization_tutorials/rviz_plugin_tutorials -DCATKIN_DEVEL_PREFIX=/home/wasif/ros_catkin_ws/devel_isolated/rviz_plugin_tutorials -DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated -DCMAKE_BUILD_TYPE=Release -G 'Unix Makefiles'

Command failed, exiting.
```

Resolve by rerunning ```wstool update -j 4 -t src``` now that we are on US server
Invoke catkin\_make\_isolated

ERROR

```
CMake Error at /usr/share/cmake-3.2/Modules/FindPackageHandleStandardArgs.cmake:138 (message):
  Could NOT find FLTK (missing: FLTK_LIBRARIES)
Call Stack (most recent call first):
  /usr/share/cmake-3.2/Modules/FindPackageHandleStandardArgs.cmake:374 (_FPHSA_FAILURE_MESSAGE)
  /usr/share/cmake-3.2/Modules/FindFLTK.cmake:316 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  CMakeLists.txt:106 (find_package)


-- Configuring incomplete, errors occurred!
See also "/home/wasif/ros_catkin_ws/build_isolated/stage/install/CMakeFiles/CMakeOutput.log".
<== Failed to process package 'stage': 
  Command '['/home/wasif/ros_catkin_ws/install_isolated/env.sh', 'cmake', '/home/wasif/ros_catkin_ws/src/stage', '-DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated', '-DCMAKE_BUILD_TYPE=Release', '-G', 'Unix Makefiles']' returned non-zero exit status 1

Reproduce this error by running:
==> cd /home/wasif/ros_catkin_ws/build_isolated/stage && /home/wasif/ros_catkin_ws/install_isolated/env.sh cmake /home/wasif/ros_catkin_ws/src/stage -DCMAKE_INSTALL_PREFIX=/home/wasif/ros_catkin_ws/install_isolated -DCMAKE_BUILD_TYPE=Release -G 'Unix Makefiles'

Command failed, exiting.

```

I cannot figure out how to resolve this.
Giving up on source install.

## [Debian Package Install](http://wiki.ros.org/jade/Installation/Ubuntu "ROS")

Configure Ubuntu repositories to allow "restricted," "universe," and "multiverse"
Open Software & Update and under the Ubuntu Software tab enable these options.

Setup sources.list
ROS Jade ONLY supports Trusty (14.04), Utopic (14.10) and Vivid (15.04)

```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

Set up keys

```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```

Update

```
sudo apt-get update
```

Desktop-Full Install: ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception

```
sudo apt-get install ros-jade-desktop-full
```

ERROR

```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 ros-jade-desktop-full : Depends: ros-jade-simulators but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

Resolve error by installing ros-jade-simulators

```
sudo apt-get install ros-jade-simulators
```

ERROR

```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 ros-jade-simulators : Depends: ros-jade-gazebo-ros-pkgs but it is not going to be installed
                       Depends: ros-jade-stage-ros but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

Resolve error by ```sudo apt-get install ros-jade-gazebo-ros-pkgs``` and ```sudo apt-get install ros-jade-stage-ros```

ERROR
```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 ros-jade-stage-ros : Depends: libfltk1.1 (>= 1.1.6) but it is not installable
                      Depends: ros-jade-stage but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

Resolve error by ```sudo apt-get install libfltk1.1``` and ```sudo apt-get install ros-jade-stage```

ERROR

```
Package libfltk1.1 is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'libfltk1.1' has no installation candidate
```

```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 ros-jade-stage : Depends: libfltk1.1 (>= 1.1.7) but it is not installable
                  Depends: libfltk1.1-dev but it is not installable
E: Unable to correct problems, you have held broken packages.
```

Resolve by ```sudo apt-get install  libfltk1.1-dev```

```
Package libfltk1.1-dev is not available, but is referred to by another package.
This may mean that the package is missing, has been obsoleted, or
is only available from another source

E: Package 'libfltk1.1-dev' has no installation candidate
```

Sad day
