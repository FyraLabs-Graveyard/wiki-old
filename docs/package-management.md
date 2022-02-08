---
title: Managing Software
description: 
published: true
date: 2022-02-08T01:38:07.455Z
tags: 
editor: markdown
dateCreated: 2022-02-08T01:06:13.442Z
---

# Managing Software
Most operating systems manage software using a set of packages that can be updated and tracked easily.


# Package Managers
Package managers are a collection of software that automates the process of installing, uninstalling and managing software. There are also responsible for managing and resolving dependencies for each individual software, similar to an app store.
> Package managers actually precede app stores as a form of software management. In fact, most app stores are actually a frontend to the underlying package manager.
{.is-info}

## Native packages
This is the most low-level form of package management. This is usually what the main operating system uses to manage software and its dependencies.

Ultramarine Linux mainly uses the DNF Package Manager. Which in turn relies upon the RPM package manager for low-level tasks. Other packages for other operating systems/Linux distributions include:
- Arch Linux uses the `pacman` package manager for managing its own packages, which can be found in their own repositories and the AUR.
- Debian and Ubuntu uses the APT package manager, which relies on DPKG (The Debian Package Manager) to manage its own software.
- macOS uses its own PKG format to manage apps.
- Android uses APK for installing apps.
- iOS uses IPA packages to install apps.
- Windows has their own version of packages in the form of MSI installers and UWP Apps/AppX packages.

## Flatpaks
Flatpak is a distribution-agnostic package management solution. Its packages are fully sandboxed and require permissions to proceed.

## AppImages
AppImages are an another form of distro-agnostic packages. Its package are served in a single binary which means that the end user does not need to install anything else to use the software.

## Snaps
Snap is a sandboxed package solution by Canonical. They are similar to Flatpaks, but currently only has 1 centralized repository from Snapcraft. It is the secondary package manager for Ubuntu.