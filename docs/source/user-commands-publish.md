---
title: 'Tatin user commands for publishing'
description: 'Further user commands for publishing Tatin packages'
keywords: apl,dyalog,host,publish,registry,tatin,ui
---
# User commands for publishing

!!! abstract "User commands for publishing Tatin packages"

[BuildPackage](#build-package)       [DeprecatePackage](#deprecate-package)
[CopyRegistry](#copy-registry)       [GetDeletePolicy](#get-delete-policy)
[CreatePackage](#create-package)      [PublishPackage](#publish-package)
[Debug](#debug)              [UsageData](#usage-data)
[DeletePackages](#delete-packages)
{: .typewriter}

These further [user commands](user-commands.md) concern publishing packages.


---
## :fontawesome-solid-terminal: Build package

    ]TATIN.BuildPackage [source] [target]

Where `source` and `target` are paths to folders, write the contents of the former as a ZIP in the latter,
bumping the build number unless the `version` option sets it.

Folder `source` must contain a file `apl-package.json` defining the package.

You can omit `source` and/or `target` if the source is a Cider project.

The command asks you to confirm any assumptions.

---|---
bump=         | Either `patch` or `minor` or `major`: bump that part of the version number, together with any build number. Affects both the package and its config file.
dependencies= | Find dependencies in this subfolder of the project. (Rarely need to specify this: see [Publishing Packages](publish-packages.md).)
projectspace= | Where the package contents are to be installed.
version=      | Set the version number in both the package project and the package to be created.

Note that `bump=` and `version=` are mutually exclusive.

Until version 0.118.0 `projectspace` was `tatinVars`.

Until version 0.117.0 you could set the build number by including it in the version.
Now the build number gets bumped in all cases.

:fontawesome-solid-code: API:
[`BuildPackage`](api.md#build-package)



## :fontawesome-solid-terminal: Copy registry

    ]TATIN.CopyRegistry [source] [target]

Where

-   `source` is the URL or alias of a Tatin registry (defaults to `[tatin]`)
-   `target` is a path to a local folder (optional if `dry` flag set)

copy non-deprecated packages from `source` to `target` if not already present.

-------|-------------------------------
dependencies= | Flag: whether to copy dependencies, default 1. (Useful only for test cases.)
dry    | List packages that would be copied, but copy nothing.
force  | Overwrite existing packages.
group= | Copy packages only from specified group, but also their dependencies.
latest | Copy only the latest minor version of each major version.
list=  | <p>One of</p><ul markdown><li>a comma-separated list of package IDs</li><li markdown>a file with package names, one per row, specified with the `file://` protocol</li><li markdown>a fully qualified variable name</li></ul><p>Specify all packages as group-name or group-name-major.</p>
verbose= | <ol markdown><li>Print a detailed report for each package copied.</li><li>Print reports as the list is processed.</li></ol>

??? example "Examples"

    List packages that would be copied.

        ]CopyRegistry [tatin] -dry
        ]CopyRegistry -dry

    Copy all packages from `[tatin]` if not already available.

        ]CopyRegistry /path/2/Reg

    Copy the latest minor versions of the highest major versions of packages from `[company-reg]`.

        ]CopyRegistry [company-reg] /path/2/Reg -latest

    Copy from `[tatin]` all packages of the group `aplteam`.

        ]CopyRegistry /path/2/Reg group=aplteam -force

    Copy from `[tatin]`, if not already available, packages listed in variable `#.MyVars`.

        #.MyVars←'aplteam-FilesAndDirs,aplteam-APLTreeUtils2'
        ]CopyRegistry /path/2/Reg -list=#.MyVars

    Copy from `[tatin]` packages specified in variable `#.MyVars`

        ]CopyRegistry /path/2/Reg -list=aplteam-FilesAndDirs-4 -force

    Copy all packages specified in the file /myPkgs.txt if not already available

        ]CopyRegistry /path/2/Reg -list=file=/myPkgs.txt

:fontawesome-solid-code: API:
[`CopyRegistry`](api.md#copy-registry)




## :fontawesome-solid-terminal: Create package

    ]TATIN.CreatePackage target

Where `target` is a path to a folder, create a new Tatin package in it.

