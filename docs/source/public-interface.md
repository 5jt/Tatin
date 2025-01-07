---
title: 'Tatin public interfaces'
description: 'How to control which objects of a Tatin package are exposed as its public interface'
keywords: api, apl, dyalog, interface, package, tatin
---
# Public interface to a package

!!! abstract "How to control which objects of a Tatin package are exposed as its public interface"

These cover functions are one-line dfns that call the function with the same name one level above. Because they are one-liners the Tracer will ignore them: it is not possible to trace into a one-line dfn. Most of the time this is a blessing, but sometimes it is a curse.

By default the contents of the package becomes the public interface: `⎕NL 2 3 4 9`. However, if there is a function `Public` available, then this is expected to return a list of simple text vectors defining the public interface. `CreateAPIfromCFG` will use the result of `Public` if it finds one in the root of the given namespace.

Alternatively, you can pass a list of names as left argument to `CreateAPIfromCFG` - that would take precedence.

It is possible to keep `names` very simple by specifying a single name, but it can also be complex and comprehend the names of functions, operators, variables, interfaces, classes and namespace, simple and scripted. It can also use dotted syntax to define names within namespaces, though only one level deep.

!> ### Tips and hints
=> You may call `CreateAPIfromCFG` when initialising a package after loading it (from a function that is executed by the package's `lx` parameter, see "Tatin's Package Configuration File" for details).
=> 
=> Alternatively, you may call it as part of the package creation process, making the API a permanent part of the package. 


#### Typical scenarios

1. The package consists of a single class or a single ordinary namespace

1. The package consists of a single function or operator

1. The package consists of several objects: a mixture of functions, operators, classes and/or namespaces

A> ### Single functions 
A>
A> You _must not_ specify the name of a function (or an operator) as the API in any of these cases.
A> 
A> This restriction helps to avoid confusion, but there is also a technical issue: Tatin needs to establish references to the API, and although in Dyalog one can establish references (kind of) to monadic, ambivalent, and dyadic functions, this is not possible for neither operators nor niladic functions.


##### A single namespace

* If you don't specify `api` then the name of the namespace is the API. 

  For example, if the package name is `pkgName` and the namespace's name (see the `source`  parameter) is `foo` and it has a function `Hello`, then you call `Hello` with:

  `pkgName.foo.Hello`

* If you do specify `api` by assigning an APL name to it, then it must be the name of the namespace. In that case, the _contents_ of the namespace becomes the API.

  For example, if the package name is `pkgName` and the namespace's name is `foo` and it has a function `Hello`, then you specify `api` as `foo` and call `Hello` with:

  `pkgName.Hello`


##### A single scripted namespace

If a package consists of a single scripted namespace then you cannot use the services of Tatin's `CreateAPIfromCFG` function. The reason is that this function tries to establish a sub-namespace (with a name defined by the package config parameter `api`) inside the target, and that works only with ordinary namespaces, but not with scripted namespaces.

You could still create an API yourself, however. For example, if a package `Foo` consists of a scripted namspace that carries (say) about 20 variables, functions and operators of which only two should be accessible by a user (say `Encode` and `Decode`), then you could create this namespaces structure:

```
#.Foo
#.Foo.Core
  ... ⍝ vars, fns and oprs
#.Foo.API
#.Foo.API.Encode
#.Foo.API.Decode  
```

This is how `#.Foo.API.Decode` would look like:

```
Decode←{⍺ ##.JWTAPL.Decode ⍵}
```

And this is how `#.Foo.API.Encode` would look like:

```
Encode←{⍺ ##.JWTAPL.Encode ⍵}
```

You just need to establish these functions yourself.

You don't need a function `Public` here because that function's only purpose is to be called by `CreateAPIfromCFG` in order to establish a list of public objects. Here the job is done by you, therefore there is no need for `Public`.


##### A single class

* If you don't specify `api` then the name of the class is the API. 

  For example, if the package name is `pkgName` and the class name is `foo` and it has a function `Hello`, then you call `Hello` with:

  `pkgName.foo.Hello`

* If you do specify `api` then it must be the name of the class. In that case, everything in the class with `:Access Public Shared` becomes the API.

  For example, if the package name is `pkgName` and the class name is `foo` and it has a publicly shared function `Hello`, then you call `Hello` with:

  `pkgName.Hello`


##### A single function or operator

If the name of the package is `pkgName`, and the name of the function is `MyFns`, then it is called as `pkgName.MyFns`. The function may be niladic, monadic, ambivalent or dyadic.

The same holds for an operator.

In this particular case `api` _must not_ be defined (remain empty).


##### A mixture of several APL objects

* If `api` is not set then all top-level objects of the package become the API: functions, operators, namespaces, classes, interfaces.

* If `api` is set then it must point to one of the namespaces or classes, or a sub-namespace (using dotted syntax), or a class in a sub-namespace. Then just the objects in what `api` is pointing to become the API.


##### Restricting what's "public"

The user might want to expose only a subset of functions/operators of a namespace (classes have such an interface anyway: `:Public Shared`), and in that case, the user must not only specify `api`, but also structure her code accordingly.

If the name of the package is `pkgName`, and it is loaded into `#`, and you want to expose only the functions `Run` and `CreateParmSpace`, then the recommended way of doing this is to create a sub-namespace with the name (say) `MyAPI` and populate it with two functions:

* `Run`:

  ```
  Run←{⍺←⊢ ⋄ ⍺ ##.Run ⍵}
  ```

  (Assumes that `Run` takes an optional left argument)

* `CreateParmSpace`:

  ```
  CreateParmSpace←{##.CreateParmSpace ⍵}
  ```

  (Assumes that `CreateParmSpace` does not accept a left argument)

Finally, you need to specify `api: "MyAPI"` in the package config file.

Calling the function `Run` (after loading the package) would then require:

```
      #.PkgName.Run
```

To the outside world, only two functions are visible:

```
      #.PkgName.⎕nl ⍳16
#.Foo.Run
#.Foo.CreateParmSpace
```

Similarly, if `PkgName` consists of the two namespaces `Boo` and `Goo`, and `Run` and `CreateParmSpace` live in `Boo`, then you could also have a sub-namespace `Boo.API` that hosts `Run` and `CreateParmSpace`, and `api` would be `Boo.API`, while calls are still `PkgName.Run` and `PkgName.CreateParmSpace`.

