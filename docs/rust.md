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
- In node.js it’s `npm install`. In Rust you can use `cargo add` if you install [cargo-edit](https://github.com/killercup/cargo-edit)first. Note: **not** cargo-add, just in case you come across it.
`$ cargo install cargo-edit`

This gives you four new commands: `add`, `rm`, `upgrade`, and `set-version`

#### Installing tools globally
- In node.js it’s `npm install —global`. In Rust you have `cargo install`.
- If you installed rust via rustup then these are placed in a local user directory (usually `~/.cargo/bin`).

#### Running tests
- In node.js it’s `npm test`. In Rust you have `cargo test`.


### Publishing modules
- In node.js it’s `npm publish`. In Rust you have `cargo publish`. You’ll need to have an account on  [crates.io](https://crates.io/)

### Running tasks
- In node.js you might use `npm run start` to run your server or executable. In Rust you would use `cargo run`. You can even use `cargo run —example xxx` to automatically run example code.

- `cargo bench` to profile your code.
- `cargo build` to build
- `cargo clean` will wipe away your build folder (target, by default).
- `cargo doc` to generate documentation.

- For code generation or pre-build steps, cargo supports  [build scripts](https://doc.rust-lang.org/cargo/reference/build-scripts.html)  which run before the main build.

- npm’s built-in task runner is one of the reasons why you rarely see Makefiles in JavaScript projects. In the Rust ecosystem, you’re not as lucky. Makefiles are still common but  [just](https://github.com/casey/just)  is an attractive option that is gaining adoption. It irons out a lot of the wonkiness of Makefiles while keeping a similar syntax. Install just via `cargo install just`
-  If you do go the Makefile route, check out  [isaacs’s tutorial](https://gist.github.com/isaacs/62a2d1825d04437c6f08)  and read  [Your makefiles are wrong](https://tech.davis-hansson.com/p/make/).

### Workspaces and Monorepos
- Both package managers use a workspace concept to help you work with multiple small modules in a large project. In Rust, you create a Cargo.toml file in the root directory with a `[workspace]` entry that describes what’s included and excluded in the workspace. It could be as simple as:
```toml
[workspace]
members = [
  "crates/*"
]
```
- Workspace members that depend on each other can then just point to the local directory as their dependency, e.g.
```toml
[dependencies]
other-project = { path = "../other-project" }
```

### Other Tools
- cargo-edit adds `cargo add` and `cargo rm` and other commands to help manage dependencies from the command line. (`cargo install cargo-edit`)
- cargo-workspaces simplifies creating and managing workspaces and their members. It was inspired by node’s lerna and picks up where cargo leaves off. One of its most valuable features is automating the publish of a workspace’s members, replacing local dependencies with the published versions. (`cargo install cargo-workspaces`)
- Macros are great at hand waving away code you don’t want to write repeatedly but they can make code hard to follow and troubleshoot. `cargo expand` helps pull back the curtain.
- cargo-expand needs a nightly toolchain installed which you can get by running `rustup install nightly`
- Then install cargo-expand via `cargo install cargo-expand`
- Once installed, you can run `cargo expand [item]` to print out the fully generated source that rustc compiles.
- `tomlq` While not a cargo xxx command, it’s useful for querying data in .toml files like Cargo.toml. It’s a less featureful sibling to the amazing jq.
