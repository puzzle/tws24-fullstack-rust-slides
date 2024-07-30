<!-- .slide: class="master-cover" -->

## Tech Workshop 24<br>Rust Fullstack Development

<br/>

### 5.9.2024
<!-- .element style="margin-bottom: 12rem" --->

note:
## Agenda

* Kurzeinführung/Refresher in Rust, abgestimmt auf das eingesetzte Web Framework und Beispielprojekt
* Tipps um nicht vom Borrow Checker aufgehalten zu werden
* Einführung in das Web Framework
* Einführung in Beispielprojekt
* Selbständiges Studieren und Erweitern des Beispielprojektes

-*-*-

# Rust

* Multi-paradigm, influenced by functional and OO programming
* Memory-safe without garbage collector
* Wide field of application thanks to unsafe code and macros
  * From embedded programming to web and desktop apps
* No undefined behaviour unlike C/C++

-*-*-

## Rust Syntax: Variable Binding

```rust
let x = 42;  // defaults to i32 type
let _x = 42  // _ = don't warn if not used
```
<!-- .element class="very-big" --->

All variables must be initialized before use.

-*-*-

## Rust Syntax: Tuples

```rust
let pair = ('a', 17);
pair.0; // this is 'a'
pair.1; // this is 17
```
<!-- .element class="very-big" --->

Or with type annotations:

```rust
let pair: (char, i32) = ('a', 17);
pair.0; // this is 'a'
pair.1; // this is 17
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Destructuring Tuples

```rust
let (some_char, some_int) = ('a', 17);
// now, `some_char` is 'a', and `some_int` is 17
```
<!-- .element class="very-big" --->

This is especially useful when a function returns a tuple:

```rust
let (left, right) = slice.split_at(middle);
```
<!-- .element class="very-big" --->

```rust
let (_, right) = slice.split_at(middle);
```
<!-- .element class="very-big" --->

The variable `_` is commonly use to throw away values.

-*-*-

## Rust Syntax: Functions

Function that doesn't return anything:

```rust
fn greet() {
    println!("Hi there!");
}
```
<!-- .element class="very-big" --->

Function that returns an integer:
```rust
fn fair_dice_roll() -> i32 {
    4
}
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Blocks

<!-- .slide: data-visibility="hidden" -->

```rust
    // This prints "in", then "out"
    let x = "out";
    {
        // this is a different `x`
        let x = "in";
        println!("{}", x);
    }
    println!("{}", x);
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Expressions

Blocks are also expressions, they evaluate to a value.

```rust
let x = { 42 };  // x is now 42
```
<!-- .element class="very-big" --->

```rust
let x = {
    let y = 1; // first statement
    let z = 2; // second statement
    y + z // this is the *tail* - what the block will evaluate to
};
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Expressions

If conditionals are also expressions:

```rust
fn fair_dice_roll() -> i32 {
    if feeling_lucky {
        6
    } else {
        4
    }
}
```
<!-- .element class="very-big" --->

A match is also an expression:

```rust
fn fair_dice_roll() -> i32 {
    match feeling_lucky {
        true => 6,
        false => 4,
    }
}
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Modules

```rust
let least = std::cmp::min(3, 8); // this is 3
```
<!-- .element class="very-big" --->

`std` is a crate (~ a library), `cmp` is a module (~ a source file), and `min` is a function.

```rust
use std::cmp::min;

let least = min(7, 1); // this is 1
```
<!-- .element class="very-big" --->

`use` directives can be used to "bring in scope" names from other namespaces.

-*-*-

## Rust Syntax: Types

Types are namespaces too, and methods can be called as regular functions:

```rust
let x = "amos".len(); // this is 4
let x = str::len("amos"); // this is also 4
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Structs

```rust
struct Vec2 {
    x: f64, // 64-bit floating point, aka "double precision"
    y: f64,
}
```
<!-- .element class="very-big" --->

They can be initialized using struct literals:

```rust
let v1 = Vec2 { x: 1.0, y: 3.0 };
let v2 = Vec2 { y: 2.0, x: 4.0 };
// the order does not matter, only the names do
```
<!-- .element class="very-big" --->

-*-*-

## Rust Syntax: Destructuring Structs

```rust
let v = Vec2 { x: 3.0, y: 6.0 };
let Vec2 { x, y } = v;
// `x` is now 3.0, `y` is now `6.0`
```
<!-- .element class="very-big" --->

```rust
let Vec2 { x, .. } = v;
// .. throws away all other struct members
```
<!-- .element class="very-big" --->


-*-*-

## (Rust Syntax: Pattern Matching)



-*-*-

## Rust Syntax: Methods

-*-*-

## Rust Syntax: Immutability

-*-*-

## Rust Syntax: Generics

-*-*-

## Rust Syntax: Macros

-*-*-

## Rust Syntax: Enums

-*-*-
## Rust Syntax: Option
-*-*-
## Rust Syntax: Result
-*-*-

## Rust Syntax: Error Handling

-*-*-

## Rust Syntax: Iterators

-*-*-

## Rust Syntax: Closures

-*-*-

## Rust Syntax: Traits

-*-*-

## (Rust Syntax: Lifetimes)

-*-*-

https://fasterthanli.me/articles/a-half-hour-to-learn-rust

-*-*-

## Installation

Install Rust, should work on Linux and macOS:

```sh
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
rustup toolchain install nightly
rustup target add wasm32-unknown-unknown
```

<!-- <small> -->

This installs stable and nightly versions of:

<div class="container">

<div class="col">
<small>

* rustc: The Rust compiler
* rust-std: The Rust standard library
* cargo: Dependency manager/build tool
* rustdoc: Documentation generator

</small>
</div>

<div class="col">
<small>

* rustfmt: Code formatter
* clippy: Additional lints
* wasm32: Webassembly compilation target

</small>
</div>


<!-- </small> -->

-*-*-

## Installation

Install Leptos support:

```sh
cargo install cargo-leptos leptosfmt
```
