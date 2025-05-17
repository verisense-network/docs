# Quick Start Guide: Developing Nucleus on Verisense

This guide will help you quickly get started with developing your first Nucleus using Rust. By the end, you’ll have a simple deployed Nucleus and be able to interact with it.
## Preparing Environment

First, install Rust and configure it for WebAssembly (Wasm) compilation:

### Install Rust:
You can visit the [Rust installation page](https://www.rust-lang.org/tools/install) or run the following bash command:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### Add the WebAssembly target:
A Nucleus is developed in Rust and compiled into a WebAssembly (Wasm) `.wasm` executable. In Verisense, the core logic of a Nucleus is hosted and executed on the network in the form of this Wasm file.

WebAssembly (Wasm) is a modern binary instruction format designed to bring high-performance code execution to the web and other environments. It is portable, compact, and secure, allowing code written in multiple languages (such as Rust or C/C++) to run efficiently across various platforms.

To enable Rust to generate Wasm target files, you must add the `wasm32-unknown-unknown` compilation target to your Rust toolchain. The steps below show how to add this target.

```bash
rustup target add wasm32-unknown-unknown
```

### Verify Installation

After installation, run the following commands to ensure Rust and the Wasm target are set up correctly:

```bash
rustc --version
cargo --version
rustup target list --installed
```

You should see output similar to:

- Version numbers for `rustc` and `cargo`
- `wasm32-unknown-unknown` listed among the installed targets

If all commands produce the expected output, your environment is ready for development.

Next, in chapter [Creating a Simple Nucleus](./simple.md), you'll learn how to build a simple Nucleus program. In the following sections, we'll walk through the steps to implement, compile, and deploy your first Nucleus instance.



