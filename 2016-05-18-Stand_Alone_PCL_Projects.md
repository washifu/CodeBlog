---
layout: page
title: Stand-Alone PCL Projects
---

I have been making files within versions of my PCL git repository and adjusting the CMakeList.txt for the subdirectory to sneakily compile my custom programs. This is a safe way to avoid library linking issues, as PCL has a plethora of headers and many wonderful functions across its many files.

However, this is a guide for a cleaner way to build small stand-alone projects that use PCL libraries.

First make some directory to house your project

```
mkdir pcl_project
```

Steal your favorite example or code snippet from PCL and save it in your project root directory. <br>
Edit the name to match the project directory name. (pcl_project.cpp)

Then create a CMakeLists.txt in your project directory that looks like the following:

```
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

project(pcl_project)

find_package(PCL 1.3 REQUIRED)

include_directories(${PCL_INCLUDE_DIRS})
link_directories(${PCL_LIBRARY_DIRS})
add_definitions(${PCL_DEFINITIONS})

add_executable (pcl_project pcl_project.cpp)
target_link_libraries (pcl_project ${PCL_LIBRARIES})
```
The tricky part is ```target_link_libraries``` which seem to take pre-defined variables. In general, this usually follows the directory hierarchy of PCL, for example ```${PCL_LIBRARIES} ${PCL_VLP_VIEWER} ${PCL_IO} ${PCL_COMMON} ${PCL_VISUALIZATION}``` etc.
See this link for [PCL's official guide on this](http://www.pointclouds.org/documentation/tutorials/using_pcl_pcl_config.php "PCL").

Then just create a ```build``` directory, cd into there and let cmake do its magic.

```
$mkdir build
$cd build
$cmake ..
```

You should now have a Makfile which will compile your program and link against the right PCL libraries.

```
make
```

After which you smile when you see:

```
[100%] Building CXX object CMakeFiles/pcl_project.dir/pcl_project.cpp.o
Linking CXX executable pcl_project
[100%] Built target pcl_project
```

Now, if you run into issues where a certain header file cannot be found (for example, for some odd reason my compiler kept saying vlp_grabber.h does not exist while hdl_grabber.h does, even though they were in the same folder!), you need to realize that in your stand-alone project you are not actually linking to the libraries located in the pcl-trunk git repository.
Instead, you are linking to header files stored in ```/usr/local/include/pcl-1.8/pcl/```. Therefore, if you cannot link to header files or "new" header files, make sure you go back to you pcl-trunk git repo and 

```
$make clean
$sudo make -j8 install
```

This will store all the headers in ```/usr/local/include/pcl-1.8/pcl/``` <br>
By the way, the ```-j8``` just uses all 8 logical cores to thread through the compiling faster, which, even then, takes 15 minutes.

Now you can make Stand-Alone Projects for quick and dirty testing!
