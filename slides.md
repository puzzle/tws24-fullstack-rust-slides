<!-- .slide: class="master-cover" -->

## Rust Fullstack Development

<br/>

### Tech Workshop<br>5.9.2024
### Daniel Tschan<br>Mathis Hofer
<!-- .element style="margin-bottom: 12rem" --->

-*-*-

# Agenda

- Introduction to Rust
- Introduction to the Leptos Web Framework
- Hands-On

note:
* Kurzeinführung/Refresher in Rust, abgestimmt auf das eingesetzte Web Framework und Beispielprojekt
* Tipps um nicht vom Borrow Checker aufgehalten zu werden
* Einführung in das Leptos Web Framework
* Einführung in Beispielprojekt
* Selbständiges Studieren und Erweitern des Beispielprojektes

<!-- .slide: class="master-agenda" -->

-*-*-

<div class="nr"></div>

# Rust Introduction

<!-- .slide: class="master-title" -->

***

## What is Rust?

* Multi-paradigm, influenced by functional and OO programming
* Memory-safe without garbage collector
* Wide field of application thanks to unsafe code and macros
  * From embedded programming to web and desktop apps
* No undefined behaviour unlike C/C++
* Statically and strongly typed with inference

***

## Rust Syntax: Variable Binding

```rust
let x = 42;  // defaults to i32 type
let _x = 42  // _ = don't warn if not used
```
<!-- .element class="very-big" --->

All variables must be initialized before use.

***

## Rust Syntax: Scalar Types

* Signed int: `i8`, `i16`, `i32`, `i64`, `i128`, `isize` (ptr size)
* Unsigned int: `u8`, `u16`, `u32`, `u64`, `u128` and `usize`
* Floating point: `f32`, `f64`
* `char`: Unicode scalar like `'a'`, `'α'`, `'∞'` (4 bytes)
* `bool`: `true` or `false`
* `()`: Unit type, can only be empty tuple `()`

***

## Rust Syntax: Compound Types

* Arrays: fixed size, single type, e.g. `[1, 2, 3]`
* Tuples: fixed size, multiple types, e.g.: `(1, true)`

***

## Rust Syntax: Notable String Types

* `String`:
  * A growable, heap-allocated string.
  * Used when you need to own and modify string data.
  * Can be converted from and to slices (&str).
  * Commonly seen in data structures.

***

## Rust Syntax: Notable String Types

* `&str` ("stir", "String Slice"):
  * A non-owning reference to a sequence of UTF-8 encoded string data.
  * Used for reading string data without taking ownership.
  * Commonly seen as function parameters.
  * String literals have type `&str`

***

## Rust Syntax: Notable String Types

```text
  String
    | addr | ---------> Hello World!
    | size |                    |
    | cap  |                    |
                                |
    &str                        |
    | addr | -------------------|
    | size |
```

***

## Rust: Type Inference

* Rust types are inferred where possible
* Function arguments must always be specified
  * To prevent implementation from changes affecting call sites
* Rust supports partial type inference:

    ```rust
    let values = (1 .. 10).collect::<Vec<_>>();
    ```
<!-- .element class="very-big" --->

***

## Rust: Ownership and Moves

* Each value in Rust has an owner
* There can only be one owner at a time
* When the owner goes out of scope, the value will be dropped
* Ownership can be transferred between variables

***

## Rust: Ownership and Moves

```rust
let message = String::from("Hello");
let message2 = message;  // Ownership of string transfered to message2
let message3 = message;  // Error: use of moved value
```
<!-- .element class="very-big" --->

```rust
fn welcome(name: &str) -> String {
    let result = format!("Hello {}", name);

    result  // Transfer ownership of string to caller
}

let message = welcome("John");  // `message` now owns the string
```
<!-- .element class="very-big" --->

***

## Rust: Copy types

* Values consisting of primitive types are copied on move operations
* The corresponding types implement the `Copy` trait

```rust
let number = 42;  // 42 is of type i32, which is copy
let number2 = number;  // value is copied, not moved
let number3 = number;  // this works, value is copied again 
```
<!-- .element class="very-big" --->

***

## Rust Syntax: References

Rust has two types of references:

* &T: Immutable, shared reference
* &mut T: Mutable, exclusive reference

***

## Rust: Borrowing

The use of references is govenered by borrowing rules:

