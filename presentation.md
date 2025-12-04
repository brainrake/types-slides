---
marp: true
---

# Type `Type` Type

Outline:

- Types in Programming Languages 5p
- Algebraic Data Types 10p
- Let's Code! 25p
- Why Types? 5p
- Advanced Example: Dependent Types 5p
- Advanced Example: Linear Types (Rust) 5p

---

# Types in Programming Languages

- Javascript Python PHP Ruby Perl Bash

- C C++ C# Java Typescript

- SML OCaml Haskell F# Kotlin Elm

- Rust 

- Rocq Agda ATS Idris F* Lean

---

## What is a programming language?

- syntax
- static semantics (compile time)
- dynamic semantics (runtime)


---

## Static vs Dynamic

Type errors at compile time or runtime


---

## Strong vs Weak Type System

Strong type systems never automatically convert values between types, as that may lead to unexpected incorrect results.


--- 

## Dynamic Types

Dynamic types are just static types with only one type (the `Any` type).

Any time something is done to a value (function call, operator application, indexing, etc), the interpreter has to check what to do:

```python
class int:
    def __plus__(self, other):
        if type(other) == int:
            return builtin_plus_int(self, other)
        else if type(other) == float
            return builtin_plus_float(float(self), other)
        # ... more cases ...
        else:
            raise Exception("type error")
```

---

# Algebraic Data Types

OK we have basic types like Int, Bool and so on. But how can we combine them? And what's up with functions?

A type can be thought of as a set containing all values that have that type.

Eg. there are two values of type `Bool`: `true` and `false`, like the set { `true`, `false` }.

---

## Sum Type: Bool (cardinality: 2)

```elm
type Bool
    = True
    | False
```

```
case x of
    True -> "yes
    False -> "no"
```

aka `if`

---

## Sum Type: Rock Paper Scissors (cardinality: 3)

```elm
type RPS
    = Rock
    | Paper
    | Scissors
```

---

## Sum Type: MaybeInt (cardinality: n+1)

```elm
type MaybeInt
    = AnInt Int
    | NoInt
```

```
case x of
    AnInt i -> i + 1
    NoInt -> 0
```

---

## Polymorphic Type : Maybe (cardinality: 1 + n)

```elm
type Maybe a
    = Nothing
    | Just a
```

```
case m of
    Nothing -> ...
    Just x -> ...
```

Types that depend on other types.


```
Just 42 : Maybe Int
```
---

## Sum Type: Result (cardinality: m + n)

```elm
type Result ok err
    = Ok ok
    | Err err
```

## Sum Type: Either (cardinality: m + n)

```elm
type Either l r
    = Left l
    | Right r
```

---

## Product Type: Vector2D (Cardinality: ℵ₀)

```elm
type Vector2D =
    MkVector2D Int Int

mkVector : Int -> Int -> Vector2D
mkVector x y =
    MkVector2D x y

first : Vector2D -> Int
first (MkVector2D x y) =
    x

second: Vector2D -> Int
second (MkVector2D x y) =
    y
```

</div>
</div>

---

## Product Type aka Tuple (cardinality: m * n)

```elm
type Tuple a b =
    MkTuple a b

mkTuple : a -> b -> Tuple a b
mkTuple x y =
    MkTuple x y

first : Tuple a b -> a
first (MkTuple x y) =
    x

second: Tuple a b -> b
second (MkTuple x y) =
    y
```
---

## Algebraic Data Type: List (cardinality: 1 / (1 - n))

```elm
type List e
    = Nil
    | Cons e (List e)
```

```
case list of
    Nil -> ...
    Cons x xs -> ...
```

---

## Function type

```elm
f : a -> b
```

```elm
apply : (a -> b) -> a -> b
apply f x =
    f x
```

```elm
compose : (a -> b) -> (b -> c) -> (a -> c)
compose f g x =
    f (g x)
```


---

# Let's Code!

--- 

## Type Myth

### "I don't need to worry about types in a dynamic language."

Wrong. Writing a type-incorrect program will cause an error in python too.

The difference is _when_: python will crash during **runtime** in **production**.

---

## Type Mythconception

### "I need to type more."

Many languages have at least partial type inference, so many type annotations are optional.

Inferred type annotations can be added to the code (via editor shortcut). However, I found that the reverse actually works better:

### Write the types first!

Types add documentation, safety, easier maintenance and better error messages _while writing the code_.

---


## Algebraic Data Types: Cardinality

<table>
<tr>Type<td></td><td>Cardinality</td></tr>
<tr><td><code>Bool</code></td><td>2</td></tr>
<tr><td><code>RPS</code></td><td>3</td></tr>
<tr><td><code>Maybe Bool</code></td><td>3</td></tr>
<tr><td><code>Either RPS Bool</code></td><td>5</td></tr>
<tr><td><code>Tuple RPS Bool</code></td><td>6</td></tr>
<tr><td><code>RPS -> Bool</code></td><td>?</td></tr>

</table>

---

## Algebraic Data Types: Recap

```
type Tuple a b = MkTuple a b
```

```
type Either l r = Left l | Right r
```

```
f : a -> b
```

---

## Everything is connected

<table class = "big">
<tr><td>Programming</td><td>Math&nbsp;&nbsp;&nbsp;&nbsp;</td></tr>
<tr><td>Type</td><td>Set</td></tr>
<tr><td><code>Either a b</code></td><td>A ⋃ B</td></tr>
<tr><td><code>Tuple a b</code></td><td>A ⨉ B</td></tr>
<tr><td><code>a -> b</code></td><td>B<sup>A</sup></tr>
</table>

