---
title: Koji Management
description: 
published: true
date: 2022-03-21T14:15:02.109Z
tags: 
editor: markdown
dateCreated: 2022-03-21T14:15:02.109Z
---

# Koji instance management
This is a guide on how to manage the Ultramarine Koji instance

Please also refer to the [Koji Documentation](https://docs.pagure.org/koji/) for more general instructions

Ultramarine uses the Koji Build system for building packages and operating system images.

Also follow the package building page to set up your Koji instances


To add a new package to a tag, (for example, um36), run the following:
```
koji add-pkg --owner <username of owner> <package> um36
```

You should also add the package to the build tag too.

```
koji add-pkg --owner <username of owner> <package> um36-build
```

# SOP for branching a new release
Create a new dist tag
```
koji add-tag um37
```

Create a new build tag (example will be um37-build)

```
koji add-tag --parent um37 um37-build
```

Then copy over things from the older tag

```
koji clone-tag --all um36-build um37-build
```

Then add the external repos for RPMFusion and Fedora

```
koji add-external-repo -t um37-build f37 https://dl.fedoraproject.org/pub/fedora/linux/releases/37/Everything/\$arch/os/
koji add-external-repo -t um37-build f37-updates https://dl.fedoraproject.org/pub/fedora/linux/updates/37/Everything/\$arch/
koji add-external-repo -t um37-build rf36-free http://download1.rpmfusion.org/free/fedora/releases/37/Everything/\$arch/os/
koji add-external-repo -t um37-build rf36-nonfree http://download1.rpmfusion.org/nonfree/fedora/releases/37/Everything/\$arch/os/
```

Finally, add a new build target for the tag
```
koji add-target um37 um37-build
```

Now let's import the base comps file (Available [here](https://gitlab.ultramarine-linux.org/-/snippets/2)) and push it to the build tag

```
wget https://gitlab.ultramarine-linux.org/-/snippets/2/raw/main/comps.xml
koji import-comps comps.xml um37-build
```

Then we can try to generate a repository for the build tag
```
koji regen-repo um37-build
```

