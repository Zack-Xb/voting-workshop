# Setting up the environment for development on Aleo Netwrok

## Setup for Linux and MacOS

## Check prerequisites
```
git --version
cargo --version
```

## Install Git and Rust

- Install [Git](https://git-scm.com/downloads)
- Install [Rust](https://www.rust-lang.org/tools/install)

Note: After installation, if your `git` and `rustc` command doesn't work, try to close the current terminal window, open a new one, and try again.

## Install leo

Clone the `leo` repository

```bash
# Download the source code
git clone https://github.com/ProvableHQ/leo
cd leo
git checkout testnet-beta
```

Build and install `leo` CLI

```bash
# Build and install
cargo install --path .
```

That will generate the executable ~/.cargo/bin/leo.

Now to use Leo, in your terminal, run:

    leo
For more details about how to use `leo` Cli, check out [this link](https://developer.aleo.org/leo/commands)

## Install `leo` IDE Syntax Highlighting:

Check out Guide [Here](https://developer.aleo.org/leo/installation#3-ide-syntax-highlighting)


## Windows Installation for Rust

### Rust Installation
Rust runs on many platforms, and there are many ways to install Rust. This guide describes installation via rustup, a tool that manages multiple Rust toolchains in a consistent way across all platforms Rust supports.
- On Windows, download and run [rustup-init.exe](https://static.rust-lang.org/rustup/dist/i686-pc-windows-gnu/rustup-init.exe)
- rustup-init can be configured interactively, and all options can additionally be controlled by command-line arguments, which can be passed through the shell script. Pass --help to rustup-init as follows to display the arguments rustup-init accepts:
```
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- --help
```
- The above command needs to executed using WSL
- If you prefer not to use the shell script, you may directly download rustup-init for windows [here](https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe)
- verify rust installation by runnin ```rustc --version``` in wsl


