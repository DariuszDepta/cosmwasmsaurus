---
sidebar_position: 1
---

# Installation

## Setting up the environment

Before diving right into writing code, you need to install some tooling in order to compile your contract.
CosmWasm is luckily rather self-contained and therefore needs little external tooling to compile.
Our only external dependency is Rust, which you need to install for your platform.

:::info Installing Rust

We recommend installing Rust using the official [rustup installer]. This makes it easy to stay on
the most recent Rust version and to install compiler targets.

:::

:::tip Optimizing compiler

For production builds you probably also want to install [Docker], too.
This is because we offer the [CosmWasm Optimizing Compiler], which uses
a Docker image to build the smallest contract possible in a deterministic fashion.

<details>
  <summary>Additional information about the **Optimizing Compiler**</summary>
  
  Please note that this image is intended for reproducible production builds.
  It is _not_ optimized for development or in general environments where you
  want to iterate quickly. The builder is optimizing for size, not compilation speed.
  If you want to slim down your contract for development, you can do so by
  tweaking your Cargo profile.
    
  ```toml
  [profile.dev]
  lto = "thin"
  strip = true
  ```
    
  If you want to build with native tools, you might miss out on determinism, but you can still build your
  contract into a small size like so:
    
  ```shell
  RUSTFLAGS="-C link-arg=-s" cargo build --release --lib --target=wasm32-unknown-unknown
  wasm-opt -Os --signext-lowering "target/wasm32-unknown-unknown/release/my-contract.wasm" -o "artifacts/my-contract.wasm"
  ```
    
  (Note: Replace `my-contract` with the name of your contract. You also need `wasm-opt` installed,
  which is part of the [binaryen] project.)  
</details>

:::

After installing Rust, you need to add the WebAssembly target.
This is needed so Rust knows how to build your code to WebAssembly.

To install the WebAssembly target using **rustup**, run the following command:

```shell
rustup target add wasm32-unknown-unknown
```

Now that we set up the foundation we just need two more tools:

1. **cargo-generate**
2. **cargo-run-script** (this is required to later optimize your contract for production)

To install those, run the following commands:

```shell
cargo install cargo-generate --features vendored-openssl
cargo install cargo-run-script
```

## Setting up the contract

Now that the environment is all done, let's create the project!

Luckily you don't need to start from scratch, we already took care of the most tedious parts of
setting up a new project in form of a template!

In order to generate a fresh project, run this command and off we go:

:::tip    
Make sure to change `PROJECT_NAME` to the name of your contract!
:::

```shell
$ cargo generate --git https://github.com/CosmWasm/cw-template.git --name PROJECT_NAME
```

Now you should have a ready contract project in a new folder called `PROJECT_NAME` (or whatever you changed it to).

[rustup installer]: https://rustup.rs
[Docker]: https://www.docker.com/
[CosmWasm Optimizing Compiler]: https://github.com/CosmWasm/optimizer
[binaryen]: https://github.com/WebAssembly/binaryen
