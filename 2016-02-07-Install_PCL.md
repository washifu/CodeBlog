---
layout: page
title: Install PCL on wasif in Echidna
---

### Install PCL ###

So far I have followed the excellent advice from Leo in his [PCL Configuration Instrutions Under Ubuntu 14.04 LTS](http://222.200.163.209/wiki/projects/pointcloudsvlp/wiki/Configuration_Instrucion_under_Ubuntu_1404_LTS)
<br>But I ran into this problem when installing PCL:

```
In file included from /home/wasif/PCL/pcl-trunk/release/apps/ui_manual_registration.h:26:0,
                 from /home/wasif/PCL/pcl-trunk/release/apps/include/pcl/apps/../../../../../apps/include/pcl/apps/manual_registration.h:37,
                 from /home/wasif/PCL/pcl-trunk/release/apps/include/pcl/apps/moc_manual_registration.cpp:9:
/usr/include/vtk-5.8/QVTKWidget.h:40:25: fatal error: QtGui/QWidget: No such file or directory
 #include <QtGui/QWidget>
                         ^
compilation terminated.
make[2]: *** [apps/CMakeFiles/pcl_manual_registration.dir/include/pcl/apps/moc_manual_registration.cpp.o] Error 1
make[1]: *** [apps/CMakeFiles/pcl_manual_registration.dir/all] Error 2
make: *** [all] Error 2
```

Solution: Downgrade Qt5 to Qt4. (PCL, as of 2016年 2月 8日, does not fully support Qt5)