$$ B^A = \left\{ (b_1, b_2, \dots, b_{|A|}) \in B^{|A|} \right\} $$

$$ B^n = \underbrace{B \times B \times \dots \times B}_{\text{n times}} $$

---

## Everything is connected

<table class = "big">
<tr><td>Programming</td><td>Math&nbsp;&nbsp;&nbsp;&nbsp;</td><td>Arithmetic</td></tr>
<tr><td>Type</td><td>Set</td><td>Number</td></tr>
<tr><td><code>Either a b</code></td><td>A ⋃ B</td><td>𝑎 + 𝑏</td></tr>
<tr><td><code>Tuple a b</code></td><td>A ⨉ B</td><td>𝑎 ⋅ 𝑏</td></tr>
<tr><td><code>a -> b</code></td><td>B<sup>A</sup></td><td>𝑏<sup>𝑎</sup></td></tr>
</table>

$$ B^A = \left\{ (b_1, b_2, \dots, b_{|A|}) \in B^{|A|} \right\} $$

$$ B^n = \underbrace{B \times B \times \dots \times B}_{\text{n times}} $$

---

## Everything is connected

<table class = "big">
<tr><td>Programming</td><td>Math&nbsp;&nbsp;&nbsp;&nbsp;</td><td>Arithmetic</td><td>Logic&nbsp;&nbsp;&nbsp;&nbsp;</td></tr>
<tr><td>Type</td><td>Set</td><td>Number</td><td>Proposition</td></tr>
<tr><td><code>Either a b</code></td><td>A ⋃ B</td><td>𝑎 + 𝑏</td><td>𝑝 ⋁ 𝑞</td></tr>
<tr><td><code>Tuple a b</code></td><td>A ⨉ B</td><td>𝑎 ⋅ 𝑏</td><td>𝑝 ⋀ 𝑞</td></tr>
<tr><td><code>a -> b</code></td><td>B<sup>A</sup></td><td>𝑏<sup>𝑎</sup></td><td>𝑝 → 𝑞</td></tr>
</table>

$$ B^A = \left\{ (b_1, b_2, \dots, b_{|A|}) \in B^{|A|} \right\} $$

$$ B^n = \underbrace{B \times B \times \dots \times B}_{\text{n times}} $$

---

## Programs and Logic are the same!

<table class = "big">
<tr><td>Programming</td><td>Logic</td></tr>
<tr><td>Type</td><td>Theorem</td></tr>
<tr><td>Program</td><td>Proof</td></tr>
</table>

### Curry-Howard Isomorphism

---

# Why Types?

<div class="col2">
<div>

Avoid:

- runtime errors 🤬
- undefined behavior 💥
- security exploits 🥷
- data loss 😵
- downtime ⏳
- expensive/hacky fixes 💸

</div>
<div>

Unblock:

- formally verified code
- safety critical systems
- robust large-scale systems

</div>
</div>

---

## When Types?

- modeling data (db, ui, network, state, etc.)
- any serious app (>1000 lines)
- use what is available (Typescript meh)

---

# Advanced Example: Dependent Types

Dependent types are polymorphic types where types depend not only on other types, but on values as well.

Example: `Vect` is similar to `List`, but the length is known at compile time.

```
[1, 2, 3] : List Int

[1, 2, 3] : Vect 3 Int
```

---

## Vect

```idris
data Vect : (len : Nat) -> (e : Type) -> Type where
    Nil :  Vect 0 e
    Cons : e -> Vect len e -> Vect (len + 1) e
```

```
case vect of
    Nil -> ...
    Cons x xs -> ...
```

---

## Vect

```idris2
data Vect : (len : Nat) -> (e : Type) -> Type where
    Nil :  Vect 0 e
    Cons : e -> Vect len e -> Vect (len + 1) e
```

```idris2
append : Vect m e -> Vect n e -> ?
```

---

## Vect

```idris2
data Vect : (len : Nat) -> (e : Type) -> Type where
    Nil :  Vect 0 e
    Cons : e -> Vect len e -> Vect (len + 1) e
```

```idris2
append : Vect m e -> Vect n e -> Vect (n + m) e
```

---

## Vect

```idris2
data Vect : (len : Nat) -> (e : Type) -> Type where
    Nil :  Vect 0 e
    Cons : e -> Vect len e -> Vect (len + 1) e
```

```idris2
append : Vect m e -> Vect n e -> Vect (n + m) e
append Nil Nil = Nil
append Nil v = v
append v Nil = v
append (Cons x xs) ys = (Cons x (append xs ys))
```

---

## Matrix

```
Matrix m n e = Vect m (Vect n e)
```

```
multiply : Matrix a b e -> Matrix b c e -> Matrix a c e
```

Compile-time error when trying to multiply incompatible matrixes.

---


# Advanced Example: Linear Type

https://docs.idris-lang.org/en/latest/st/examples.html


---

Thanks

https://github.com/brainrake/

---

<style>
.col2 {
    display: grid;
    grid-auto-columns: 1fr 1fr;
    grid-auto-flow: column;
}

.big {
    font-size: 1.5em;
}
table {
    table-layout: fixed;
}
</style>

<script>
import * as Viz from "@viz-js/viz";

Viz.instance().then(viz => {
  document.body.appendChild(viz.renderSVGElement("digraph { a -> b }"))
});
</script>
