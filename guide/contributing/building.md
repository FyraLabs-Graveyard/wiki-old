---
title: Building Packages
description: 
published: true
date: 2022-01-10T11:09:45.543Z
tags: 
editor: markdown
dateCreated: 2021-12-17T16:34:23.741Z
---

# Building packages for Ultramarine

To build packages for Ultramarine, you should use the `umpkg` tool from the repos, or manually install the tool as a Python package.

For more inquiries, please ask on the `#dev` channel on the Ultramarine Discord server.

## Authenticating to the Build System
Before you can build packages, you need to properly set up your Koji client to use the Ultramarine servers.
But first, you have to set up Kerberos authentication.


### Kerberos Authentication

Install Koji and Kerberos by running
```
sudo dnf install koji krb5-workstation
```

Then log in to Kerberos using your credentials.
```
kinit <YOU>@ULTRAMARINE-LINUX.ORG
```

If you do not already have your Kerberos credentials, you need to sign up first.

### Setting up Koji

Koji is Fedora's main build system, and also the build system for Ultramarine.

To properly push and manage packages for Ultramarine, you need to make sure you have [your kerberos credentials](#kerberos-authentication) set up correctly first.

To configure Koji to use the Ultramarine servers, you can either:
- Directly edit the systemwide configuration file `/etc/koji.conf`
- Copy `/etc/koji.conf` to your home directory as `~/.koji/config`

Then edit the file to have the following lines:
```
[koji]
;url of XMLRPC server
server = https://lapis.ultramarine-linux.org/kojihub/

;url of web interface
weburl = https://lapis.ultramarine-linux.org/koji

;path to the koji top directory
topurl = https://lapis.ultramarine-linux.org/kojifiles

;configuration for Kerberos authentication
authtype = kerberos
krb_rdns = false

;the principal to auth as for automated clients
;principal = user@ULTRAMARINE-LINUX.ORG

;the keytab to auth as for automated clients
;keytab = /etc/krb5.keytab


;configuration for SSL authentication

;client certificate
;cert = ~/.koji/client.crt

;certificate of the CA that issued the HTTP server certificate
;serverca = ~/.koji/serverca.crt

;plugin paths, separated by ':' as the same as the shell's PATH
;koji_cli_plugins module and ~/.koji/plugins are always loaded in advance,
;and then be overridden by this option
plugin_paths = ~/.koji/plugins

;[not_implemented_yet]
;enabled plugins for CLI, runroot and save_failed_tree are available
;plugins =
; runroot plugin is enabled by default in fedora
plugins = runroot

;timeout of XMLRPC requests by seconds, default: 60 * 60 * 12 = 43200
;timeout = 43200

;timeout of GSSAPI/SSL authentication by seconds, default: 60
;auth_timeout = 60

; use the fast upload feature of koji by default
use_fast_upload = yes

```

Or add the above config as a seperate profile for Koji and replace `[koji]` with `[ultramarine]`, then put the file in `/etc/koji.conf.d/` or `~/.koji/conf.d/`.

This will be automatically resolved later once we properly package `umpkg`.

Note: `umpkg` will use the main Koji profile for now, this will be fixed in the future, if you want to use the Ultramarine profile separately, you can use the Koji CLI directly with the `--profile` option.

## Using `umpkg` to build packages
Ultramarine, like Fedora, uses the RPM package manager for package management. To start building packages, you should start by referring to the [RPM Packaging Guide](https://rpm-packaging-guide.github.io/).

To build an existing package, clone the repo from Gitlab by running `umpkg get <package path in dist-pkgs group`, then build the package by going inside the directory and running `umpkg build <package>`

To push the package onto Ultramarine's build system, run `umpkg push <Koji Tag> <package>`

`umpkg` is a collection of tools for building and maintaining packages for Ultramarine, similar to `fedpkg`.

## SOP for Package maintainers

To add a new package to Ultramarine, you should create a new repo in the dist-pkgs group

If the package is part of a group that depends on each other, you can also make a subgroup inside the dist-pkgs group, for example: The Budgie Desktop group is a subgroup at `dist-pkgs/budgie-desktop`.

If you are adding a patched version of a package that already exists in Fedora, create a new repo in the group by importing the existing repo from [src.fedoraproject.org](https://src.fedoraproject.org), then add your own patches. Preferrably, you should also remove the Fedora-specific branches from the repo, and add the Ultramarine branches instead. Then configure your `umpkg.conf` file.