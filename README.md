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

