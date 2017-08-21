---
layout: page
title: Custom PCL Programs [for NOOBs like me]
---

## (Pre-Requisite: Install PCL on wasif in Echidna FULL)

We will create a program called wasifs_vlp_viewer

First cd into your PCL root directory
 
```
cd PCL/pcl-trunk/
```
 
Create a new git branch so not to ruin the existing version
 
```
git checkout -b sandbox
```

Use cmake to make all the necessary Makefile, CMakeLists.txt, CMakeFiles directories and files

```
cmake .
```

The program I want to modify is pcl_vlp_viewer.cpp
cd to this file location

```
cd visualizations/tools/
```

Open with some editor and save a new file named wasifs_vlp_viewer.cpp with your edits

```
sublime pcl_vlp_viewer.cpp
```

We must include this in the make process
Edit the CMakeLists.txt

```
sublime CMakeLists.txt
```

Find the lines for pcl_vlp_viewer.cpp, copy/paste, and change pcl to wasifs
Then cd back to the PCL root directory and make
Use the -j8 flag if you have a GPU to avoid waiting for an hour

```
$cd ../../
$make -j8
```

cd into the bin and you will see your executable

```
$cd bin/
$ls
$./wasifs_vlp_viewer
```

This method can be used for making any custom PCL program!

  
 
 
 
 
