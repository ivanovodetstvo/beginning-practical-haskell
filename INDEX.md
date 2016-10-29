# Beginning Practical Haskell

## Overview

* Will teach the essentials required to write real programs in Haskell
* Will only dig into mathematical underpinnings as needed to understand real-world programming problems
* Will enable the student to use the tools needed to build real programs and consume Haskell libraries from third parties
* Intend to complement other courses offered by [Seattle Area Haskell Users' Group][seahug]

## Prerequisites

* This course will use [Stack][stack]
* Please follow [setup instructions][stackhowto] to install Stack
* Make sure the example in the setup guide works:

```bash
stack new my-project
cd my-project
stack setup
stack build
stack exec my-project-exe
```

* This course will use the [LTS 7.5][lts75] snapshot of Stack which uses [GHC 8.0.1][ghc801]

## Part 1

### 1.1 What is Haskell?

*[Sources: [1][haskellwikifp], [2][wikipediahaskell]]*

* Purely functional programming language
* Non-strict
* Statically typed

#### Purely functional programming language

* Functional
	* Functions are first-class objects
	* Effectively values that can be passed around
* Pure
	* Haskell functions more closely resemble mathematical functions
	* Given any input value, they return the same output
	* This is _referential transparency_
	* Typically operate on immutable data
	* No side effects

#### Strictness

* Function arguments not evaluated unless they're actually used

#### Static typing

* Catch many kinds of programmer error at _compile time_
* Expressive type system allows programmer lots of power and flexibility

### What else?

* Haskell has a clean, minimal syntax
* Much of this is a consequence of some of these other characteristics
* Example: non-strict evaluation allows us to _build_ certain flow
control constructs where other languages require language-level syntax

### 1.2 Let's do something

#### Interactive (REPL) Haskell

* Fire up your terminal or command prompt
* Create a new project named `hello-world`

```bash
stack new hello-world --resolver=lts-7.5
cd hello-world
```

* Fire up GHCI

```bash
stack ghci
```

* GHC is the Glasgow Haskell Compiler
* GHCI is GHC _Interactive_
* It's GHC's read-evaluate-print-loop (REPL)

```ghci
λ> x = 5
λ> y = 6
λ> z = x + y
λ> z
11
λ> :t z
z :: Num a => a
λ> :t 5
5 :: Num t => t
λ> z = "hello"
λ> z
"hello"
λ> :t z
z :: [Char]
λ> :q
```

#### Your first Haskell source file

* Let's do some similar stuff in a source file
* Create `Hello.hs`:

```haskell
x = 5
y = 6
z = x + y

main = print z
```

* Run it as follows:

```bash
stack runhaskell Hello.hs
```

* Change `Hello.hs` to the following to mimic the GHCI example:

```haskell
x = 5
y = 6
z = x + y
z = "hello"

main = print z
```

* Run it again:

```console
> stack runhaskell Hello.hs

Hello.hs:4:1: error:
    Multiple declarations of ‘z’
    Declared at: Hello.hs:3:1
                 Hello.hs:4:1
```

#### Differences between GHC and GHCI

* Names can be _shadowed_ in GHCI: i.e. we can introduce a new `z` that _hides_ the previous definition with name `z`
* In "full" Haskell, a top-level name can be used exactly once
* Though "full" Haskell allows shadowing within nested lexical scopes

### 1.3 A more realistic example

*[Sources: [1][haskellnumbers]]*

* Let's add a `module` declaration and type annotations
* Start up GHCI again

```ghci
λ> x :: Integer; x = 5
λ> y :: Integer; x = 6
λ> z = x + y
λ> z
11
λ> :t x
x :: Integer
λ> :t y
y :: Integer
λ> :t z
z :: Integer
λ> a = 5
λ> :t a
a :: Num t => t
```

* Let's do that in our source file:

```haskell
x :: Integer
x = 5

y :: Integer
y = 6

z :: Integer
z = x + y

main :: IO ()
main = print z
```

* Consider the type of `a`:
  * Items to the left of `=>` are _type constraints_
  * Lower-case `t` is a _type variable_ and can be any type that fulfils the type constraints
  * `Num t` constrains `t` to be an instance of the `Num` _type class_
  * For now, it suffices to say that `Num` is a _type class_ which has an instance for (or "is implemented by") all numeric types in Haskell
* Consider the type of `x`, `y` and `z`:
  * These have no `=>` and, therefore, no type constraints
  * Upper-case `Integer` is a _concrete type_ corresponding to arbitrary-precision integers: this is an _instance_ of `Num`

[ghc801]: https://downloads.haskell.org/~ghc/master/users-guide/8.0.1-notes.html
[haskellnumbers]: https://www.haskell.org/tutorial/numbers.html
[haskellwikifp]: https://wiki.haskell.org/Functional_programming
[lts75]: https://www.stackage.org/lts-7.5
[seahug]: http://seattlehaskell.org/
[stack]: https://docs.haskellstack.org/
[stackhowto]: https://docs.haskellstack.org/en/stable/README/#how-to-install
[wikipediahaskell]: https://en.wikipedia.org/wiki/Haskell_(programming_language)