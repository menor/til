# Rust

## Rust compared to Node

### Rustup
- Rustup is the rust equivalent to nvm

- Rustup manages your Rust installation as well as additonal targets (like WebAssembly) and core tools like `cargo` (Rust’s npm), `clippy` (Rust’s eslint), `rustfmt` (Rust’s prettier).

- Specifying your toolchain with rustup is easy enough. As you get deeper, you may get into configurations where different projects require different toolchains or Rust versions. That’s where `rust-toolchain.toml` comes into play. Specify your project’s required toolchain, targets, and supporting tools here so that cargo and rustup can work automatically.

### Cargo
- `cargo` is Rust’s package manager and operates similarly to npm from node’s universe. Cargo downloads dependiencs from  [crates.io](https://crates.io/)  by default.

#### npm to cargo mapping
- In node.js you have `package.json`. In Rust you have `Cargo.toml`.
- In node.js it’s `npm init`. In Rust you have `cargo init` and `cargo new`
  - `cargo init` will initialize the current directory.
  - `cargo new` initializes projects in a new directory.

#### Installing dependencies
- In node.js it’s `npm install`. In Rust you can use `cargo add` if you install  [cargo-edit](https://github.com/killercup/cargo-edit)first. Note: **not** cargo-add, just in case you come across it.
`$ cargo install cargo-edit`

This gives you four new commands: `add`, `rm`, `upgrade`, and `set-version`
