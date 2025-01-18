---
title: 'Tatin glossary'
description: 'An explanation of terms used in the Tatin documentation'
keywords: apl, documentation, glossary, tatin
---
# Glossary

<style>
    dt {
        font-weight: bold;
    }
</style>

!!! abstract "Some terms used in the Tatin documentation"

beta
: A package with a text description in its patch.

    (See **version** and examples of **full package ID**.)


build number
: Tatin assigns each **version** a build number, in chronological sequence.

    This is not part of the version.
    Tatin treats two packages that differ only in build number
    as the same version of the same package.

    (See examples of **full package ID**.)


dependency
: Contrary to normal usage, a dependency of a package `foo` is another package on which `foo` depends.


full package ID
: A package is uniquely identified by its full package ID: group, name, and **version**.

    Group, name and version are separated by hyphens.
    When shown, a **build number** is separated from the patch by a plus sign.

    Some examples:

        apltree-Foo-1.0.0
        apltree-Foo-1.0.1+123
        apltree-Foo-1.0.1-alpha-1         ⍝ beta
        apltree-Foo-1.0.2-alpha-1+127     ⍝ beta


fully qualified namespace
: A reference beginning with `#.` or `⎕SE.`

known registries
: Registries specified in your [user settings](user-settings.md)

    A registry scan searches known registries with a priority above zero,
    in descending order of priority.

package alias
: A short name you can use locally as an alternative to a full package ID.
    (Allows you to work with multiple versions of the same package.)

package ID
: A case-insensitive pattern for matching against full package names: the name of a package, possibly also including its group; major version number; major and minor version numbers; or major and minor version and patch numbers. Examples:

    ```apl
    'MarkAPL'                  ⍝ name
    'aplteam-MarkAPL'          ⍝ group-name
    'MarkAPL-11'               ⍝ name-major
    'APLTEAM-MARKAPL-12.1'     ⍝ group-name-major-minor
    'MarkAPL-13.1.2'           ⍝ name-major-minor-patch
    'aplteam-MarkAPL-13.1.2'   ⍝ group-name-major-minor-patch
    ```

parameter space
: A namespace of variables representing parameters


registry alias
: A short name you can use locally as an alternative to its URL.
<!-- FIXME Can I also use as an alternative to a path? -->

: Tatin is installed with `tatin` and `tatin-test` as aliases for the Principal Registry and the Test Server.


string
: A character vector, e.g. `'abc def'`


strings
: A vector of strings, e.g. `'abc' 'def'`


version
: A version follows the conventions of
    [Semantic Versioning](https://semver.org):
    and comprises major and minor versions (integers)
    and a patch, separated by periods,
    e.g. `1.0.3`

    A patch is also an integer,
    but may be followed by a hyphen and a text description,
    indicating a **beta** version, e.g. `1.0.3-trial`.

    A new

    -   major version marks a breaking change
    -   minor version marks a non-breaking change
    -   patch marks a change in implementation

    (See examples of **full package ID**.)

