# Resolved Part-Of directives

## Contact information

Author: Sigmund Cherem ([@sigmundch][])

[DEP proposal location](https://github.com/sigmundch/DEP-resolved-part-of/blob/master/proposal.md)

## Summary

We propose changing `part of` directives to specify either a library
name (as it is today) or a URI to identify the library that contains it.

All other directives in dart (import, export, part) use a URI to uniquely
identify the relation between files, this proposal is one step towards making
this consistent also for the part-of directives.

Because part-of directives are mainly used for tooling, we propose introducing
this change in a non-breaking way. Providing a library name continues to be
allowed. However, we should consider this proposal together with a [concurrent
DEP][DEP-nonuri-imports] that proposes encoding imports as library identifiers.
At a syntax level, that proposal matches the identifier syntax for part-of
directives today, but semantically it makes library names syntactic sugar for
URIs. If that proposal is accepted, it would be natural to make `part of`
directives produce a compile-time error if the library name doesn't expand to a
known library accoding to the [desugaring rules][rules]. This error would be
consistent with what other directives do today. Alternatively, if we wanted to
avoid making this a breaking change, we could produce a static warning until
Dart reaches version 2.0.

For additional background, see this [thread in core-dev](https://groups.google.com/a/dartlang.org/forum/#!topic/core-dev/Mtii4OONYkQ).

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

We would change section **18.3** as follows (~~strikethrough~~ means to delete,
**bold** means to add):

> A part header begins with `part of` followed by **either a URI** or the name of
> the library the part belongs to.  A part declaration consists of a part header
> followed by a sequence of top-level declarations.  **It is a compile-time
> error if the specified URI does not refer to a library declaration.**
>
> Compiling a part directive of the form `part s`; causes the Dart system to
> attempt to compile the contents of the URI that is the value of *s*. The
> top-level declarations at that URI are then compiled by the Dart compiler in
> the scope of the current library. It is a compile-time error if the contents
> of the URI are not a valid part declaration. It is a static warning if the
> referenced part declaration *p* ~~names~~ **refers to** a library other than
> the current library as the library to which *p* belongs.

If the [concurrent proposal][DEP-nonuri-imports] is also accepted, we'd change
this section further to reflect that now library identifiers may produce an
error (the text below shows only the additional changes on top of the previous
changes we showed earlier):

> A part header begins with `part of` followed by either a URI or the ~~name~~
> **identifier** of the library the part belongs to.  A part declaration
> consists of a part header followed by a sequence of top-level declarations.
> It is a compile-time error if the specified URI **or library identifier** does
> not refer to a library declaration.
>
> ...


[DEP-nonuri-imports]: https://github.com/sigmundch/DEP-nonuri-imports/blob/master/proposal.md
[@sigmundch]: https://github.com/sigmundch
[rules]: https://github.com/sigmundch/DEP-nonuri-imports/blob/master/proposal.md#syntax-and-semantics
