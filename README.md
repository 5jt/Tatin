# Tatin - a Package Manager for Dyalog APL

## Overview

Tatin consists of three modules:

* `Tatin` - the client
* `Tatin_Server` - the Server
* `Tatin_Registry` - all code that is shared between the client and the server.

Client as well as Server run under Windows, Linux and Mac-OS.

Tatin requires Dyalog APL 18.0 Unicode or better.

The Tatin wiki contains exhaustive documentation for how to use the Tatin Client and how to get a Tatin Server up and running.

## Usage

Once you've installed the Tatin client you have access to <https://tatin.dev>, the principle Tatin server.

However, you might find it useful to run your own Tatin server. For example, all packages used within a company could be served by a Tatin Server within your company LAN.

You may also keep a local Registry on your own machine, for example for development work. From a consumer's perspective there is no difference between consuming a package that is coming from a Tatin Server or from a local Registry.



## Contributing to the code 

There is a separate document "ForDevelopers.html" available in the `docs/` folder.

-----

Latest revision 2020-07-18