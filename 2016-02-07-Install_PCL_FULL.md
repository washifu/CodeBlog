---
layout: page
title: Install PCL on wasif in Echidna FULL
---

## Full Instructions to Install PCL (Copy/Pasted from Leo's Wiki Post)


# Configuration Instruction under Ubuntu 14.04 LTS

<pre>
Because of the high demands of PCL library dependence， it will be better to compile the PCL source code.
In the introduction, I will provide the detailed illustration of the dependence choose and compile process.
</pre>

Under Windows or Mac platform, we could download the [VeloView software](http://www.paraview.org/Wiki/VeloView "VeloView") to scan the data directly.
However, the Velodyne LiDAR company doesn't provide the official version for Linux.

# Download the latest PCL version

To install the latest PCL on Ubuntu, +git+ is necessary. Install it by:

> sudo apt-get install git

Change directory to a place where to store the source code, then proceed to clone the PCL repository and create a symbolic link.
(Note:It might be a good idea to create a symbolic link referencing the latest version, this way your application does not have to change when the PCL is upgraded to the next version.)

> git clone https://github.com/PointCloudLibrary/pcl.git pcl-trunk
> ln -s pcl-trunk pcl

Now change directory to pcl you will see the PCL source code. Before proceeding with building the PCL libraries, there'are also some prerequisites needed.

# Install 3rd party Prerequisites

Compiling PCL especially for the VLP sensor requires a host of prerequisites, including +cmake,git+ and several other packages.
I will record the function of the packages step by step to unfold their effects in case of certain needs.
There're 3 sorts of dependence, which are 
(Anything related to the cmake installation please refer to [[PCL based on Ubuntu 14.04]], I have illustrated the steps in detail there.)

# Fundamental part

This one is for the g++,cmake and some essential build dependence, which should be already installed or finished at the Ubuntu PCL environment build before, but still need to check them.

> sudo apt-get install g++ cmake cmake-gui build-essential

# Mandatory part

In this part I will list the package, required for the compilation and usage of the PCL libraries.

Boost, version>=1.46, pcl_*

> sudo apt-get install libboost-all-dev

Eigen, version>=3.0, pcl_*

> sudo apt-get install libeigen3-dev

FLANN, version>=1.71, pcl_*

> sudo apt-get install libflann1 libflann-dev

VTK, version>=5.6, pcl_visualization

> sudo apt-get install libvtk5.8-qt4 libvtk5.8 libvtk5-dev

QT, for the GUI of PCL and can provide some extra choice for the PCL visualizer.

> sudo apt-get install qt-sdk libqt4-opengl-dev openjdk-7-jdk openjdk-7-jre

*+_Warning 1:_+* *PLEASE DON'T* install the VTK6 and QT5 although the trunk version use them or you'll meet a real mass then.
*+_Warning 2:_+* The apt-get install command may generate a window like:

!http://larrylisky.files.wordpress.com/2014/03/missingphonon.png?w=720!

Answer OK to continue, but you may want to follow the recommendation and install any missing components. For the blog editor, he needed a couple of missing components to enable full operation of Phonon:

> sudo apt-get install phonon-backend-gstreamer

> sudo apt-get install phonon-backend-vlc

For the analyze of .pcap file, *_+extremely crucial in this part!!!+_* This is the key point why I have to re-compile the PCL library for more than 4 times.

> sudo apt-get install libpcap-dev

Some other dependence for the compilation:

> sudo apt-get install git-core freeglut3-dev pkg-config

> sudo apt-get install doxygen

> sudo apt-get install mpi-default-dev openmpi-bin openmpi-common

> sudo apt-get install libusb-dev libusb-1.0-0-dev

> sudo apt-get install libgtest-dev

> sudo apt-get install libxmu-dev libxi-dev

> sudo apt-get install graphviz mono-complete

> sudo apt-get install libglew-dev libsuitesparse-dev

# Optional part

This are the optional part in the official page, but out of the VLP numerous dependence, some of them are mandatory in fact. I will mark them.

*COMPULSORY*

Although the listed two dependence are marked as optional in the website but I extremely recommend installing them , so I annotated them as "COMPULSORY".

CUDA part has been finished by @Gary. :-)

QHull >=2011.1 pcl_surface 

> sudo apt-get install libqhull* libqhull-dev


*ELECTIVE*

There're 2 versions, OpenNI 1.x(called OpenNI) and OpenNI 2(called OpenNI2) respectively. In fact, OpenNI is the driver to support Kinect, but I have seen OpenNI includes some libraries related to pcl_io, so I installed OpenNI in case of the IO issue.

* OpenNI

> git clone https://github.com/OpenNI/OpenNI.git openni

And change the branch to unstable:

