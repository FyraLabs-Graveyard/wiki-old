---
title: Manually Installing Ultramarine
description: 
published: true
date: 2021-12-22T04:29:56.569Z
tags: advanced, install
editor: markdown
dateCreated: 2021-12-19T08:22:12.381Z
---

# Manually installing Ultramarine Linux

If you'd like a more custom install of Ultramarine, you can pick 3 different ways to make your own installation of Ultramarine Linux

## Using the Network Installer

The network installer, also internally called the “Everything” variant, is a bootable installer ISO file containing the plain install tree for Ultramarine Linux.

It also serves as the main software repository for all software in Ultramarine Linux, and is configured to override all software that already existed in Fedora.

To use the network installer, download the bootable ISO image and flash it to a flash drive or burn it to a disc following the [Standard installation guide](/guide/installation).

### Installing without the Ultramarine Network installer

You can also use the Fedora Everything installer, the Fedora Server installer, RHEL installer, Rocky Linux installer, or an installer for any Linux distribution that uses the [Anaconda Installer](https://en.wikipedia.org/wiki/Anaconda_(installer)).

To do this, simply use said installer and then point the installation source to the Ultramarine repositories. You should also set up additional repositories for Fedora itself and RPMFusion, as Ultramarine in itself is not self-hosting at the moment.

## Building your own image

To build your own custom image of Ultramarine, you can use the standard [Kickstart scripts](https://gitlab.ultramarine-linux.org/release-engineering/build-scripts) as a starting point. Kickstart is a scripting language for the Anaconda installer that allows for custom unattended installations using the Anaconda installer. It can also be scripted in a way to be used to create a custom image.

To get started, please refer to the [PyKickstart Documentation](https://pykickstart.readthedocs.io/en/latest/kickstart-docs.html).

To build an image, clone the kickstart files from the Ultramarine GitLab

```plaintext
git clone https://gitlab.ultramarine-linux.org/release-engineering/build-scripts.git
```

Then, edit the files as you see fit. After that you can either go 2 ways:

### Using Onceler to build the image

Onceler is a simple wrapper for Lorax that can be used to quickly build live images from kickstart files. 

To use it, simply edit the given `onceler.cfg` file, then run Onceler

```plaintext
sudo onceler --variant <variant>
```

### Using the standard Live Media Creator

After you have finished configuring your kickstart configuration, flatten your kickstart files to a single file.

```plaintext
ksflatten -c <input.ks> -o <output.ks>
```

Then, use the Live Media Creator to build the image.

**Example Command:**

---

```plaintext
sudo livemedia-creator --make-iso\
 --ks ultramarine-pantheon-flattened.ks\
 --project "Ultramarine-Pantheon-Live"\
 --releasever 35\
 --no-virt\
 --macboot\
 --iso-only\
 --resultdir result/
```

For more instructions on how to use the Live Media Creator, please refer to the [Lorax Documentation](https://weldr.io/lorax/lorax.html).

## Manually bootstrapping Ultramarine using DNFStrap

> WARNING: This method is only recommended for advanced users only. We are not responsible for broken installations, lost data, and broken components when using this method. You have been warned.
{.is-warning}

If you would like to take it to an another step and install Ultramarine manually like an [Arch Linux installation](https://wiki.archlinux.org/title/installation_guide), or want an even more minimal installation for whatever purpose, You can manually install Ultramarine using DNFStrap.

To get started, you need to make sure you have DNF installed in your system of choice, be it [Arch Linux](https://aur.archlinux.org/packages/dnf/), [Debian](https://packages.debian.org/bullseye/dnf), or any other OS, including Fedora.

You need to also need to have Git installed to download the bootstrap script.

First, set up the partitions using your favorite partitioning tool. For simplicity, we'll be using blivet-gui to edit our partitions.

If you're using EFI, set up an EFI partition now.

If you're sure you'll be wiping the entire disk and using a singular root partition, you can skip this part and use DNFStrap's built in auto partitioning and setup by cloning the repository and running:

```plaintext
git clone https://gitlab.ultramarine-linux.org/extras/dnfstrap.git && cd dnfstrap && sudo ./dnfstrap -r 35 -d /dev/sda -i ultramarine-release-basic
```

This will install Ultramarine with a F35 base and automatically format your disk with an EFI partition.

If you want a custom configuration, you'll have to continue with the instructions below.

### Partitioning Setup

Create a small EFI partition to store all the EFI bootloader data at the beginning of the disk.

![](/advanced-setup/efi.png)

Then, create a main root partition for the system.

![](/advanced-setup/root.png)

Optionally, you can also create a seperate home partition for personal data.

Once that's done, apply your changes by clicking the checkmark button on the top bar.

---

Now the following steps will require you to use the terminal. Please make sure you already have experience using the command line before following this method.

Create an empty folder at the location of where you would like to mount your system.

```plaintext
sudo mkdir -p /mnt/sysroot
```

Mount the partitions you have just created to the folder you have created. You can look at the list of your block devices (partitions) using `lsblk`.

```plaintext
sudo mount /dev/sda2 /mnt/sysroot
```

For EFI users, you have to also mount your EFI partition.

```plaintext
sudo mkdir -p /mnt/sysroot/boot/efi
sudo mount /dev/sda1 /mnt/sysroot/boot/efi
```

### Bootstrapping Ultramarine

Make sure you have DNF installed before proceeding any further.

Clone the DNFStrap script from the Ultramarine Gitlab.

```plaintext
git clone https://gitlab.ultramarine-linux.org/extras/dnfstrap.git && cd dnfstrap
```

Now start the bootstrapping process. This command below will install Ultramarine based on Fedora Linux 35 at the specified chroot.

```plaintext
sudo ./dnfstrap -r 35 -c /mnt/sysroot -i ultramarine-release-basic -i -fedora-release*
```

We assume you already have knowledge on how to generate a file system table, so please set up your /etc/fstab after bootstrapping your installation.

**Example File System Table in a virtual machine:**

```plaintext
/dev/vda2       /               ext4    defaults        0 0
/dev/vda1       /boot/efi       vfat    umask=0077,shortname=winnt 0 2
```

It is recommended you change the direct device paths to the partition's UUID instead.

### Installing the Linux kernel

Due to the way that Fedora/Ultramarine works, you need to install the kernel with, or after the bootloader has been installed.

DNFStrap will ask you which kernel to install automatically.

But however, if you have a custom boot configuration, simply install the vanilla kernel by running this in the chroot:

```plaintext
dnf install kernel
```

### Setting up the bootloader

To set up the GRUB Bootloader, chroot into the installation and install GRUB.

To install GRUB on an x86\_64 machine, run the following:

```plaintext
dnf install shim grub2-efi-x64 grub2-tools-efi
```

Then check the UUIDs of your partitions using `blkid`

```plaintext
blkid
```

Get the UUID of your root partition, then make a GRUB config file at `/boot/efi/EFI/fedora/grub.cfg`

Make sure the contents are like this:

```plaintext
search --no-floppy --fs-uuid --set=dev <UUID>
set prefix=($dev)/boot/grub2

export $prefix
configfile $prefix/grub.cfg
```

Replace the `<UUID>` part with the UUID of your root partition.

Do the same for the boot entries after you installed the kernel.

You can do this by removing the root entry in your GRUB configuration, then adding it again with the correct UUID using `grubby`.

```plaintext
rootdisk=$(df -P / | sed -n '$s/[[:blank:]].*//p')
grubby --remove-args="root" --update-kernel ALL
grubby --remove-args='rd.live.image' --update-kernel ALL
grubby --args "root=UUID=$(blkid -o value -s UUID $rootdisk) rw quiet" --update-kernel ALL
```

Then generate a GRUB config

```plaintext
grub2-mkconfig -o /etc/grub2.cfg
grub2-mkconfig -o /etc/grub2-efi.cfg
```

And that's it! You have successfully installed Ultramarine Linux manually. Now you can install additional packages and set up your installation.

### Notes for post-installation

Please set up your SELinux settings, or disable it to be able to actually use the system. Graphical logins may fail when installing a DE and you should try `chown`ing your home directory and setting your SELinux contexts if this were to happen.

You can set the contexts by using `restorecon -R /path/to/folder/`