---
title: 'Tatin: Server Tips'
description: ''
keywords: 
---
# Server: Tips and tricks

Once you've installed a server there are a couple of things that need to be taken care of.

## Maintenance

### Tags

The most important thing is to watch tags. Tags can be very useful to find a package, but the problem is that package authors tend to use different tags for the same thing, use legal but different spelling (UK versus US) or invalid spelling, tags that make no sense like adding the group name or "dyalog" etc.

That means that for tags to work there has to be a gatekeeper who is responsible for correcting/removing/adding tags.

That gatekeeper needs to be able to execute code on the server, but only once. The idea is to correct problems in the package config files somehow.

### The package config file and the ZIP file

Note that changing the config file is enough: If the file got changed then Tatin will make sure that the new version is automatically also added to the ZIP file of the package, effectively overwriting the old version of the package file. 

This can be achieved by uploading an `.aplf` text file (read: a function) into a folder that is defined in the INI file as `[CONFIG]MaintenancePath`.

If one or more of such files are found by the Tatin Server while doing housekeeping, they are loaded into the server and executed. Once executed the files are renamed by adding an extension `.executed`.

For example, if there is a file `RemoveDyalogFromTags.aplf` then it is loaded into an unnamed namespace and called with a right argument `G` (for "globals"). Once executed the file is then renamed to `RemoveDyalogFromTags.aplf.executed`.

That makes sure the code is not executed again, but it is also useful for documenting what code was executed, and when.

#### Crashing maintenance files

Like any other program, a maintenance file may crash. If that happens the server carries out the following steps:

1. Report it to the log file
2. Send an email reporting the crash with details to the maintainer (if enabled in the INI file)
3. Rename the file from `*.aplf` to `*.crashed` --- we don't want that function to be executed again


## :fontawesome-solid-calendar-plus: Update the server

Download the release ZIP from the [Releases](https://github.com/aplteam/Tatin/releases) page and unzip it.

**Read the release note** before doing anything else.

!!! warning "An update could require taking the server down for maintenance."


By defult, a running Tatin server watches the workspace on disk
and reloads it if it changes.
This makes for an easy update if no other action is required.

The automatic update can be switched off in the INI file with `[CONFIG]ReloadWS`.

While reloading the workspace, the server returns error messages.
Expect this to last 10 seconds or more, depending on the number of packages managed.


### The INI file

The update might add or remove settings in the INI file: consult the release note.

If there are changes, follow instructions in the note.

!!! danger "Do not replace the INI file."

The server monitors the INI file for changes, and re-initialises if it finds them.
Whether that works depends on the change: some settings are used at an early stage and cannot be changed later.
Again, the release note will tell.


### Assets

The release note describes what action to take, if any.
Often the subfolder `docs/` is to be replaced. (Contains the documentation.)


### Maintenance folder

!!! danger "Never replace the maintenance folder."

    The folder `maintenance/` documents changes made to the packages:
    you don’t want to lose this.

If the new folder is not empty, copy its content over.
Maintenance files can be used to carry out changes on all or some of the packages managed by the server, like adding a new property to the package config files of all packages.





