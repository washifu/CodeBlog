---
layout: page
title: Converting .pcap to .pcd
---

(Pre-requisite: See ROS Indigo Installation)

Start with sourcing ROS indigo.

```
source ~/ros_catkin_ws_indigo/install_isolated/setup.bash
```

Getting the [ros velodyne](http://wiki.ros.org/velodyne/Tutorials/Getting%20Started%20with%20the%20HDL-32E "ROS")

```
sudo apt-get install ros-indigo-velodyne
```

So this didn't really work. <br>
I will manually [install the stack](http://answers.ros.org/question/89986/installing-stacks-in-hydro/ "ROS")

## Create a velodyne workspace

```
$ mkdir -p ~/velodyne_ws
$ cd ~/velodyne_ws
```

Creating rosinstall file for velodyne

```
rosintsall_generator --wet --exclude RPP --deps velodyne > velodyne.rosinstall
```

Compiling and making the velodyne.rosinstall

```
$ wstool init src velodyne.rosintall
$ catkin_make
```

When running from this workspace, source manually.

```
source ~/velodyne_ws/devel/setup.bash
```

## [Converting .pcap to .pcd](http://www.pcl-users.org/Convert-pcap-to-pcd-Velodyne-HDL32E-td4034165.html "Some Silly Blog")

Now that our environment is finally ready, let's convert a file!

Open a new terminal and source to velodyne workspace.

```
source ~/velodyne_ws/devel/setup.bash
```

Run velodyne_pointcloud to open the .pcap file as a point cloud in ROS (no visualization)

```
roslaunch velodyne_pointcloud 32e_points.launch pcap:=path/of/your/file.pcap
```

Open a new terminal and source to velodyne workspace. <br>
Run the pointcloud_to_pcd to effectively convert that ROS pointcloud that's currently open to a pcd file.

```
rosrun pcl_ros pointcloud_to_pcd input:=/velodyne_points _prefix:=path/to/save/it.pcd
```
