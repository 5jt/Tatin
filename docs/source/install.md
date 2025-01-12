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

Test for inline help: in a new Dyalog session enter

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


## :fontawesome-solid-plus: Update an old version of Tatin

### Versions before 0.105.0

Tatin Version 0.105.0 introduced the [`Update` user command](user-commands.md#update).

To update an earlier version:

1.  Remove `Tatin/` from the `MyUCMDs/` folder. (See [Expose the API](#expose-the-api).)
2.  Download and install Tatin from scratch. (Also Cider, if you are using it.)
3.  Review your `setup.dyalog` script if you are using one.

### :fontawesome-brands-windows: OneDrive and versions before 0.114.0

OneDrive is cloud storage provided by Microsoft for backing up your personal folders.
A Windows user `JohnDoe` who enables OneDrive changes his Documents folder:

    C:\Users\JohnDoe\Documents\             ⍝ from
    C:\Users\JohnDoe\OneDrive\Documents\    ⍝ to

Dyalog keeps in his Documents folder such folders as

    Dyalog APL Files                  ⍝ version-agnostic
    Dyalog APL-64 19.0 Unicode Files  ⍝ version-specific

Before version 0.114.0, Tatin read environment variable `USERPROFILE` to find the Documents folder.
But `USERPROFILE` does not reflect use of OneDrive.
An update of Tatin would seem to succeed but, not written to the OneDrive Documents folder,
would not appear in subsequent launches of Dyalog.

Since version 0.114.0, Tatin uses the experimental I-beam `4070⌶0` to recognise a Documents folder in OneDrive.

If you are using OneDrive and a version of Tatin older than 0.114.0

1.  Determine your tatin-home: the relative path from your OneDrive Dcuments folder in which your Tatin files are kept, e.g.

        Dyalog APL-64 19.0 Unicode Files/SessionExtensions/CiderTatin

1.  Add to SALT’s `cmddir` the path

        C:/Users/<⎕AN>/OneDrive/Documents/<tatin-home>

1.  Remove from SALT’s `cmddir`  the path

        C:/Users/<⎕AN>/Documents/<tatin-home>

1.  Remove from your file system folder

        C:/Users/JohnDoe/Documents/<tatin-home>

Future updates will work as expected.

??? danger "Reverting to a version older than 0.114.0"

    If you revert to a Tatin version older than 0.114.0, for example with

        ]activate -reset

    you also recreate the problem.


## :fontawesome-solid-trash-can: Deactivate

Remove Tatin and/or Cider from the installation folder:

    ]Deactivate [all|cider|tatin] [-versionagnostic]


## :dyalog-cider-logo: Cider

Cider depends upon Tatin. If you use Cider:

-   Deactivate and remove it before Tatin
-   Re/Install Tatin before Cider.


