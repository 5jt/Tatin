---
title: 'Tatin: Installing & Updating'
description: ''
keywords: 
---
# Install and update the client


!!! abstract "How to install Tatin in Dyalog 18.2"

    Tatin is already installed in Dyalog 19.0+.
    This article applies to Dyalog 18.2 only.


## :fontawesome-solid-list-check: Requirements

* Dyalog 18.2 **Unicode**
* [Link](https://dyalog.github.io/link/4.0/) version 3.0.8 or better

`]Tatin.Init` checks the requirements are met.


## :fontawesome-solid-download: Download

Download the latest release of the Tatin client.

:fontawesome-brands-github:
<https://github.com/aplteam/Tatin/releases>

Unzip the file and move folder `Tatin/` to the installation folder.


## :fontawesome-solid-folder-open: Installation folder

The installation folder is at `<path>/SessionExtensions/CiderTatin`
where `path` is:


=== "Version-specific"

    These file paths are specific to Version 18.2:
    ```
    /home/<⎕AN>/dyalog.182U<bit>.files                          ⍝ Linux
    /Users/<⎕AN>/dyalog.182U64.files                            ⍝ macOS
    C:\Users\<⎕AN>\Documents\Dyalog APL[-64] 18.2 Unicode Files ⍝ Windows
    ```

=== "Version-agnostic"

    These file paths make Tatin available to all Dyalog versions:
    ```
    /home/<⎕AN>/dyalog.files                  ⍝ Linux
    /Users/<⎕AN>/dyalog.files                 ⍝ macOS
    C:\Users\<⎕AN>\Documents\Dyalog APL Files ⍝ Windows
    ```


## :fontawesome-solid-terminal: Connect user commands

Include the installation folder in SALT’s search path.

    ]SALT.Settings cmddir ",<installation-folder>" -p

The comma tells SALT to _extend_ its path, not replace it.
The `-p` flag makes the change permanent.

Test for inline help in a new Dyalog session enter

    ]tatin -?


## :fontawesome-solid-code: Expose the API

Executing any Tatin command exposes the [API](api.md).

!!! detail "Dyalog 19.0+ exposes the Tatin API before any user command is executed."

An automated build process might need to expose the API 
without first executing a command.
You can do this with a `setup.dyalog` script in your `MyUCMDs/` folder.

Default locations for the  `MyUCMDs/` folder:

    /home/{username}/               ⍝ Linux
    /Users/{username}/              ⍝ macOS
    C:\Users\{username}\Documents\  ⍝ Windows

The Windows installer creates this folder;
on other platforms, create it yourself.


!!! detail "Setup scripts"

    At launch, Dyalog looks <!-- FIXME where? --> for a monadic `Setup` function in a `setup.dyalog` script.

    In version 19.0 the `Run.aplf` function offers a better way to achieve that: `setup.dyalog` superfluous.

:fontawesome-solid-download:
[`setup.dyalog`](assets/setup.dyalog) model setup script

If you already have a setup script:

-   Copy from the model script functions `IfAtLeastVersion`, `GetProgramFilesFolder` and `AutoLoadTatin`
-   Ensure your `Setup` function calls `AutoLoadTatin` and, if it returns an empty vector (success), reset the command cache:

        {}⎕SE.SALTUtils.ResetUCMDcache -1


## :fontawesome-solid-plus: Update Tatin

Tatin Version 0.105.0 introduced the [`Update` user command](user-commands.md#update).

To update an earlier version of Tatin:

1.  Remove `Tatin/` from the `MyUCMDs/` folder. (See [Expose the API](#expose-the-api).)
2.  Download and install Tatin from scratch. (Also Cider, if you are using it.)
3.  Review your `setup.dyalog` script if you are using one.


## :fontawesome-solid-trash-can: Deactivate

Remove Tatin and/or Cider from the installation folder:

    ]Deactivate [all|cider|tatin] [-versionagnostic]


## :dyalog-cider-logo: Cider

Cider depends upon Tatin. If you use Cider:

-   Deactivate and remove Cider before Tatin
-   Re/Install Tatin before Cider.


