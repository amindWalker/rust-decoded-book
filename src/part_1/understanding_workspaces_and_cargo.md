# Understanding Workspaces and Cargo

Cargo is Rust’s indispensable build system and package manager. It handles numerous tasks for Rust projects, including code building, dependency management, and library handling.

## Getting Started with Cargo
Most Rust developers rely on Cargo to manage their projects efficiently. As your projects grow in complexity, Cargo simplifies the addition and management of dependencies.

## Verifying Cargo Installation
Ensure Cargo is installed alongside Rust. You can verify its installation by running `cargo --version` in your terminal.

```fish
cargo --version 
cargo 1.77.0
```
## Creating a Project with Cargo
Let’s create a new project using Cargo. Navigate to your project directory and execute:

```fish
cargo new hello_cargo
cd hello_cargo
```

Cargo sets up a directory structure with essential files like `Cargo.toml` and a `src` directory.

## Understanding Cargo Configurations
Open `Cargo.toml` to configure your project. It's formatted in [TOML (configuration file)](https://toml.io/en/), detailing project metadata and dependencies.

## Building and Running with Cargo
Build your project with:

```fish
cargo build
```
Run your executable with:

```fish
./target/debug/hello_cargo
```
Cargo creates a `Cargo.lock` file to track dependencies.
Use `cargo run` to compile and execute your project in one command. Additionally, `cargo check` quickly verifies code without producing an executable.

## Optimizing for Release
For release-ready projects, optimize performance with:

```fish
cargo build --release
```
Cargo proves invaluable as projects evolve. Even simple projects benefit from its streamlined workflow and dependency management.

## Seamless Integration with Existing Projects
To work on existing projects, clone the repository and build with Cargo:

```fish
git clone example.org/someproject
cd someproject
cargo build
```

# Optimizing Rust Development with Cargo Workspaces

Cargo workspaces streamline multi-project development in Rust, offering several advantages:

- **Consolidated Builds:** Workspaces merge all builds into a single `target` folder, simplifying cleanup and organization.
  
- **Shared Dependencies:** Projects within workspaces share downloads and build results, leading to faster compilation and reduced disk space usage.
  
- **Unified Commands:** Many cargo commands support the `--all` flag in the workspace root, enabling operations across all projects. For instance, `cargo test --all` runs tests for all projects, while `cargo build --all` compiles everything. Caution is advised with `cargo run --all` as it executes every program in the workspace.

## Setting Up Your Workspace

Begin by navigating to your source folder (for example, `/home/user/say_hello`). Open `Cargo.toml` and add a workspace section:

```toml
[workspace]
members = []
```

The workspace automatically includes the top-level `src` directory, serving as the parent workspace. To avoid confusion, customize the message printed when running the parent project:

```rust
fn main() {
    println!("Choose one of your projects within workspaces.");
}
```

To add a new project (`say_hello`) to the workspace:

```fish
cd /home/user/say_hello
cargo new say_goodbye
```

Ensure to update the parent's `Cargo.toml` to include the new project:

```toml
members = [
    "say_goodbye", # Add some description.
]
```

**Try It Out**

Navigate to the workspace root and run `cargo run`. You'll receive a message indicating that you probably didn't intend to run this project. Switch to the `say_goodbye` program and run `cargo run` to see "Hello World." Notice that all compilation artifacts reside in a single `target` folder.

**To find out all configurations supported by Cargo Workspaces read the [Cargo Boook - Workspaces](https://doc.rust-lang.org/cargo/reference/workspaces.html).**
