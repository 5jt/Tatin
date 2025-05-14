---
title: 'Tatin for Contributors'
description: 'How to contribute to Tatin, and how to set up to do that.'
keywords: 'apl, contribute, dyalog, github, tatin'
---
# Contribute to Tatin


!!! abstract "Tatin is published under the MIT licence, and you are welcome to contribute to it."

[Tatin](https://github.com/aplteam/Tatin) is not owned by anybody; it is a community project.



## Requirements

To work on Tatin you need

!!! tip inline end ""

    :fontawesome-brands-github:
    [How to contribute to a GitHub project](https://docs.github.com/en/get-started/exploring-projects-on-github/contributing-to-a-project)

-   Git and a GitHub account
-   Dyalog 18.2 Unicode or better (Classic is not supported)
-   .NET installed and [Link](https://github.com/dyalog/link) activated
-   [Cider](https://github.com/aplteam/cider)

**Operating system**
Linux, macOS, or Windows.
AIX is not supported. 
The Raspberry Pi is not officially supported but might work anyway.

You can _develop_ on any operating system, but _building a new version_ is currently supported only on Windows.
<!-- This restriction is likely to be lifted in a later version. -->


## Managed by Cider

Tatin is managed by the [Cider project management tool](https://github.com/aplteam/cider).
If you are new to Cider, spend some time playing with it before using it for serious work. (Thirty minutes should suffice.)

??? tip "Working without Cider"

    While it is possible to make changes or add code to Tatin without Cider, using Cider makes it significantly easier. And the build process requires Cider.

    That said, you are not required to build a new version before submitting a pull request, so you might get away without Cider, but using Cider is certainly recommended.


## Get started

1.  On GitHub make your own fork of [Tatin](https://github.com/aplteam/cider) and clone it to your local machine, say at `C:\Tatin`.

2.  Launch Dyalog and open the project

        ]Cider.OpenProject C:\Tatin

Now you have a working version of Tatin on your machine and you can start contributing.

!!! info "No need to save a workspace"

    Every function, operator, class, interface or namespace script changed in `#.Tatin` is automatically saved to disk by Link.


## Code

To modify the source code:

1.  [Open an issue](https://github.com/aplteam/Tatin/issues) on the GitHub repository and declare what you intend to do.
    GitHub will assign an ID number to your issue.

1.  On your local machine, create a branch of `main` and name it after your issue. 
    For example, if your issue is _Foo is failing in bar_ and has ID 123, name your branch `123-fix-foo-bar`.

1.  Before finishing, confirm your branch `123-fix-foo-bar` passes the Tatin tests.

1.  On GitHub, synch your fork with the source repo.
    On your local machine, pull the `main` branch from your GitHub fork; then merge `main` into `123-fix-foo-bar`. 
    Confirm  your branch `123-fix-foo-bar` passes the Tatin tests.

If your changes pass the tests, push `123-fix-foo-bar` to your fork, and submit a pull request.


## Documentation

If you find parts of this guide confusing, outdated, unclear or missing bits and pieces, change it.
That might well be your first valuable contribution.

A minor edit to the documentation does not require its own issue and branch; it can be made in the `main` branch.


## Submit your changes

When you have improved the documentation, fixed a bug, or added a feature, create a pull request (PR).
The project team will check your contribution.