* There can be either multiple immutable references to a value
* Or exactly one mutable reference
* A mutably referenced value can only be access through that reference
* A reference cannot outlive the referenced value

***

## Rust: Lifetimes

Every variable binding has a lifetime:
```
let r;
{
    let i = 1;
    r = &i;
}

println!("{}", r);  // Error: `i` does not live long enough
```
<!-- .element class="very-big" style="margin-top: 0.5em;" --->

Rust sometimes needs explicit lifetime specifiers:

```rust
fn foo<'a, 'b>(x: &'a u32, y: &'b u32) -> &'a u32 {
    x
}
```
<!-- .element class="very-big" style="margin-top: 0.5em;" --->

***



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

***

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

The variable `_` is commonly used to throw away values.

***

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

***

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

***

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

***

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

***

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

***

## Rust Syntax: Types

Types are namespaces too, and methods can be called as regular functions:

```rust
let x = "amos".len(); // this is 4
let x = str::len("amos"); // this is also 4
```
<!-- .element class="very-big" --->

***

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

***

## Rust: Destructuring Structs

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


***

## Rust Syntax: Pattern Matching

```rust
struct Number {
    odd: bool,
    value: i32,
}

fn main() {
    let one = Number { odd: true, value: 1 };
    let two = Number { odd: false, value: 2 };
    print_number(one);
    print_number(two);
}

fn print_number(n: Number) {
    match n {
        Number { odd: true, value } => println!("Odd number: {}", value),
        Number { odd: false, value } => println!("Even number: {}", value),
    }
}
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Methods

You can declare methods on your own types:

```rust
struct Number {
    odd: bool,
    value: i32,
}

impl Number {
    fn is_strictly_positive(self) -> bool {
        self.value > 0
    }
}
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Immutability

Variable bindings are immutable by default:

```rust
let n = Number {
    odd: true,
    value: 17,
};

n.odd = false; // error: cannot assign to `n.odd`,
               // as `n` is not declared to be mutable

n = Number {
    odd: false,
    value: 22,
}; // error: cannot assign twice to immutable variable `n`
```
<!-- .element class="very-big" --->


***

## Rust Syntax: Immutability

`mut` makes a variable binding mutable:

```rust
let mut n = Number {
    odd: true,
    value: 17,
};

n.value = 19; // all good
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Macros

`name!()`, `name![]` or `name!{}` invoke a macro.

```rust
fn main() {
    println!("{}", "Hello there!");
}
```
<!-- .element class="very-big" --->

This expands to something that has the same effect as:

```rust
fn main() {
    use std::io::{self, Write};
    io::stdout().lock().write_all(b"Hello there!\n").unwrap();
}
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Enums

Rust enums can just be constants like in other languages:

```rust
enum Message {
    Quit,
    Move,
    Write,
    ChangeColor,
}

let msg = Message::Move;
```

***

## Rust Syntax: Enums

But they can also contain data:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

let msg = Message::Move { x: 42, y: 0 }; 
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Option

Option is an enum to represent optional values:

```rust
enum Option<T> {
    None,
    Some(T),
}
```
<!-- .element class="very-big" style="margin-top: 0.5em;" --->


```rust 
fn divide(numerator: f64, denominator: f64) -> Option<f64> {
    if denominator == 0.0 {
        None
    } else {
        Some(numerator / denominator)
    }
}

let result = divide(2.0, 3.0);

match result {
    Some(x) => println!("Result: {x}"),
    None    => println!("Cannot divide by 0"),
}
```
<!-- .element class="very-big" --->


***
## Rust Syntax: Result

Result is an enum used for returning errors:
```rust
enum Result<T, E> {
   Ok(T),
   Err(E),
}
```
<!-- .element class="very-big" style="margin-top: 0.5em;" --->

```rust
fn main() {
    let number = env::args().nth(1).expect("Please provide a number");
    match number.parse::<f64>() {
        Ok(number) =>
            println!("The square root of {} is {}", number, number.sqrt()),
        Err(error) =>
            println!("Can't parse '{}' as number: {}", number, error),
    };
}
```
<!-- .element class="very-big" --->

***

## Rust: Loops and Iterators

Anything that is iterable can be used in a `for in` loop:

```rust
for i in (0..10).filter(|x| x % 2 == 0); {
    println!("{}", i);
}
```

Which also supports `break` and `continue`.

