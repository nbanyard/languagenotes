# Command Line Tools

## Cargo

### Project Management

Create a new binary (program) project. Created with one binary crate, more can be added.
```shell
cargo new my-new-program
```

Create a new library crate. See [[Packages, Crates and Modules#Packages]] for binary and library crate detail.
```shell
cargo new --lib my-new-library
```

Project configuration is in `Cargo.toml`
```toml
[package]
name = "project_name"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

### Building and running projects

```shell
cargo check  # Checks code but does not build

cargo build  # Executable in ./target/debug/<program>

cargo run   # Checks, builds and runs

cargo build --release  # Executable in ./target/release/<program>
```

### Dependency management

```shell
cargo update  # Update dependencies in Cargo.lock

cargo search [options] [query...]  # Search crates.io
```

## Toolchain Installer

Bootstrap
```shell
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Update rustup and all rust all toolchains (channels, versions, platforms).
```shell
rustup update
```

Update only rustup
```shell
rustup self update
```

## Cross compiling

Machine setup (Pi Zero):
```shell
brew install arm-linux-gnueabihf-binutils
rustup target add arm-unknown-linux-gnueabihf
```

Project setup:
```tomp
[build]
target = "arm-unknown-linux-gnueabihf" # Optionally change default target

[target.arm-unknown-linux-gnueabihf]
linker = "arm-linux-gnueabihf-ld"
```

Build:
```shell
cargo build --target=arm-unknown-linux-gnueabihf