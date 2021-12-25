---
title: Music Production on Ultramarine Linux
description: 
published: true
date: 2021-12-25T09:26:18.658Z
tags: use case, hardware, software
editor: markdown
dateCreated: 2021-12-25T09:16:59.153Z
---

# Music Production / General Audio
Ultramarine Linux can be quickly set up as a Digital Audio Workstation (DAW) for Music Production or any general audio editing use case. While the resulting setup may not be up to industry standards, it can be generally used to to basic (and some advanced) Audio Engineering tasks.


## Disclaimer
Unfortunately due to the way the music industry works, you may encounter the following issues while setting up your workstation:

- DRM (Software Licensing and activation issues) Ex: iLok DRM, iZotope Plugins, Native Access
- Hardware Support (No drivers, etc.)
- Software Support (Some software do not work well under Ultramarine/Fedora specifically, or Linux in general)

While we unforunately do not have any way to fix these issues, there are workarounds for some of them.

## Getting Started

To get started on setting up a DAW running Ultramarine Linux, you should download the `realtime-setup` package for real-time audio performance.
```
sudo dnf install realtime-setup
```
Then add yourself to the `realtime` group
```
sudo usermod -aG realtime <YOU>
```
### Custom kernel
You may also want to use a custom kernel for additional real-time performance, we recommend using [XanMod](https://xanmod.org/) for audio production.
```
sudo dnf copr enable rmnsncnce/kernel-xanmod fedora-35-x86_64
```

Install the real-time kernel if you only want real-time optimizations (Do not install this if you use an NVIDIA graphics card!)
```
sudo dnf install kernel-xanmod-rt
```
...Or use the EDGE version for latest updates.
```
sudo dnf install kernel-xanmod-edge
```


---

If you simply want the Fedora Jam packages (for native Linux audio production), simply install the Audio Production group featured in the Fedora repository
```
sudo dnf group install "Audio Production"
```

You can also use the [Audinux](https://copr.fedorainfracloud.org/coprs/ycollet/audinux) repository for even more audio software.

After setting up these packages, reboot your computer and continue with the following guides as you see fit.

## Windows > Linux Plugin bridging
> iLok may break due to the way that the DRM works with hardware IDs. It will work the first time but iLok will stop working once you install it in another prefix. It is not recommended to use iLok plugins under Wine due to this.
{.is-warning}

> While we do not condone piracy, some of the DRM issues can be worked around by simply installing a pirated version of the plugin, mostly for iZotope and iLok plugins.
{.is-info}


Windows VST plugins do not support Linux, but we can use [Wine](https://en.wikipedia.org/wiki/Wine_(software)) to run some of them.

To do this, first install Wine
```
sudo dnf install wine
```
Then install the desired plugins to your desired location.

After that, use [yabridge](https://github.com/robbert-vdh/yabridge) to bridge the Windows-only plugins for Linux. The builds for yabridge are available in [Patrick Laimbock's Copr repository](https://copr.fedorainfracloud.org/coprs/patrickl/yabridge/).
```
sudo dnf copr enable patrickl/yabridge
sudo dnf install yabridge
```

The configuration guide for yabridge is available in the GitHub repository linked above.

### Xfer Serum
Serum may not display graphics properly. To fix this you have to disable Direct2D by disabling the `d2d1.dll` from loading in the Wine configuration.

### Spire
Spire has the opposite requirements to Serum to work properly, It is recommended to install Spire in a seperate Wine prefix instead if you want to use both.

### Native Instruments plugins
Native Access will try and fail to download installer images for its respective plugins, To work around this you will have to download the installer images and install them manually.
#### Kontakt
Kontakt will freeze using default memory lock settings. To fix this you have to follow the Getting Started guide on the top of this page.

### iZotope plugins
Unfortunately, there is no workaround to the iZotope authentication scheme. You have to either try offline authentication or use a patched version of the plugins.

### XLN Audio plugins
XLN Audio plugins mostly will work out of the box, simply follow the Windows installation guide to install them.

### Vital
Vital does have a Linux version, but it is made only for Debian. The Debian version will have issues regarding TLS authentication on other Linux distributions. It is recommended you use either the [Vitalium](https://github.com/DISTRHO/DISTRHO-Ports/tree/master/ports/vitalium) source port or install the Windows version instead.

## DAWs
You can use most Windows DAWs on Ultramarine Linux using Wine. However there are also DAWs that are natively supported on Linux:
- Bitwig Studio (Packaged in the Ultramarine Repositories)
- REAPER
- Ardour/Mixbus
- LMMS
- Rosegarden
- Qtractor
- Tracktion Waveform
- Audacity (Basic audio editing only)
- MusE

### FL Studio
FL Studio should work out of the box when running under Wine. (Not Tested)

### Ableton Live
Ableton Live has issues with online authentication. You need to set up offline authentication to activate Ableton Live.

