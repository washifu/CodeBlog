---
layout: page
title: Install PCL on macOS
---

### Install PCL ###

[Point Cloud Library's Mac Installation Page](http://pointclouds.org/downloads/macosx.html)

I installed with [Homebrew](http://www.pointclouds.org/documentation/tutorials/installing_homebrew.php):

Prepare Homebrew (if you have not already):

```
brew update
brew tap homebrew/science
```

Install PCL with Homebrew:

```
brew install pcl --with-examples --with-openni --with-openni2 --with-qt5 --with-surface_on_nurbs
```
