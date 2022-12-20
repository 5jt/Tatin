[parm]:title             = 'Tatin: Intro'
[parm]:leanpubExtensions = 1
[parm]:collapsibleTOC    = 1
[parm]:toc               = 2 3
[parm]:numberHeaders     = 2 3 4 5 6


# Server: Tips and tricks

Once you've installed a server their are a couple of things that need to be taken care of.

## Maintenance

### Tags

The most important thing is to watch tags. Tags can be very useful in order to find a package, but the problem ist that package authors tend to use different tags for the same thing, use legal but different spelling (UK versus US) or invalid spelling, tags that make no sense like adding the group name as a tag, or adding "dyalog" etc.

That means that for tags to work there has to be a gate keeper who is responsible for correcting / removing tags.

That gatekeeper needs to be able to execute code within the server, but only once. The idea is to correct problems in the package config files somehow.

A> ### The package config file and the ZIP file 
A>
A> Note that changing the config file is enough: If the file got changes then Tatin will make sure that the new version is automatically also added to the ZIP file of the package, effectively overwriting the old version of the package file. 

This can be achieved by uploading an `.aplf` text file (read: a function) into a folder that is defined in the INI file as `[CONFIG]MaintenanceFolder`.

If one or more of such files are found by the Tatin Server they are loaded into the server and executed. Once executed the files are renamed by adding an extension `.executed`.

For example, if there is a file `RemoveDyalogFromTags.aplf` then it is loaded into an unnamed namespace and called with a right argument `G` (for "globals"). Once executed the file is then renamed to `RemoveDyalogFromTags.aplf.executed`.

That makes sure the code is not executed again, but it is also  useful for documenting what code was executed, and when.


## Developing

If you want to make changes or add new features you need a good understanding of the basic design of Tatin.


### Structure

#### Main namespaces

There are four ordinary namespace that contain all the Tatin code:

`Admin`

: Contains helpers useful to administrate Tatin, for example creating a new version, performing maintenance tasks etc.

`Client`

Contains the code executed on the Client side of Tatin.

`Server`

Contains the code to run a Tatin Server
 
`Registry`

Contains all code that is shared between a Tatin client and a Tatin server.

#### Other stuff

There are some mory ordinary namespace that are used by Tatin

`Plodder`

: A fully fledged HTTP server that is bases on Rumba and Conga

: For details see <https://github.com/aplteam/Plodder>

`RumbaLean`

: An implementation of HTTP 1.1 in APL

: For details see <https://github.com/aplteam/RumbaLean>

These two are not part of the Tatin project.


### Version

Note that by design new versions always comprehend both the server and the client, so the version number is always the same for both.


### Developing with a running server

You might want to run a server while Tatin is an opened Cider project. The running server allows you to investigate what the coder is doing, and at the same time any changes abd additions would be added to the project by Link.

Let's assume that you want to run the Tatin server that is part of the Tatin project. When the Tatin test cases are executed then Tatin would ask you whether you want to start the server automaticallt -- that is the server we talking about, **no** https://test.tatin.dev

In order to achive this execute the following steps:

1. Open the Tatin project with `]CiderProject`

2. Execute `#.Tatin.Admin.Initialize_Server`

3. Change the current directory with these statements:

   1. `#.Tatin.CiderConfig.HOME,'/TestServer/Server'`
   2. Execute `]cd` with the output of the first statement as input

4. Execute `#.Tatin.Server.Run 1`

The main handlers are:

  * `#.Tatin.Server.OnRequest`
  * `#.Tatin.Server.OnHouseKeeping`

`OnRequest` will eventually call one of these:

  * `_GET`
  * `Handle_GET_REST_Version1`
  * `Handle_PUT_And_POST`
  * `Handle_PUT_And_POST_REST_Version1`

These will call the Tatin functions that perform the real actions.

#### Error trapping

Keep in mind that error trapping is active, so when you change a function and inject a typo it will trigger it once your code get executed.

If there is any danger for you to lock horns with error trapping consider to put this into `OnRequest`:

`⎕TRAP←0 'S'  ⍝TODO⍝`   

Also, make `⎕TRAP` a local variable in `OnRequest`.

`⍝TODO⍝` is a remainder that won't go unnoticed: there is a test case that will detect these markers and report them, so it's pretty hard to forget them.



### Developing with two sessions, server and client

When the test cases are executed the user is asked whether she want the Tatin server required by the Tatin test cases to be started automatically.

* If the user answert this with a "yes" and instance 

You must know exactly what you are doing, otherwise you are likely to loose code.

| **To be enhanced** |

## Misc

### Special REST commands 

A Tatin server can offer a number of special REST commands only useful for a developer when testing Tatin. They should never be available on a production server.

Whether these commands would be carried out is decided by the INI entry `[CONFIG]SpecialCommands`.

One of the commands will return an HTML page with all available commands. We use this as example.

In a Browser enter this URL:

```
https://localhost:5001/v1/list-commands
```

Note that with the exception of `list-commands` these commands do not return HTML, they trigger actions.


### Executing the Tatin test suite

| **To be enhanced** |

### Creating a new version

| **To be enhanced** |