> cd openni

> git checkout unstable

Run these lines to install OpenNI:

> cd Platform/Linux/CreateRedist/

> chmod +x RedistMaker

> ./RedistMaker

> cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.8.5

Maybe not that version, just change it according to the real condition.

> sudo ./install.sh

(Note: The command line ./RedistMaker will compile several source files. If there was any error, you will not get a ../Redist folder, and therefore, you won’t be able to go to the next step. Fix the build error first and then rerun ./RedistMaker.)

* OpenNI 2

The cmake sometimes will warn that the OpenNI2 library couldn't find. But I searched that the OpenNI2's function is to support the Kinect2, as I have installed the OpenNI before and that one will act for some IO function, and I didn't find any specific problem until now. So, the installation depends on your need.

+However, still some problem now.The cmake is always reporting "missing the OpenNI2 library files", will be fixed later.+
+Recommended not use it now.+

Download the install package from the official website:

> wget http://com.occipital.openni.s3.amazonaws.com/OpenNI-Linux-x64-2.2.0.33.tar.zip

For me, the latest version will be 2.2.0.33 but you can check it on the "OpenNI2":http://structure.io/openni of the newest one.

Decompress it:

> unzip OpenNI-Linux-x64-2.2.0.33.tar.zip

> tar -xjf OpenNI-Linux-x64-2.2.tar.bz2

> cd OpenNI-Linux-x64-2.2

It's same to change the version number so that the command will match the actual one.

Install it:

> sudo ./install.sh

After the generation of ??OpenNIDevEnvironment?? file then add the environment variables of the OpenNI2:

> cat OpenNIDevEnvironment >> ~/.bashrc

So that the path of the Include and Redist folder will be imported to the environment variables: @OPENNI2_INCLUDE@ and @OPENNI2_REDIST@.
(Note: Reboot needed and check it by @echo@ command.)

# Summary

To summarize the compile process, I install the below requests:

> sudo apt-get install libboost-all-dev

> sudo apt-get install libeigen3-dev

> sudo apt-get install libflann1 libflann-dev

> sudo apt-get install libvtk5.8-qt4 libvtk5.8 libvtk5-dev

> sudo apt-get install qt-sdk libqt4-opengl-dev

> sudo apt-get install libqhull* libqhull-dev

> sudo apt-get install libpcap-dev

> sudo apt-get install git-core freeglut3-dev pkg-config

> sudo apt-get install doxygen

> sudo apt-get install mpi-default-dev openmpi-bin openmpi-common

> sudo apt-get install libusb-dev libusb-1.0-0-dev

> sudo apt-get install libgtest-dev

> sudo apt-get install libxmu-dev libxi-dev

> sudo apt-get install graphviz mono-complete

> sudo apt-get install libglew-dev libsuitesparse-dev

> sudo apt-get install openjdk-7-jdk openjdk-7-jre

plus the procedure of OpenNI.

In addition, I have seen such words at the PCL official website:
pcl_* denotes all PCL libraries, meaning that the particular dependency is a strict requirement for the usage of anything in PCL.
So, it's possible to use:

> sudo apt-get install pcl_*

as a complementary of PCL basic dependence.

# Compile and Install PCL

PCL is compiled using the cmake or cmake-gui utility. Change directory to the PCL source directory and enter the following commands:

> mkdir release

> cd release

> cmake -DCMAKE_BUILD_TYPE=None -DBUILD_GPU=ON -DBUILD_apps=ON -DBUILD_examples=ON ..

> make

(Note: For some reason the use of -DCMAKE_BUILD_TYPE=Release will lead to assembler error, but -DCMAKE_BUILD_TYPE=None will build just fine.)

Finally, to install PCL, enter the command:

> sudo make install

The library files are installed at /usr/local/lib, and the header files are installed at /usr/local/include/pcl-1.8 (or whatever the version is).

# Run the test example

[[Velodyne 16 Mapping(VLP-16) Grabber Demo]]

---

# +Reference:+

"INSTALLING AND RUNNING POINT CLOUD LIBRARY ON UBUNTU":http://larrylisky.com/2014/03/03/installing-pcl-on-ubuntu/
"Compiling Point Cloud Library from Source":http://www.pointclouds.org/downloads/source.html
"PCL/OpenNI tutorial 1: Installing and testing":http://robotica.unileon.es/mediawiki/index.php/PCL/OpenNI_tutorial_1:_Installing_and_testing
"PCL/OpenNI troubleshooting":http://robotica.unileon.es/mediawiki/index.php/PCL/OpenNI_troubleshooting#OpenNI
"Sina Blog_Openni2+libFreenect under Ubuntu":http://blog.sina.com.cn/s/blog_4a628f030102vb9a.html
