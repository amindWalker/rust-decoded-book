# Setting Up Rust

> [!TIP]
> ### The most recommended way of installing Rust is by using `rustup`:
> - On Unix Systems (**Linux** or **MacOS**), run the following in your terminal: <br>
>   ```bash
>   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
>   ```
>   then:
>   ```bash
>   rustup default stable
>   ```
> - If you are running **Windows 64-bit**, download and run: [rustup‑init.exe](https://win.rustup.rs/x86_64) then follow the onscreen instructions.
> - If you are running **Windows 32-bit**, download and run: [rustup‑init.exe](https://win.rustup.rs/i686) then follow the onscreen instructions.

There is also alternative ways to install Rust on your device if you don't want to follow the recommended `rustup` way. Go to [Alternative Installation](https://forge.rust-lang.org/infra/other-installation-methods.html)

## Additional Toolchains and targets

Rust includes cross-compilation and many other targets than your default system.

For example, you can add WASM by running in terminal:
```bash
rustup target add wasm32-unknown-unknown
```
## Rust Playground
If you just want to try Rust without installing it on your device, you can. Go to the [Rust Playground](https://play.rust-lang.org)

## Troubleshooting
If you get linker errors, you should install a `C` compiler, which will typically include a
linker. It is likely you already have one. A `C` compiler is also useful because some common Rust packages depend on 
`C` code and will need a `C` compiler.

<br>

**On macOS, you can get a C compiler by running:**

```zsh
xcode-select --install
```

Linux users should generally install `GCC` or `Clang`, according to their
distribution’s documentation. For example, if you use Ubuntu, you can install
the `build-essential` package.

<br>

On Windows, go to https://www.rust-lang.org/tools/install and follow
the instructions for installing Rust. At some point in the installation, you’ll
receive a message explaining that you’ll also need the **MSVC** build tools for
**Visual Studio 2013** or later.

To acquire the build tools, you’ll need to install [Visual Studio
2022](https://visualstudio.microsoft.com/downloads/). When asked which workloads to install, include:

- “Desktop Development with C++”
- The Windows 10 or 11 SDK
- The English language pack component, along with any other language pack of
your choosing

The rest of this book uses commands that work in both `cmd.exe` and `PowerShell`.
