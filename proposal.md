# Resolved Part-Of directives

## Contact information

Author: Sigmund Cherem ([@sigmundch][])

[DEP proposal location](https://github.com/sigmundch/DEP-nonuri-imports/blob/master/proposal.md)

## Summary

We propose changing `part of` directives to provide a URI, instead of a name, to
identify the library that contians it.

All other directives in dart (import, export, part) use a URI to uniquely
identify the relation between files, we simply propose to make this consistent
also for the part-of directive.

Because part-of directives are mainly used for tooling, we could introduce this
change in a non-breaking way. For example, producing warnings for old-style part
headers.

We should consider this proposal together with a [concurrent
DEP][DEP-nonuri-imports] that proposes encoding imports as library identifiers.
At a syntax level, that proposal would make the changes for part-of directive
smaller when implementing this proposal. Instead of requiring users to change
from a library name to a URI, we would just require that the library name is
consistent with a set of resolution rules. Please refer to the other proposal
for more details.

## Example

When writing a part header a user can write:
```dart
part of 'package:foo/bar.dart'; // is equivalent to: part of foo.bar;
```

## Spec changes

We'd allow using a URI in part headers, as follows:

```dart
partHeader:  metadata part of uriOrLibraryIdentifier ';'
          ;
uriOrLibraryIdentifier: uri
                      | identifier ('.' identifier)* ';'
                      ;
```

[DEP-nonuri-imports]: https://github.com/sigmundch/DEP-nonuri-imports/blob/master/proposal.md
[@sigmundch]: https://github.com/sigmundch
