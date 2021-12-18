---
title: NVIDIA
description: 
published: true
date: 2021-12-17T16:31:41.977Z
tags: 
editor: markdown
dateCreated: 2021-12-17T16:31:33.080Z
---

## Guide for NVIDIA GPUs

To manually install NVIDIA drivers, you need to first install the kernel headers and development libraries to automatically build the kernel modules.

For the default kernel, you can simply install the NVIDIA Driver group
```
sudo dnf group install nvidia-driver
```

The NVIDIA driver then should be installed automatically. All you need to do is reboot the system.

If the NVIDIA driver is still not available, make sure you're on the latest version of the kernel since the module will be built for the newest available kernel w/  development libraries.

## NVIDIA Optimus

NVIDIA Optimus is a feature that allows you to switch between your CPU's integrated graphics processor and the discrete NVIDIA graphics card, it is designed to improve performance and reduce power consumption on laptops.

If you happen to own a laptop with NVIDIA Optimus, you can follow the steps below to set your NVIDIA GPU as the default graphics processor:

Copy the example X.org configuration for NVIDIA
```
sudo cp -p /usr/share/X11/xorg.conf.d/nvidia.conf /etc/X11/xorg.conf.d/nvidia.conf
```
Then edit the file by using your favorite text editor (example: nano):
```
sudo nano /etc/X11/xorg.conf.d/nvidia.conf
```

Make sure to add this line:

```
Option "PrimaryGPU" "yes"
```

Your configuration should look like this:
```
#This file is provided by xorg-x11-drv-nvidia
#Do not edit

Section "OutputClass"
	Identifier "nvidia"
	MatchDriver "nvidia-drm"
	Driver "nvidia"
	Option "AllowEmptyInitialConfiguration"
	Option "SLI" "Auto"
	Option "BaseMosaic" "on"
	Option "PrimaryGPU" "yes"
EndSection

Section "ServerLayout"
	Identifier "layout"
	Option "AllowNVIDIAGPUScreens"
EndSection
```