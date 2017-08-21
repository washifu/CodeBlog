---
layout: page
title: A Better Way of Converting .pcap to .pcd
---

I made a program using PCL tools and compiled it as wasif_vlp_viewer.

This program effectively allows the user to 
- feed in a .pcap file, type in the name of an output directory
- choose the time in between each .pcd frame [minimum of 0.1 seconds]
- and the output directory will be automatically created with all the .pcd frames saved into that directory.

It is best not to interact with the 3D viewer while the program runs to avoid pausing and disrupting the framerate. <br>
The viewer can also display .pcap files without recording .pcd

Checkout Echidna for wasif\_vlp\_viewer.cpp

I won't upload the contents of the file--to avoid a clever yet unethical master student from snagging away my hard efforts ;)
