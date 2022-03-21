---
title: Package maintenance
description: 
published: true
date: 2022-03-21T14:30:45.494Z
tags: 
editor: markdown
dateCreated: 2022-03-21T14:30:45.494Z
---

# Package maintenance
Please also refer to the building guide.

Ultramarine's Packaging guidelines are a superset of the [Fedora Packaging Guidelines](https://docs.fedoraproject.org/en-US/packaging-guidelines/) with one small difference:

Since we do not use dist-git, you should add this line to the top of every Ultramarine spec file to make Mock automatically download source code

```
%define _disable_source_fetch 0
```

You also need to install `umpkg` to maintain any package.
> For maintainers: Please update the `mock-core-configs` package for every new release done. Please.
{.is-info}

## Adding a new package
To add a new package, create a new repository in the `dist-pkgs` group in the Ultramarine GitLab, then add a branch for each build target you want to build.

Then clone your repository.

Now to initialize your repository, run `umpkg init` to initialize the repository.

You will now see a `umpkg.cfg` file. Set the `git_repo` key to the HTTPS Git URL of the repository.

Make a commit, the push to each branch for your build targets

## Building packages

To build a package locally, run `umpkg build <spec file>`. This will run Mock to build the package locally for testing.

To push your package to the Koji build system, run `umpkg push <target>` after making sure you have pushed all your changes to Git.