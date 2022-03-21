---
title: Composing Ultramarine
description: 
published: true
date: 2022-03-21T14:39:08.773Z
tags: 
editor: markdown
dateCreated: 2021-12-17T16:37:06.470Z
---

# Composing Ultramarine

Ultramarine uses Red Hat's [Pungi](https://pagure.io/pungi) for OS composition.

To start composing the whole tree, you need to already be running a copy of Fedora, Ultramarine or any member of the EPEL family (RHEL, Rocky, AlmaLinux, etc.) and have a working Pungi installation.

To properly make use of this documentation, you need to already be a part of the Ultramarine release team.

## SOP for new compose
1. Connect to the Andes server and make sure you have root access.
```
ssh 68.162.241.30 -p 11002
```
2. Clone the `ultramarine-pungi` repo from the GitLab repository.

3. Enter the repo, then run one of the compose scripts in the repository as root.

`test-compose` is for non-release versions, `compose` is for new production version, and `updates-compose` is for updates for the existing release version

```
sudo ./compose.sh <pungi flags>
```

So for a RC1.0 release, you run
```
sudo ./compose.sh --label RC-1.0
```