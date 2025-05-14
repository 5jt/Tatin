---
title: 'Create new Tatin version'
description: 'How to create a new version of Tatin.'
keywords: 'apl, dyalog, github, profread, tatin, version'
---
# Create a new version of Tatin

!!! abstract "What an administrator of the Tatin project on GitHub does once she has accepted at least one PR or finished her work on a branch."

A contributor’s job is done once they have submitted a Pull Request (PR) on GitHub.


## Check version, history and documentation

!!! info "Versions comprehend both server and client, so the version number is always the same for both."

1. Check `#.Tatin.Registry.Version` is correct
1. Check `#.Tatin.Registry.History` is correct
1. Check the document `docs/source/release-notes.md` is correct
1. Proofread the documentation

A function creates a single HTML file from all the Markdown documentation, making proofreading much easier:

          htmlFilename←#.Tatin.Admin.CreateProofReadDocument 1

Open that file with the word processor of your choice and use its spell-checking capabilities.


## The `Make` function

<!-- FIXME Consider documenting this rather than referring to Cider’s meta-function. -->
Ask Cider how to create a new version of Tatin:

          ]Cider.Make
    #.Tatin.#.Tatin.Admin.Make 0 ⍝ Execute this for creating a new version


The `Make` function launches two instances of Dyalog, one for creating the client version, one for creating the server version.

1.  runs `#.Tatin.Admin.MakeClient`
1.  compiles the documentation from Markdown into HTML files and distributes them
1.  runs `#.Tatin.Admin.MakeServer`

**The arguments** to `Make` are both flags:

---|---
right | Suppress user prompts: 1 is suitable for, say, a batch script.<br><br>If 0, `Make` will ask whether you want to try updating the packages Tatin itself depends on, and whether to copy the new version of the Tatin Client to the `MyUCMDs/` folder.
left | (optional) Block the `load` and `lx` command-line options in instances of Dyalog started to create the Client and the Server, thus preventing the interpreter from immediately running the code, and allowing you to run the code in the Tracer. (Defaults to zero.)<br><br>Command-line options `lx` and `load` were introduced in 18.0.

!!! info "The build ID is always bumped when you create a new version."


## Distribution files

The `Dist/` folder gets replaced.
It contains the ZIPs to be released on GitHub;
`.gitignore` prevents the folder appearing on GitHub.

The ZIPs:

    Tatin-Client-{major}.{minor}.{patch}.zip
    Tatin-Documentation-{major}.{minor}.{patch}.zip
    Tatin-Server-{major}.{minor}.{patch}.zip

<!-- FIXME What to do with the ZIPs? -->