The command is a wrapper for [`]TATIN.PackageConfig -edit`](user-commands.md#package-config).


## :fontawesome-solid-terminal: Debug

    ]TATIN.Debug [toggle]

Where `toggle` is 0, 1, `on` or `off`, set Debug Mode on or off.

If `toggle` is omitted, report current state.

With Debug Mode on, Tatin leaves application errors untrapped so you can investigate them.
(Error guards in dfns, errors when communicating via TCP/IP and similar errors are still trapped.)


## :fontawesome-solid-terminal: Delete packages

    ]Tatin.DeletePackages pattern

Where `pattern` is

-   a registry URL or alias, followed by a package ID
-   a path to a folder containing a package (must contain a file `apl-package.json`) specified with the `file://` protocol

delete one or more packages from the registry or folder, including deprecated packages.

If the pattern matches multiple packages, ask which to delete.

Example arguments:

    https:/tatin.dev/grp-foo-1.0.0         ⍝ registry URL, package ID
    [test-tatin]grp-foo-1.0.0              ⍝ registry alias, package ID
    [test-tatin]foo-1.0.0                  ⍝ no group name
    [test-tatin]foo-1                      ⍝ versions of foo with major=1
    [test-tatin]foo                        ⍝ versions of foo
    file:///path/2/Registry/grp-foo-1.0.0  ⍝ local package

:fontawesome-solid-code: API:
[`DeletePackages`](api.md#delete-packages)


## :fontawesome-solid-terminal: Deprecate package

    ]TATIN.DeprecatePackage pattern[majorversion] comment

Where

-   `pattern` is a registry URL or alias followed by group and package names

-   `majorversion` (optional) is a major version number (omitted, defaults to all major versions)

-   `comment` is text explaining why the package is deprecated (remmeber to delimit with quotes)

asks to confirm the action, then create
(for each major version targeted)
a new minor version with the `deprecated` property set.

------|-------------------------
force | Don’t ask for confirmation. (Useful mainly for tests.)

Example: Deprecate on `[tatin]` all major versions of `grp-foo`:
```
]TATIN.DeprecatePackages [tatin]grp-foo "Use MarkAPL instead"
```

:fontawesome-solid-code: API:
[`DeprecatePackage`](api.md#deprecate-package)


## :fontawesome-solid-terminal: Get delete policy

    ]TATIN.GetDeletePolicy [reg]

Where (optional) `reg` is

-   a registry URL or alias
-   `*` (all known registries)
-   `?` (list them and ask me to choose)

or if omitted, `[tatin]`,
report the delete policy (`None`, `Any`, or `JustBetas`) of the server/s concerned
and cache the result.

Query a registry for its delete policy just once
and then cache the result.

------|----------------------------------------
check | Ignore the cache: query the server and cache the response.

:fontawesome-solid-code: API:
[`GetDeletePolicy`](api.md#get-delete-policy)


## :fontawesome-solid-terminal: Publish package

    ]TATIN.PublishPackage [source] [reg]

Where

-   `source` is a path to a package folder, or a ZIP file (typically created by [BuildPackage](#build-package))
-   `reg` is a registry alias or URL, or `?` (ask me which known registry)

publish the package to the registry if specified, otherwise to `[tatin]`.

If `source` is not specified, look for open Cider projects.
If you find one, use it; if multiple, ask me which.

If the registry’s delete policy is `none`, ask me to confirm publication.

The name of the resulting package is extracted from the ZIP file which therefore must conform
to the Tatin rules.

--------------|-----------------------------------------------------------------
dependencies= | Find dependencies in this project subfolder. (Rarely need to specify: see [Publish Packages](publish-packages.md).)

:fontawesome-solid-hand-point-right:
[Publishing a user-command package](publish-packages.md#user-command-packages)<br>
:fontawesome-solid-code: API:
[`PublishPackage`](api.md#publish-package)




## :fontawesome-solid-terminal: Usage data

    ]TATIN.UsageData [reg] [ -download [-all] [-folder=] [-unzip] ]

Where `reg` is a registry URL or alias
(if omitted, ask me which)
list the usage files available.

---------|-----------------------------------------------
all      | Select all available.
download | Ask me which usage files to download to a subfolder of my temp folder.
folder=  | Download to this empty folder.
unzip    | Unzip downloaded file/s and delete ZIPs.



