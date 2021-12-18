---
title: Composing Ultramarine
description: 
published: true
date: 2021-12-17T16:37:13.066Z
tags: 
editor: markdown
dateCreated: 2021-12-17T16:37:06.470Z
---

# Composing Ultramarine

Ultramarine uses Red Hat's [Pungi](https://pagure.io/pungi) for OS composition.

To start composing the whole tree, you need to already be running a copy of Fedora, Ultramarine or any member of the EPEL family (RHEL, Rocky, AlmaLinux, etc.) and have a working Pungi installation.

To properly make use of this documentation, you need to already be a part of the Ultramarine release team.

## SOP for production compose
1. Connect to the Andes server and make sure you have root access.
```
ssh lapis.ultramarine-linux.org
```
2. The compose scripts are located in `/srv/uml-pungi/`, make sure to run `git pull` to fetch the latest changes, and then run `sudo ./compose.sh <label flags>`. The composition will be started, it is recommended to run this script in a terminal multiplexer like `screen` or `tmux`. If the script fails, please try and troubleshoot it then report to Cappy Ishihara, or one of the members in the release engineering team for assistance.

## Building a local compose
First, make sure you have been given access to the internal VPN, and have mounted the Koji files at /mnt/koji.

Then clone the `ultramarine-pungi` repo from the GitLab repository.

Then, run the following command to build a local compose:
```
sudo pungi-koji --config ultramarine.conf <additional pungi flags>
```
Refer to the [Pungi documentation](https://docs.pagure.org/pungi/) for instructions on how to use Pungi.

## Common issues
- Live Media composition will get stuck. this is becuase you need to actually set the install tree to Everything.
- The install tree will not be considered final unless you set the label to RC-* or add the --supported flag to the compose, the current way we sidestep this for live media composes is to make Kickstart generate a dummy "final" buildstamp to avoid the warning.

