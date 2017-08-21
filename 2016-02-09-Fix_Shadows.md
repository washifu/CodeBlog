---
layout: page
title: Fix Shadows
---

When the openni_grabber demo is run with any VLP-16 data set, two (or more) copies of all the points appear above/below the first set of points. The points are not vertical translations but appear to be angular translations from the LiDAR origin.

It seems that the openni\_grabber demo is designed for HDL-32 data sets as described in [The Velodyne High Definition LiDAR HDL Grabber](http://pointclouds.org/documentation/tutorials/hdl_grabber.php#hdl-grabber "PCL: HDL Grabber") page by PCL.

All the data sets I've collected with the Velodyne VLP-16 have these shadows as do the sample data sets Leo has left behind. <br>
I ran some HDL-32 sample data sets and confirmed no occurence of these shadows, so it seems that the program is calibrated for the HDL-32 and not the VLP-16.

<del>There doesn't seem to be a calibration file for VLP-16 but</del> PCL does actually have a *vlp_viewer.cpp* which makes use of *vlp_grabber.cpp* in the library. <br>
I have found the VLP-16 and HDL-32 calibration files thanks to Tsukasa Sugiura's comment on his video of [Drawing VLP-16 Data using PCL](https://www.youtube.com/watch?v=7BUFxkyH1r0 "YouTube Video").

*vlp_viewer*

> visualization/tools/vlp_viewer.cpp

*vlp_grabber*

> io/src/vlp_grabber.cpp

### Next Step: 
  * Compile these files. [DONE]
  * If too hasslesome, try to make the HDL Grabber compatible with VLP-16. [DONE]
  * I am still curious about the other programs, but will leave compiling those as secondary tasks. [DONE]
  
After troubleshooting the vlp\_viewer from PCL, I have realized that compiling this program outside of the PCL repository leads to many build issues. Henceforth I will just make a branch in the PCL repository and make adaptations to the programs and run the programs from the `pcl-trunk/x86_64/bin` folder.  
