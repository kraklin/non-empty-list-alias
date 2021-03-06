# List.NonEmpty alias with Zipper

[![Build Status](https://travis-ci.org/turboMaCk/nonempty-list-alias-with-zipper.svg?branch=master)](https://travis-ci.org/turboMaCk/nonempty-list-alias-with-zipper)

**Work in progress**

Functions for `NonEmptyList` you already have and `Zipper` implementation that goes with it.

This implementation of non-empty list is using alias to pair `(a, List a)` so comparable type can be produced without dependency on this library.
Despite this, this package aims to provide the most feature complete implementation of `NonEmptyList` for Elm and
includes variety of practical functions like Json decoder as well as high level combinators like `duplicate` and `extend`.

## Motivation

[elm/core](https://package.elm-lang.org/packages/elm/core/latest/) doesn't come with any sort of `NonEmptyList` type.
This means one usually has to rely on one of the user implementations on `NonEmpty` type:

* [mgold/elm-nonempty-list](https://package.elm-lang.org/packages/mgold/elm-nonempty-list/latest/)
* [hrldcpr/elm-cons](https://package.elm-lang.org/packages/hrldcpr/elm-cons/latest/)

These implementations usually define custom type like `NonEmpty a = Cons a (List a)` and expose the constructor
to make pattern matching possible. This makes it hard for library authors to provide
support for `NonEmpty` because they would need to pick one of these libraries and use it as a dependency
of their own implementation and essentially impose this decision on their users.

This implementation uses different approach. `NonEmpty` is an alias for the pair `type alias NonEmpty a = (a, List a)`.
Relying on anonymous data-type like tuple means:

1. [Libraries](https://package.elm-lang.org/packages/elm-community/list-extra/latest/List-Extra#uncons) can produce `NonEmpty` data without depending on specific implementation
1. Implementation provided by this package can be easily replaced by different implementation using the same type.
1. Users maye choose to work with tuple directly without needing to transform from and to `NonEmpty` type.

### Zipper

One of the areas where downside of current approaches is already noticeable is when it comes to Zipper implementation.

* [jjant/elm-comonad-zipper](https://package.elm-lang.org/packages/jjant/elm-comonad-zipper/latest/)
* [wernerdegroot/listzipper](https://package.elm-lang.org/packages/wernerdegroot/listzipper/latest/)
* [yotamDvir/elm-pivot](https://package.elm-lang.org/packages/yotamDvir/elm-pivot/latest/Pivot)

All of which are usually constructed using `List a -> Maybe (Zipper a)` instead of `NonEmpty a -> Zipper a`.
The motivation behind including `Zipper` in this package is to encourage its usage together with `NonEmpty` list.
My favorite implementation of zipper which doesn't rely on `NonEmpty` is [zwilias/elm-holey-zipper](https://package.elm-lang.org/packages/zwilias/elm-holey-zipper/latest).

### Drawbacks

Compared to other implementations this implementation has less descriptive constructor in value space.
This means that pattern matching happens on the pair instead of explicit constructor like `Cons` or `NonEmpty`.

**conventional library:**

```elm
matchNonEmpty : NonEmptyList a -> foo
matchNonEmpty (Cons h t) =
    .....
```

**With this library:**

```elm
matchNonEmpty : NonEmptyList a -> foo
matchNonEmpty (h, t) =
    .....
```

## For Haskell Fanboys

`List.NoneEmpty.NonEmpty` is:

* Functor
* Foldable
* Applicative
* Monad
* Comonad

`List.NonEmpty.Zipper` is:

* Functor
* Foldable
* Applicative
* Comonad
