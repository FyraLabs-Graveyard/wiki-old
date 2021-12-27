---
title: SOP: Contingency Plans
description: Contingency plans for Distro-wide disasters
published: true
date: 2021-12-27T10:39:37.383Z
tags: 
editor: markdown
dateCreated: 2021-12-27T10:39:37.383Z
---

# SOP: Contingency Plans
> This document is made for and will only be useful to the core Ultramarine Linux Team only. It contains information in cases of disasters that affect the Ultramarine Linux infrastructure, or team. In case of this project's discontinuation, you can also follow this guide to make your own full fork of Ultramarine Linux.
{.is-info}

# Infrastructure-related disasters
## Main Server down
In case the main build system/repository server is now out of service, you should first try recovering the disk image/data from the affected server. If this is impossible however, you will have to [Set up Koji from scratch](https://docs.pagure.org/koji/server_howto/#), then follow the following instructions:

1. Install `umpkg` from https://gitlab.ultramarine-linux.org/release-engineering/umpkg.
2. Set up your Koji configuration to point to the new server, and all the rebuild scripts.
3. Rebuild every single package in the `dist-pkgs` group using umpkg by using the Mass Rebuild script.

This should rebuild everything from source. However this may take a long amount of time.