[Iterators](https://doc.rust-lang.org/std/iter/trait.Iterator.html) provide various
methods for filtering and transforming. 

***

## Rust Syntax: Traits

Traits are something multiple types have in common:

```rust
trait Signed {
    fn is_strictly_negative(self) -> bool;
}

impl Signed for Number {
    fn is_strictly_negative(self) -> bool {
        self.value < 0
    }
}

fn main() {
    let n = Number { odd: false, value: -44 };
    println!("{}", n.is_strictly_negative()); // prints "true"
}
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Traits

Traits can be implemented for foreign types:

```rust
impl Signed for i32 {
    fn is_strictly_negative(self) -> bool {
        self < 0
    }
}
```
<!-- .element class="very-big" --->

And foreign traits for own types:

```rust
impl std::ops::Neg for Number {
    type Output = Number;

    fn neg(self) -> Number {
        Number {
            value: -self.value,
            odd: self.odd,
        }        
    }
}
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Traits

* But foreign traits can't be implemented for foreign types
* This is part of the orphan rules
* The orphan rules prevent incoherence, i.e. multiple implementations of a trait for a type
* Traits are quite different from Java interfaces: https://stackoverflow.com/questions/69477460/is-rust-trait-the-same-as-java-interface


* impl Trait return type

***

## Rust Syntax: Closures

```rust
let add_one = |x| x + 1;
println!("{}", add_one(2)); // prints 3
```

* Closures are functions with captured context
* Parameter types are infered if possible
* Closures that capture variables by reference are subjected to lifetime rules
* Closure have unique types, but implement traits

***

## Rust Syntax: Closures

`Fn`: For closures that don't mutate or move captured variables

```rust
let hello = String::from("Hello");
let msg = || hello.clone() + " World";
```
<!-- .element class="very-big" --->

Captures `hello` as `&String` because of `String::clone(&self) -> String`

***

## Rust Syntax: Closures

`FnMut`: For closures that mutate but don't move captured variables

```rust
let mut hello = String::from("Hello");
let mut msg = || hello.push_str(" World");
```
<!-- .element class="very-big" --->

Captures `hello` as `&mut String` because of `String::push_str(&mut self, string: &str)`

***

## Rust Syntax: Closures

`FnOnce`: For closures that move captured variables out of their bodies

```rust
let hello = String::from("Hello")
let msg = || hello + " World";
```
<!-- .element class="very-big" --->

Captures `hello` as `String` and moves it out of the closure on first call because of `String::add(self, other: &str) -> String`


***

## Rust Syntax: Moving Closures

To avoid lifetime issues closures can capture all variables by moving them:

```rust
let mut hello = String::from("Hello");
let mut msg = move || hello.clone() + " World";
```
<!-- .element class="very-big" --->

Captures `hello` as `String` instead of `&String` because of `move`.

***

## Rust Syntax: Generics

Functions can be generic:

```rust
fn foobar<L, R>(left: L, right: R) {
    // do something with `left` and `right`
}
```
<!-- .element class="very-big" --->

Type parameters usually have constraints, so you can actually do something with them.

```rust
use std::fmt::Debug;

fn compare<T>(left: T, right: T)
where
    T: Debug + PartialEq,
{
    println!("{:?} {} {:?}", left,
        if left == right { "==" } else { "!=" }, right);
}
```
<!-- .element class="very-big" --->

***

## Rust Syntax: Generics

Structs can be generic too:

```rust
struct Pair<T> {
    a: T,
    b: T,
}

fn print_type_name<T>(_val: &T) {
    println!("{}", std::any::type_name::<T>());
}

fn main() {
    let p1 = Pair { a: 3, b: 9 };
    let p2 = Pair { a: true, b: false };
    print_type_name(&p1); // prints "Pair<i32>"
    print_type_name(&p2); // prints "Pair<bool>"
}
```
<!-- .element class="very-big" --->


***

## Rust Syntax: Async

Rust supports async functions and blocks.

```rust
async fn foo() -> u8 { 5 }
```

```rust
async {
    let x: u8 = foo().await;
    x + 5
}
```

* They return a value that implements the `Future` trait.
* `await` runs a future and waits for its completion.
* Futures store the variables used in async functions/blocks
* There are no async closures in stable Rust yet
<!-- .element style="list-style-position: inside; padding-left: 0;" --->

***

## Beginner Borrow Checker Tips

* Don't store references in structs and closures
  * Use owned values instead, i.e. String, not &str
  * Use `move` closures to capture values by move
* Clone values which are still neeeded before move
* Use `to_owned` to clone `&str` into `String`

***

## Beginner Borrow Checker Tips

* Use shadowing to clone before moving closure:

```rust
let hello = String::from("hello");

let msg = {
  let hello = hello.clone();
  move || hello + " World"
};

println!("{}", hello);  // hello is still available here
```

<!-- .element class="very-big" --->

* Syntactically this is a block returning a closure
* Shadowing like this is idiomatic in rust

-*-*-

<div class="nr"></div>

# Leptos Web Framework

<!-- .slide: class="master-title" -->

***

## Rust for UI

* A movement using Rust for UI development
* WebAssembly: Run Rust in the browser
* Frameworks on the rise: \
[Leptos](https://leptos.dev/) (Web), [sauron](https://github.com/ivanceras/sauron) (Web), [Dioxus](https://dioxuslabs.com/) (cross-plattform) and many more...

→ https://www.thoughtworks.com/radar/languages-and-frameworks/rust-for-ui

***

## Leptos (1)

* Mature Rust fullstack framework
* Supports CSR and SSR applications
* Client code compiled to WebAssembly
* Server functions can be called on the client \
  <small>→ No need for REST or other API</small>
* Fine-grained reactive, no virtual DOM \
  <small>→ Inspired by Solid (JS) and Sycamore (Rust)</small>

***

## Leptos (2)

* Based on Web standards \
  <small>→ Router based on links and forms</small>
* JSX-like template format: RSX
* Signals have value semantics \
  <small>→ No fights with the borrow checker</small>
* [Performance](https://www.youtube.com/watch?v=4KtotxNAwME) like fastest JavaScript frameworks \
  <small>→ Even though WebAssembly has no direct access to DOM</small>

***

## Leptos: CSR vs. SSR

<img src="https://mermaid.ink/svg/pako:eNqNklFrwjAQx7_KcU8V6hcoQ6iRsUkHYgRf8pK2p2ZL05CkG0797kvXMR1Dt3sKxz-X3_3IAau2Jsxwo9u3aiddgNVMGIjlu3LrpN2BwIJsaD3cd1r7IKsXyK0VOMT6qpWjKqjWQLE8dwuefF_lfAkL1z7H2F3pJsmy82EkcATj8eQoIkRjlSaotCITwEYOL_AIa56sqcy9p6bUe5gqI91-dPlEPwDmPHk0Kiip1bv85Jjz36mcJw-rpyIFxnkK_dDgf6UuYDy5V3JnGD5N-ND6wUGmFuaaMq7MNo5ayC39Uxo7S2O3pH1RRkfsD0dscMRuOxpSObvqKO6JKTbkGqnq-GMOfVtg2FFDArN4rGkjOx36JU8xKrvQ8r2pMAuuoxQ7W8tAMyWjowazjdSeTh-ZzsVe" />

<!--
Edit:
https://mermaid.live/edit#pako:eNqNklFrwjAQx7_KcU8V6hcoQ6iRsUkHYgRf8pK2p2ZL05CkG0797kvXMR1Dt3sKxz-X3_3IAau2Jsxwo9u3aiddgNVMGIjlu3LrpN2BwIJsaD3cd1r7IKsXyK0VOMT6qpWjKqjWQLE8dwuefF_lfAkL1z7H2F3pJsmy82EkcATj8eQoIkRjlSaotCITwEYOL_AIa56sqcy9p6bUe5gqI91-dPlEPwDmPHk0Kiip1bv85Jjz36mcJw-rpyIFxnkK_dDgf6UuYDy5V3JnGD5N-ND6wUGmFuaaMq7MNo5ayC39Uxo7S2O3pH1RRkfsD0dscMRuOxpSObvqKO6JKTbkGqnq-GMOfVtg2FFDArN4rGkjOx36JU8xKrvQ8r2pMAuuoxQ7W8tAMyWjowazjdSeTh-ZzsVe
-->

note:

- Leptos verwendet Trunk builden/bundlen als WASM
- CSR: Artefakte können statisch geserved werden (analog JS SPA)
- SSR:
  - Server served auch WASM Binary
  - Alternativ: Fullstack WASM/WASI für WinterCG-kompatible Runtime

***

## Leptos: Life of an SSR Page Load

<!-- On server:

1. Web server receives and routes HTTP request
2. Leptos app renders static HTML
3. Web server streams HTML to browser

In browser:

4. Browser starts loading linked JS and WASM
5. Browser renders static HTML in the meantime
6. Leptos app hydrates HTML, adding interactivity

Navigation now takes place on the client. -->

<img src="https://mermaid.ink/svg/pako:eNqllE9P4zAQxb_KyIdVK1G4R6tKbBEqK6pFpOweCAc3nqYWybhrj0EV4ruvnT-Qku2C2FMi-zfP700mfhK5USgSMZlMMsoNrXWRZARQaWuNPc3ZWJfAWpYOM6qhjBz-9kg5nmlZWFlFXEYQvEML0sFNeMbVrbSsc72VxLCy5rHd_ta8nnxd2eklbtk4mJUaAzT6dZouxm9LA_vQVLZ0Wi9EK9CcOZlOuwMS-Kmd5iBQYEMEb_pBMnZEXOvcxMJGPgmqpGC-XF6BjQEdR1DhsHxP1LVeoLPZl7w2PiB9TfgS3kgFzrEMCcPm4rJfvRclZYuyemF6ZlyvBf8MGPV8LXY9OBduz2dXd4f6cY6cb-B7ehI_yjuZ91wfsPpXU_OdsrLuUbAzkkppKkATo63rNe_GcLtcXtx96GP4Nv1wKC5aSRjhcXEMeanze1h5ZkPjN9LefaixffW5JFXiq29DB-LebFXUO_ux-M88szpAqeke2IDhTQCakf9MlsGQqB3JqpuS0aPmDbz8mkPX4khUaCupVbhJniKTieCowkyEaRAK19KXnImMngMqPZt0R7lI2Ho8Er7uSXuZiKS-a57_AKcXm-s" alt="Life of a Page Load" style="width: 70%; display: block; margin: auto;" />

<!--
Edit:
https://mermaid.live/edit#pako:eNqllE9P4zAQxb_KyIdVK1G4R6tKbBEqK6pFpOweCAc3nqYWybhrj0EV4ruvnT-Qku2C2FMi-zfP700mfhK5USgSMZlMMsoNrXWRZARQaWuNPc3ZWJfAWpYOM6qhjBz-9kg5nmlZWFlFXEYQvEML0sFNeMbVrbSsc72VxLCy5rHd_ta8nnxd2eklbtk4mJUaAzT6dZouxm9LA_vQVLZ0Wi9EK9CcOZlOuwMS-Kmd5iBQYEMEb_pBMnZEXOvcxMJGPgmqpGC-XF6BjQEdR1DhsHxP1LVeoLPZl7w2PiB9TfgS3kgFzrEMCcPm4rJfvRclZYuyemF6ZlyvBf8MGPV8LXY9OBduz2dXd4f6cY6cb-B7ehI_yjuZ91wfsPpXU_OdsrLuUbAzkkppKkATo63rNe_GcLtcXtx96GP4Nv1wKC5aSRjhcXEMeanze1h5ZkPjN9LefaixffW5JFXiq29DB-LebFXUO_ux-M88szpAqeke2IDhTQCakf9MlsGQqB3JqpuS0aPmDbz8mkPX4khUaCupVbhJniKTieCowkyEaRAK19KXnImMngMqPZt0R7lI2Ho8Er7uSXuZiKS-a57_AKcXm-s
-->

***

## Why Leptos SSR

* Improved load times (First Contentful Paint)
* Static parts can be read by search engines (SEO)
* Graceful degradation for clients without JS/WASM

***

## Installation: Rust

Install<sup>*</sup>:

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

</div>

<!-- </small> -->

<small><sup>*</sup>should work on Linux and macOS</small>

***

## Installation: Leptos

- (CSR: [Manual setup](https://book.leptos.dev/getting_started/index.html) with [Trunk](https://trunkrs.dev/) and `leptos` crate.)
- SSR: Generate with [cargo-leptos](https://github.com/leptos-rs/cargo-leptos) \
  <small>→ build tool with scaffolding support & best practices</small>

Install:

```sh
cargo install cargo-leptos leptosfmt
```

note:

- CSR Projekte müssen selber eingerichtet werden
- SSR Projekte können scaffolded werden
- Wir fokussieren uns auf SSR, nur `cargo-leptos` einrichten

***

## Leptos: Reactive Primitives

***

### Signals

Basic reactive state

```rust
let (count, set_count) = create_signal(0);

// Read state
count(); // only nightly
count.get(); // stable

// Set state
set_count(1); // only nightly
set_count.set(1); // stable

// Update state
set_count.update(|n| *n += 1);
```

***

### Derived Signals/Memos

Compute state based on signals

```rust
let (count, set_count) = create_signal(1);

// Derived signal (for most cases)
let derived_signal_double_count = move || count() * 2;

// Memo (for expensive computations)
let memoized_double_count = create_memo(move |_| count() * 2);
```

→ https://docs.rs/leptos/latest/leptos/fn.create_memo.html

***

### Effects

Run side effects on state changes \
(to synchronize with non-reactive world)

```rust
let (count, set_count) = create_signal(0);

create_effect(move |_| {
  log::debug!("Count: {}", count());
});
```

***

### Contexts

Share state accross components (like React Context)

***

## Leptos: Basic Component (1)

```rust
// main.rs
fn main() {
    leptos::mount_to_body(|| view! { <App/> })
}

#[component]
fn App() -> impl IntoView {
    let (count, set_count) = create_signal(0);

    view! {
        <button
            on:click=move |_| {
                set_count.update(|n| *n += 1);
            }
        >
            "Click me: "
            {move || count()}
            // Or simply: {count}
            // But not: {count()} ⚠️
        </button>
    }
}
```
<!-- .element class="very-big" --->

note:

* `#[component]` macro annotates `App` function to be used in templates as `<App />`
* `view!` macro converts RSX into function calls
* `App` function is executed once only ⚠️
* `create_signal` creates r/w signal with initial state
* `on:click` runs on every `click` event
  * passes event as arg to closure (is ignored)
* `count` reads signal value (reactive)

***

## Leptos: Basic Component (2)

What happens?

* `#[component]` macro annotates `App` function to be used in templates as `<App />`
* `view!` macro converts RSX into function calls
* `App` function is executed once only ⚠️

***

## Leptos: Basic Component (3)

* `create_signal` creates r/w signal with initial state
* `on:click` runs on every `click` event
  * passes event as arg to closure (is ignored)
* `count` reads signal value (reactive)

***

## Leptos: Dynamic Attributes

```rust
<button
    on:click=move |_| {
        set_count.update(|n| *n += 1);
    }

    // Dynamic class
    class:red=move || count() % 2 == 1

    // Dynamic style
    style:background-color=move || format!("rgb({}, {}, 100)", count(), 100)

    // Dynamic attribute
    value=count
>
    "Click me"
</button>
```
<!-- .element class="very-big" --->

***

## Leptos: Component Props

- Reactive & static
- Optional; with default value
- Generic

```rust
#[component]
pub fn Hello(name: String) -> impl IntoView {
    view! { <p>"Hi " {name} "!"</p> }
}
```

note:

- Reactive: `ReadSignal<i32>`
- Static: `i32`
- `MaybeSignal<i32>` für Props welche static oder reactive sein können
- Generic: man kann auch Generics verwenden


***

## Leptos: Static Iteration

```rust
#[component]
pub fn List() -> impl IntoView {
    let (values, set_values) = create_signal(vec![0, 1, 2]);
    view! {
      <ul>
        {move || 
          values().into_iter()
            .map(|value| view! { <li>{value}</li> })
            .collect_view()
        }
      </ul>
    }
}
```

- Inefficient: re-renders every element in the list
- Use for static lists

note:

- `.collect_view()`: Collects an iterator or collection into a `View`

***

## Leptos: Dynamic Iteration

```rust
#[component]
pub fn List() -> impl IntoView {
    let (values, set_values) = create_signal(vec![0, 1, 2]);
    view! {
      <ul>
        <For
          each=values
          key=|v| *v
          children=|value| view! { <li>{value}</li> }
        />
      </ul>
    }
}
```

- Key must be unique
- Reuses existing items

***

## Leptos: Control Flow

```rust
// if statement
{move ||
    if count() > 5 {
        view! { <p>"Yeah!"</p> }
    } else {
        view! { <p>"Nope!"</p> }
    }
}

// bool::then()
{move || (count() > 5).then(|| view! { <p>"Yeah!"</p> })}

// Pattern matching
{move ||
    match count() {
        5 => view! { <p>"Yeah!"</p> },
        n if n % 2 != 0 => view! { <p>"Odd!"</p> },
        _ => view! { <p>"Nope!"</p> }
    }
}
```

note:

Variante optional else:

```
{move ||
    if count() > 5 {
        Some( view! {<p>"Yeah!"</p>})
    } else {
        None
    }
}
```

***

## Leptos: Children

"Unnamed slots"

```rust
#[component]
pub fn Header(
    children: Children,
) -> impl IntoView
{
    view! {
        <h2>"Header"</h2>
        {children()}
    }
}
```

```rust
<Header>
  <p>"Some content"</p>
</Header>
```

***

## Leptos: Render Props

"Named slots"

```rust
#[component]
pub fn Header<F, IV>(
    actions: F,
) -> impl IntoView
where
    F: Fn() -> IV,
    IV: IntoView,
{
    view! {
        <h2>"Render Prop"</h2>
        {actions()}
    }
}
```

```rust
<TakesChildren actions=|| view! { <p>"Some actions"</p> } />
```

***

## Leptos: Managing State

- Use signals for component-local state
- Pass values to children (prop drilling)
- Pass signals to children
- Pass signals through context
- Create lenses into state struct with `create_slice`
- State in URL

***

## Leptos: Async Primitives

* Resource: \
  Reactive (signal-based) async task (data fetching)
* Action: \
  imperative async task (data mutation)

***

### Resource

```rust
// our source signal: some synchronous, local state
let (count, set_count) = create_signal(0);

// our resource
let async_data = create_resource(
    count,
    // every time `count` changes, this will run
    |value| async move {
        logging::log!("loading data from API");
        load_data(value).await
    },
);
```
<!-- .element class="very-big" --->

Use empty signal to create resource that runs once:

```rust
let once = create_resource(|| (), |_| async move { load_data().await });
```
<!-- .element class="very-big" --->

***

### Action

```rust
let submitted = add_todo_action.input(); // RwSignal<Option<String>>
let pending = add_todo_action.pending(); // ReadSignal<bool>
let todo_id = add_todo_action.value(); // RwSignal<Option<Uuid>>

let input_ref = create_node_ref::<Input>();

view! {
    <form
        on:submit=move |ev| {
            ev.prevent_default(); // don't reload the page...
            let input = input_ref.get().expect("input to exist");
            add_todo_action.dispatch(input.value());
        }
    >
        <label>
            "What do you need to do?"
            <input type="text"
                node_ref=input_ref
            />
        </label>
        <button type="submit">"Add Todo"</button>
    </form>
    // use our loading state
    <p>{move || pending().then("Loading...")}</p>
}
```
<!-- .element class="very-big" --->

***

## Leptos: Async Components

* `<Suspense/>`: \
  Shows children if resource ready, fallback otherwise
* `<Await/>`: \
  Shows children if future ready, nothing otherwise
* `<Transition/>`: \
  Shows children if resource ready, previous data or fallback otherwise

***

### Leptos: Suspense Component

```rust
let (name, set_name) = create_signal("Bill".to_string());

let async_data = create_resource(
    name,
    |name| async move { important_api_call(name).await },
);

view! {
    <p><code>"name:"</code> {name}</p>
    <Suspense
        // the fallback will show whenever a resource
        // read "under" the suspense is loading
        fallback=move || view! { <p>"Loading..."</p> }
    >
        // the children will be rendered once initially,
        // and then whenever any resources has been resolved
        <p>
            "Your shouting name is "
            {move || async_data.get()}
        </p>
    </Suspense>
}
```
<!-- .element class="very-big" --->

***

### Leptos: Await Component

```rust
async fn calc_ultimate_answer() -> i32 {
    TimeoutFuture::new(1_000).await;
    42
}

view! {
    <Await
        // `future` provides the `Future` to be resolved
        future=|| calc_ultimate_answer()
        // the data is bound to whatever variable name you provide
        let:data
    >
        // you receive the data by reference and can use it in your view here
        <p>"The answer to life, the universe, and everything is " {*data}</p>
    </Await>
}
```
<!-- .element class="very-big" --->

***

## Leptos: Server Functions

* Leptos supports the concept of **server functions**
  * Are running on the server
  * Are async
  * Are co-located with the component code
  * Can be called from server or browser
  * Always return `Result<T, ServerFnError>`
  * Have no access to client state

***

## Leptos: Server Functions

```rust
#[server(AddTodo, "/api")]
pub async fn add_todo(title: String) -> Result<(), ServerFnError> {
    let mut conn = db().await?;

    match sqlx::query(
            "INSERT INTO todos (title, completed) VALUES ($1, false)")
        .bind(title)
        .execute(&mut conn)
        .await
    {
        Ok(_row) => Ok(()),
        Err(e) => Err(ServerFnError::ServerError(e.to_string())),
    }
}
```
<!-- .element class="very-big" --->

Can be called on the client with spawn_local or action:

```rust
spawn_local(async {
    add_todo("So much to do!".to_string()).await;
});
```
<!-- .element class="very-big" style="margin-top: 0.5em;" --->

***

## Leptos: Server Function API

* Server functions are called with a `POST` request
* Arguments are URL-encoded form data
  * Allows them to be called from HTML forms
  * Graceful degradation when JS/WASM isn't available
* Results are encoded with JSON 
* Can be switched to `GET` for function for caching
* Arguments and results must be serializable

***

## Leptos: Routing (1)

- Separate `leptos_router` package
- Supports CSR, SSR & Hydration
- Define routes in component view with `<Router>` and `<Route path="/about" view=About />`
- Create links with `<A>`

note:

Similar to React Router

***

## Leptos: Routing (2)

- Access route params (`/users/:id`): \
  `use_params`
- Access query params (`/search?q=Foo`): \
  `use_queries`

https://book.leptos.dev/router/16_routes.html

***

## Leptos: Form Handling

- Controlled & uncontrolled inputs
- Progressively enhanced components from `leptos_router`:
  - `<Form/>`: `action` is an URL
  - `<ActionForm/>`: `action` is an `Action`
  - `<MultiActionForm/>`: `action` is a `MultiAction`

note:

- Controlled/uncontrolled → analog React
- Controlled = Input-State wird mit lokalem Signal getracked und `value` aktualisiert
- Uncontrolled = Browser tracked Input-State
- Form components sind integriert mit Client-side Routing, funktionieren aber auch ohne JS

***

## Leptos: Styling Components

- Built-in `<Stylesheet>`
- Tailwind (already integrated in cargo-leptos)
- Stylers: Compile-time CSS Extraction
- Stylance: Scoped CSS Written in CSS Files
- Styled: Runtime CSS Scoping

https://book.leptos.dev/interlude_styling.html

note:

- `<Stylesheet>`: creates `<link>` tag, preprocessed with cargo-leptos
- Stylers: unscoped, inline or in separate file
- Stylance: in separate file
- Styled: inline

***

## Leptos: Islands Architecture (1)

- Experimental ⚠️
- Use `#[island]` for "client components" \
  (included in WASM binary)
- Use `#[component]` for "server components" \
  (only run on the server)
- Pass props, children, context between islands

***

## Leptos: Islands Architecture (2)

Advantages:

- Reduce WASM binary size & serialization costs
- Easier use of server-only APIs in server components.

https://book.leptos.dev/islands.html

***

## Further Topics

- Error handling with `<ErrorBoundary>`
- Testing
- Client-side form validation
- JavaScript interop
- Deployment
- ...

-*-*-

<div class="nr"></div>

# Closing

<!-- .slide: class="master-title" -->

***

## Resources

- [A half-hour to learn Rust](https://fasterthanli.me/articles/a-half-hour-to-learn-rust)
- [Leptos book](https://book.leptos.dev/)
- [Leptos API documentation](https://docs.rs/leptos/)
- [Official Leptos examples](https://github.com/leptos-rs/leptos/tree/main/examples)
- [Awesome Leptos collection](https://github.com/leptos-rs/awesome-leptos)
- Workshop: [Slides](https://puzzle.github.io/tws24-fullstack-rust-slides/), [examples](https://github.com/puzzle/tws24-fullstack-rust-examples)

***

## Hands-On Ideas

- Play with interactive examples in [Leptos book](https://book.leptos.dev/)
- Study [SSR Todo example](https://github.com/puzzle/tws24-fullstack-rust-examples/blob/main/todo_app_sqlite_axum/)
    - Implement edit
    - Implement done state
- Study [official examples](https://github.com/leptos-rs/leptos/tree/main/examples)
- ...

***

## Credits

Authors: \
Daniel Tschan, Mathis Hofer \
Puzzle ITC

Licensed under the terms of the GNU GPL-3.0 license.
