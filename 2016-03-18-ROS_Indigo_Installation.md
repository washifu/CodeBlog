---
layout: page
title: ROS Indigo Installation
---

Bad News: ROS [velodyne stack](http://wiki.ros.org/velodyne "ROS") is only supported in indigo and not yet in jade (likely this will change) <br>
So, here is the ROS Indigo Install. It's fairly straight forward and much like the jade install but with fewer steps. <br>
Note that with two ROS distros, sourcing the setup.bash should be done manually when using each distro.

Bootstrap dependencies and rosdep has already been installed.

Make a new directory for Indigo

```
$ mkdir ~/ros_catkin_ws_indigo
$ cd ~/ros_catkin_ws_indigo
```

Desktop-Full Install

```
$ rosinstall_generator desktop_full --rosdistro indigo --deps --wet-only --tar > indigo-desktop-full-wet.rosinstall
$ wstool init -j8 src indigo-desktop-full-wet.rosinstall
```

Resolve Dependencies

```
rosdep install --from-paths src --ignore-src --rosdistro indigo -y
```

ERROR

```
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:

The following packages have unmet dependencies:
 gazebo2 : Depends: libsdformat-dev (>= 1.4.11-1osrf1) but it is not going to be installed
           Depends: libsdformat-dev (< 2.0.0) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
ERROR: the following rosdeps failed to install
  apt: command [sudo -H apt-get install -y gazebo2] failed
```

This is because the gazebo version supported by indigo (gazebo2) and jade (gazebo5) [conflict](http://answers.ros.org/question/208334/gazebo-version-collides-between-indigo-and-jade/ "Article about gazebo conflicts"). <br>
Sadly, this means uninstalling gazebo5 and breaking jade dependencies.

Fortunately, I found this [patch](https://askubuntu.com/questions/551749/ros-wont-install-on-14-04-dependency-hell "See Answers"):

```
sudo apt-get install libsdformat1
```

Output:

```
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages were automatically installed and are no longer required:
  gazebo5-common hdf5-helpers libarmadillo4 libarpack2 libdap-dev
  libdapserver7 libepsilon1 libfreexl1 libgdal-dev libgdal1h libgeos-3.4.2
  libgeos-c1 libgeos-dev libgif-dev libgsl0-dev libgsl0ldbl libgts-0.7-5
  libgts-bin libgts-dev libhdf4-0-alt libhdf4-alt-dev libhdf5-dev libkml0
  libogdi3.2 libplayerc++3.0 libplayerc++3.0-dev libplayerc3.0
  libplayerc3.0-dev libplayercommon3.0 libplayercommon3.0-dev libplayercore3.0
  libplayercore3.0-dev libplayerdrivers3.0 libplayerdrivers3.0-dev
  libplayerinterface3.0 libplayerinterface3.0-dev libplayerjpeg3.0
  libplayerjpeg3.0-dev libplayertcp3.0 libplayertcp3.0-dev libplayerwkb3.0
  libplayerwkb3.0-dev libproj0 libprotoc-dev libprotoc8 libsimbody-dev
  libsimbody3.5 libspatialite-dev libspatialite5 libspnav0 libstatgrab9
  libtar-dev libwebp-dev libwebpdemux1 libxerces-c-dev libxerces-c3.1 proj-bin
  proj-data robot-player-dev ttf-dejavu-core
Use 'apt-get autoremove' to remove them.
The following packages will be REMOVED:
  gazebo5-plugin-base libgazebo5 libgazebo5-dev libsdformat2 libsdformat2-dev
  sdformat-sdf
The following NEW packages will be installed:
  libsdformat1
0 upgraded, 1 newly installed, 6 to remove and 129 not upgraded.
Need to get 235 kB of archives.
After this operation, 32.9 MB disk space will be freed.
Do you want to continue? [Y/n]
```

Y

```
Get:1 http://mirrors.aliyun.com/ubuntu/ trusty-updates/universe libsdformat1 amd64 1.4.11-1ubuntu0.1 [235 kB]
Fetched 235 kB in 0s (663 kB/s)      
(Reading database ... 369017 files and directories currently installed.)
Removing libgazebo5-dev:amd64 (5.0.1+dfsg-1osrf2~trusty2) ...
Removing gazebo5-plugin-base (5.0.1+dfsg-1osrf2~trusty2) ...
Removing libgazebo5:amd64 (5.0.1+dfsg-1osrf2~trusty2) ...
Removing libsdformat2-dev:amd64 (2.3.2-1~trusty) ...
Removing libsdformat2:amd64 (2.3.2-1~trusty) ...
Removing sdformat-sdf (2.3.2-1~trusty) ...
Processing triggers for libc-bin (2.19-0ubuntu6.6) ...
Selecting previously unselected package libsdformat1:amd64.
(Reading database ... 368081 files and directories currently installed.)
Preparing to unpack .../libsdformat1_1.4.11-1ubuntu0.1_amd64.deb ...
Unpacking libsdformat1:amd64 (1.4.11-1ubuntu0.1) ...
Setting up libsdformat1:amd64 (1.4.11-1ubuntu0.1) ...
Processing triggers for libc-bin (2.19-0ubuntu6.6) ...
```

So it looks like gazebo5-plugin-base was removed. Jade will likely have some issues. <br>
Moving on.

Resolve Dependencies Again

```
rosdep install --from-paths src --ignore-src --rosdistro indigo -y
```

Invoke catkin\_make\_isolated (and get some coffee)

```
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
```

Source your setup.bash and you are good to go!

```
source ~/ros_catkin_ws_indigo/install_isolated/setup.bash
```

Make sure to change the ```ros_catkin_ws``` to ```ros_catkin_ws_indigo``` or whatever directory name you used for indigo.


