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
