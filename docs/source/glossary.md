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

full package ID
: The name of a package with its group and major, minor, and patch numbers. Package IDs are case-insensitive: the following are equivalent.

	```apl
    'aplteam-MarkAPL-13.1.2'   ⍝ group-name-major-minor-patch
    'aplteam-markapl-13.1.2'   ⍝ group-name-major-minor-patch
    'APLTEAM-MARKAPL-13.1.2'   ⍝ group-name-major-minor-patch
    ```

fully qualified namespace
: A reference beginning with `#.` or `⎕SE.`

known registries
: Registries specified in your [user settings](user-settings.md)

    !!! detail "Registry scans"

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

string
: A character vector, e.g. `'abc def'`

strings
: A vector of strings, e.g. `'abc' 'def'`

