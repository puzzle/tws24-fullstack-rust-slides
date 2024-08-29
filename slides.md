<!-- .slide: class="master-cover" -->

## Rust Fullstack Development

<br/>

### Tech Workshop<br>5.9.2024
### Dani Tschan<br>Mathis Hofer
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

## Rust Syntax: String Types

* `String`:
  * A growable, heap-allocated string.
  * Used when you need to own and modify string data.
  * Can be converted from and to slices (&str).
  * Commonly seen in data structures.

* `&str` ("stir", "String Slice"):
  * A non-owning reference to a sequence of UTF-8 encoded string data.
  * Used for reading string data without taking ownership.
  * Commonly seen as function parameters.
  * String literals have type `&str`

* String: Heap-allocated string, guaranteed UTF-8
* &str: Slice of a string owned by other, guaranteed UTF-8
  * String literals have type `&str`
* Data structures typically own Strings and use `String`
* Functions typically accept String references `&str`

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

## Rust Syntax: Ownership and Moves


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


***

## (Rust Syntax: Pattern Matching)



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
***
## Rust Syntax: Result
***

## Rust Syntax: Error Handling

***

## Rust Syntax: Iterators

***

## Rust Syntax: Closures

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

## (Rust Syntax: Lifetimes)

***

https://fasterthanli.me/articles/a-half-hour-to-learn-rust

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
  → No need for REST or other API
* Fine-grained reactive, no virtual DOM \
  → Inspired by Solid (JS) and Sycamore (Rust)

***

## Leptos (2)

* Based on Web standards
  * Router based on links and forms
* JSX-like template format: RSX
* Signals have value semantics
  * No fights with the borrow checker
* [Performance](https://www.youtube.com/watch?v=4KtotxNAwME) like fastest JavaScript frameworks
  * Even though WebAssembly has no direct access to DOM

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

***

## Installation

Install Leptos support:

```sh
cargo install cargo-leptos leptosfmt
```

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
- Optional, with default value
- Generic
- ...

```rust
#[component]
pub fn Hello(name: String) -> impl IntoView {
    view! { <p>"Hi " {name} "!"</p> }
}
```

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
        view! { <p>Yeah!</p> }
    } else {
        view! { <p>Nope!</p> }
    }
}

// bool::then()
{move || (count() > 5).then(|| view! { <p>Yeah!</p> })}

// Pattern matching
{move ||
    match count() {
        5 => view! { <p>Yeah!</p> },
        n if n % 2 != 0 => view! { <p>Huh?</p> },
        _ => view! { <p>Nope!</p> }
    }
}
```

note:

Variante optional else:

```
{move ||
    if count() > 5 {
        Some( view! {<p>Yeah!</p>})
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

### Leptos: Async Primitives

* Resource: \
  Reactive (signal-based) async task (data fetching)
* Action: \
  imperative async task (data mutation)

***

### Leptos: Async Streaming Modes

* `<Suspense/>`: \
  Shows children if resource ready, fallback otherwise
* `<Await/>`: \
  Shows children if future ready, nothing otherwise
* `<Transition/>`: \
  Shows children if resource ready, previous data or fallback otherwise

***

### Leptos: Server Functions & Hydration

TODO: dani?

- Fetching Data on the Server: Making HTTP requests or database queries during SSR.
- Server Functions
- Hydration: How to pass data from SSR to the client for further processing.

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

- Experimental
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
- Workshop: [Slides](https://github.com/puzzle/tws24-fullstack-rust-slides), [examples](https://github.com/puzzle/tws24-fullstack-rust-examples)

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
Dani Tschan, Mathis Hofer \
Puzzle ITC

Licensed under the terms of the GNU GPL-3.0 license.