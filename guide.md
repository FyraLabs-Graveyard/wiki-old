---
title: Guide
description: 
published: true
date: 2021-12-21T19:12:21.529Z
tags: 
editor: markdown
dateCreated: 2021-12-17T16:14:46.992Z
---

## What Should I Know About Linux Before I Pick My First Distro?

[Linux is very diverse](https://en.wikipedia.org/wiki/List_of_Linux_distributions), it is often very common for people to get overwhelmed by the sheer choice it has to offer.

We advise you to look at Linux as it's own thing, and not something like "Windows but better/worse" or "MacOS but better/worse". They have their own target audience, have their own issues and their own strengths. You are obviously entitled to your opinion and we respect all communities and their opinions.

Linux is obviously not perfect for all, there is no distro which is "all size fits one", although some distros may attempt to act as such, in reality, it often may prove difficult to keep all sides satisfied with that approach.

"Freedom" in Linux is a very vague term as well. You are "free" to use it, "free" to do whatever you want with it and "free" to or not to contribute towards the project. Linux is a community project after all, which happens to be backed by large corporations which help keep it afloat and also contribute towards the project just like others, but perhaps on a different scale.

## Fundamentals And Linux Nomenclature To Get You Started

We will be discussing things like what a Desktop Environment(from here on referred as DE) is, how it will affect your experience, what is Xorg (also interchangeable mentioned as X11) or Wayland, state of Nvidia and its drivers, etc. 

Since it will be difficult to explain everything in great detail, we will be trying to compress the information provided by focussing on the main highlights, while also redirecting you to better sources if you are interested in reading or learning further.

This is a guide for beginners after all.

## What is "Desktop Environment" And How Should It Affect Me?

To put it simply, a Desktop Environment (from here on referred as DE) is suite of tools and software, ranging from your Window Manager(Discussed further later), all the way to your file manager and even your text editor. Consider it as more of an ecosystem consisting of various pieces of utilities, if you will.

It comes pre-installed on most linux distros and comes in many varieties, but the most common one is GNOME, which is usually the default DE on most distributions.

It provides you with an environment to work in and is the face of your linux installation. Some can be very minimalistic, focused on having the least footprint on your system resources by as much as possible, to some giving fine control over the settings and looks of your interface, so on and so forth.

As with most things in Linux (and Open Source in general), there are politics involved here as well. You are advised to use whatever DE you feel comfortable with, and if you are not sure, you can always try out a few and see which one works for you.

**Ultramarine Linux ships with the following DEs:**

1. [Budgie](https://en.wikipedia.org/wiki/Budgie_(desktop_environment)) (Flagship Edition)

2. [Cutefish](https://cutefishos.com/)

3. [Pantheon](https://en.wikipedia.org/wiki/Elementary_OS) (Upcoming)

**Some other popular DEs**

1. [KDE](https://kde.org/)

2. [GNOME](https://www.gnome.org/)

3. [Cinnamon](https://en.wikipedia.org/wiki/Cinnamon_(desktop_environment))

***READ MORE HERE:***
1. [Arch Linux Wiki](https://wiki.archlinux.org/index.php/Desktop_environment)

## How Do I Install A Program? What Is A Package Manager? What are "Software Repositories"?
There are various ways of installing a program on Linux.

Broadly speaking, software can be installed in 2 ways- by installing "Binaries" or "Compiling from Source"

### Compiling from source

When you compile from source, you essentially take the various instructions involved (which may include but not limited to scripts, configuration files, etc) and "Build" your program using the various "blueprints" and "schematics" provided. Think of it as building your own PC, car, etc from scratch.

A few of the most common build systems include Meson, CMake, and GNU Make.

Generally, this method is used if an executable is not provided by the developer, or if you have applied some custom changes to the software which is otherwise not available to download directly. 

> This is generally for advanced users and developers. You should try to find a packaged version of the software you want and only use this as a last resort if your required software is not packaged for your system, or the current package version does not satisfy your requirements. 
{.is-info}

Most software will come with documentation and instructions on how to manually build it from source, please refer to said documentation or ask the author of that software for instructions on building it.


### Binaries

Binaries, however, are ready-to-use and pre-compiled, meaning you can simply execute them with a command or a click.

This is analogous to running a `.exe` file for Windows or a `.dmg` file for OS X.

> Ultramarine Linux is a RPM(Red Hat)-Based distribution, meaning you can load `.rpm` files. Generally, everything based on Debian (or rather Ubuntu) Uses the Debian Package Manager, meaning they use `.deb` packages to install programs. Do not run `.deb` packages in Ultramarine Linux.
{.is-info}


Various ways to install a specific program are discussed further below.

The most common way to install programs on Linux is by using your **package manager**.

### Package Managers

The package manager, as the name suggests, manages the packages installed on your system. These often include dependencies, weak dependencies and the program itself.

Examples of other non-Linux package managers include Windows' modern (Windows 8+) apps, iOS apps, Android's APK, `pip` for Python libraries, NPM for Node.js libraries, and Homebrew for macOS.

> Dependencies are the programs required by the software in order to function properly, these are generally installed when you are installing a package.
{.is-info}


A package manager by default queries your distribution's repositories to install programs.

This library of programs can be further expanded by adding additional repositories
There are various ways software can be distributed on Linux, however, the various methods to install a software 

> Ultramarine Linux's default package manager is [DNF](https://en.wikipedia.org/wiki/DNF_(software)).
{.is-info}


### Software Repositories
Repositories are a place where one can find a list of software they can install, an app store is a modern example of a software repository.

### Various Ways To Install An Executable In Linux

Linux comes with a plethora of ways to install an executable.

Broadly speaking, they can be either "sandboxed", or use your system's pre-installed dependencies.

In our case, the latter one is in the form of `.rpm` files.

#### RPMs

RPM Stands originally stood for the Red Hat Package Manager, however, it is now a recursive acronym.

This is generally what is in the main repositories of our linux distribution.

These programs rely on your system's pre-installed dependencies.

Experience using these executables is highly customizable and generally can be tuned to your heart's content

> In order to actually modify the code, you will need SRPMs (short for source RPMs). These will not be discussed in this guide as it lies beyond the scope of this guide. Please refer to the [Packaging Guide](/guide/contributing/building) for more information on building packages.
{.is-info}

These files are ubiquitous and easy to find for a given program, unless the package itself is proprietary software.

These programs will (most of the time) respect your system theme and configuration.

#### AppImages

>AppImages are a format for distributing portable software on linux without needing superuser permissions to install the application. Source: Wikipedia

> Superuser, `sudo` previliges, or Root privilieges are the highest order of access a program can have access to. This is analogous to Adminstrator privileges on Windows, this also applies on other UNIX-based Operating systems such as macOS.
{.is-info}

> It is **_NEVER_** recommended for a user to ever use the root user as their main account. As the root user will have unrestricted privilieges over everything on the system. This is a major security risk and most software will try to stop you from running them as root anyway.
{.is-warning}




