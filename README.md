# Learning Haskell

Learning Haskell is apparently difficult with a steep learning curve but there are benefits!

* The concepts (pure functions, monads, lazy evaluation, advanced type systems) are valuable and transfer to other languages
    * Functional programming is a style of writing software where programs are built from pure functions, i.e., functions that always return the same output for the same input and have no side effects.
* Languages influenced by it include Elm, PureScript, Scala, and Rust's type system

Haskell is a purely functional language, meaning it enforces the paradigms of functional programming at the language level rather than leaving them as conventions. R supports a functional style - functions are first-class, and tools like {purrr} encourage it - but it also allows mutable state and side effects freely, so functional programming in R is a convention rather than a requirement. Haskell makes impurity explicit and opt-in, which is a fundamentally stricter discipline.

For example, in R a function can silently modify global state, breaking the pure function guarantee:

```r
x <- 10
add_to_x <- function(n) {
  x <<- x + n  # modifies global x as a side effect
  x
}
add_to_x(5)  # returns 15
add_to_x(5)  # returns 20 - different result for the same input!
```

In Haskell, a regular function is guaranteed pure by the type system. Side effects must be declared explicitly in the type signature using `IO`:

```haskell
-- Pure: the type signature guarantees no side effects
addToX :: Int -> Int -> Int
addToX x n = x + n  -- always returns the same result for the same inputs

-- Impure: IO in the type signature signals a side effect
addAndPrint :: Int -> Int -> IO Int
addAndPrint x n = do
  let result = x + n
  print result  -- side effect must be declared
  return result
```

## Getting Started

The recommended way to install Haskell is via GHCup, which manages the compiler (GHC) and build tools:

```console
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```
```
Welcome to Haskell!

This script can download and install the following binaries:
  * ghcup - The Haskell toolchain installer
  * ghc   - The Glasgow Haskell Compiler
  * cabal - The Cabal build tool for managing Haskell software
  * stack - A cross-platform program for developing Haskell projects (similar to cabal)
  * hls   - (optional) A language server for developers to integrate with their editor/IDE

ghcup installs only into the following directory,
which can be removed anytime:
  /home/dtang/.ghcup

Press ENTER to proceed or ctrl-c to abort.
Note that this script can be re-run at any given time.
[snipped]
System requirements
  Please ensure the following distro packages are installed before continuing (you can exit ghcup and return at any time): build-essential curl libffi-dev libffi8 libgmp-dev libgmp10 libncurses-dev libncurses5 libtinfo5 pkg-config
[snipped]
All done!

To start a simple repl, run:
  ghci

To start a new haskell project in the current directory, run:
  cabal init --interactive

To install other GHC versions and tools, run:
  ghcup tui

If you are new to Haskell, check out https://www.haskell.org/ghcup/steps/
```

Reload your shell:

```console
. ~/.zshrc
```

Set default version.

```console
ghcup set ghc recommended
```
```
[ Warn  ] New ghc version available. If you want to install this latest version, run 'ghcup install ghc 9.14.1'
[ Warn  ] New cabal version available. If you want to install this latest version, run 'ghcup install cabal 3.16.1.0'
[ Warn  ] New stack version available. If you want to install this latest version, run 'ghcup install stack 3.9.1'
[ Info  ] GHC 9.6.7 successfully set as default version
```

### Hello World

Haskell scripts use the `.hs` extension.

```haskell
main :: IO ()
main = putStrLn "Hello, World!"
```

To run it interpreted (no compilation step):

```bash
runghc hello.hs
```
```
Hello, World!
```

Compile to binary.

```bash
ghc -o hello hello.hs
```
```
[1 of 2] Compiling Main             ( hello.hs, hello.o )
[2 of 2] Linking hello
```

Execute.

```console
./hello
```
```
Hello, World!
```
