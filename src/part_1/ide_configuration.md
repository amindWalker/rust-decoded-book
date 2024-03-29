# IDE Configuration for Rust Development

Congratulations on installing Rust! Now let's dive into writing your inaugural Rust program.

### Writing "Hello, World!"

It's customary in programming to start with a simple "Hello, world!" program. We'll follow suit here.

To begin, create a directory to house your Rust code. While Rust isn't picky about where your code resides, for this book, it is recommended a *project* directory in your home folder.

#### Setting Up Your Project Directory

In your terminal, execute the following commands:

```bash
mkdir ~/projects
cd ~/projects
mkdir hello_world
cd hello_world
```

### Crafting and Executing Your Program

Now, let's create our first Rust file. Create a new source file named `main.rs` within the *hello_world* directory. Remember, Rust files always carry the `.rs` extension.

Here's the code for `main.rs`:

```rust
fn main() {
    println!("Hello, world!");
}
```

After saving `main.rs`, return to your terminal in the `~/projects/hello_world` directory. On Linux or macOS, compile and run the file using:

```bash
rustc main.rs
./main
```

For Windows, utilize:

```powershell
rustc main.rs
.\main.exe
```

You should see `Hello, world!` printed to your terminal. If not, refer to the troubleshooting section in the installation part for assistance.

### Understanding Your Rust Program

Now, let's dissect the "Hello, world!" program:

- **`fn main() { }`**: This declares the `main` function, the entry point for every executable Rust program. The function has no parameters and returns nothing.

- **`println!("Hello, world!");`**: This line prints "Hello, world!" to the screen. Rust style dictates using four spaces for indentation.

### Compiling and Running

Compiling and running in Rust are separate steps. After compiling with `rustc`, you obtain an executable file, which can be shared independently, unlike dynamic languages like JavaScript, Ruby or Python.

For instance, after compiling, run the executable with:

```bash
./main # or .\main.exe on Windows
```

As your projects grow, you'll benefit from managing them with Cargo, Rust's package manager. We'll delve into that in the next sections.
