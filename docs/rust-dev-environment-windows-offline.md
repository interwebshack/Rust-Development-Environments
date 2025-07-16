# 🦀 Rust Development Environment Setup (Windows, Offline)

This guide walks through setting up a complete **Rust development environment** on a **Windows workstation** that does **not have internet access**.

## 📦 Overview

You will prepare the following components **on an internet-connected machine**, then **transfer them via USB drive** to the disconnected workstation:

- Rust toolchain (stable)
- Cargo packages (pre-downloaded)
- VSCode + Rust extension (offline VSIX)
- Build tools (C++ compiler)
- Optional: `rust-analyzer`, documentation

---

## ✅ Prerequisites

### 🖥️ On Target Workstation (Disconnected)

- Windows 10 or 11
- Administrator privileges (optional but helpful)
- USB port for transfer

### 💻 On Source Machine (Internet Connected)

- A separate Windows/Linux/Mac machine
- Internet access
- USB drive with at least 10 GB free

---

## 🔁 Step 1: Prepare Rust Toolchain

### On Connected Machine:

1. Download the Windows Rust installer:

```bash
https://static.rust-lang.org/dist/rust-1.77.2-x86_64-pc-windows-msvc.msi

```

> Replace version with latest stable, if needed.  

1. Download associated `cargo`, `rustc`, and `rust-docs` tarballs:  

```bash
https://static.rust-lang.org/dist/cargo-1.77.2-x86_64-pc-windows-msvc.tar.gz
https://static.rust-lang.org/dist/rustc-1.77.2-x86_64-pc-windows-msvc.tar.gz
https://static.rust-lang.org/dist/rust-docs-1.77.2-x86_64-pc-windows-msvc.tar.gz

```

1. Download `rustup-init.exe` **if you want to use rustup offline:**  

```bash
https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe

```

1. Transfer all files to USB.  

---

## 🧰 Step 2: Install Rust Offline  

**On Target Workstation:**

1. Run the MSI installer:  

```powershell
rust-1.77.2-x86_64-pc-windows-msvc.msi

```

> You can also run `rustup-init.exe` with the following:

```powershell
.\rustup-init.exe --default-toolchain 1.77.2 --no-update-default

```

1. Confirm installation:

```powershell
rustc --version
cargo --version

```

---

## 🧱 Step 3: Install C++ Build Tools (MSVC)  

1. On internet-connected machine, download:  

- **Build Tools for Visual Studio 2022:**  

```powershell
https://aka.ms/vs/17/release/vs_BuildTools.exe

```

1. On the connected machine, run:  

```powershell
.\vs_BuildTools.exe --layout .\offline_vs_buildtools --lang en-US --add Microsoft.VisualStudio.Component.VC.Tools.x86.x64

```

> This creates an **offline installer** directory.  

1. Transfer `offline_vs_buildtools` folder to target.

2. On the disconnected workstation:  

```powershell
.\offline_vs_buildtools\vs_BuildTools.exe --noweb --quiet --wait

```

---

## 🧩 Step 4: Set Up VSCode Offline  

1. **Download VSCode Installer**  
From connected machine:  
```shell
https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user

```

2. **Download Rust Extension VSIX**
From marketplace:  
```shell
https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer

```
To get `.vsix`, append `/Download` to the link:
```shell
https://marketplace.visualstudio.com/_apis/public/gallery/publishers/rust-lang/vsextensions/rust-analyzer/0.4.200401/download

```
Save as `rust-analyzer.vsix`.

3. **Transfer and Install on Target**
On disconnected workstation:

```shell
code --install-extension rust-analyzer.vsix

```
> You must install VSCode first via `.exe`.

---

## 📦 Step 5: Pre-Download Cargo Crates (Optional)

If you know the crates you'll use:

1. On connected machine, create a temp project:
```shell
cargo new offline-project
cd offline-project

```
2. Add your crates to `Cargo.toml`.
3. Run:
```shell
cargo fetch --locked

```
4. Zip the `.cargo` and `target` folder, transfer to workstation.
5. On disconnected system:
  * Set `CARGO_HOME` to unpacked `.cargo`
  * Use `cargo build` as usual

---

## 🧠 Step 6: Enable Local Documentation (Optional)  

Download:

```shell
https://doc.rust-lang.org/stable/rust-docs.zip

```

Unzip to a folder and create a shortcut to `index.html`.

---

## 🔍 Step 7: Test

Run:

```shell
cargo new testproj
cd testproj
cargo build

```
You should see:
```shell
Compiling testproj v0.1.0
Finished dev [unoptimized + debuginfo] target(s) in X.XXs

```
---

## 🧪 Step 8: Install Linter and Security Tools (Offline)

You can install Rust-based linting and SAST (Static Application Security Testing) tools using Cargo — **even offline** if you pre-fetch them.

### Tools to Include:

| Tool           | Description                                  |
|----------------|----------------------------------------------|
| `clippy`       | Lints your Rust code for correctness, idioms |
| `cargo-audit`  | Checks `Cargo.lock` against known CVEs       |
| `cargo-deny`   | Audits licenses, duplicates, advisories      |

---

### 📥 Step 8.1: Download and Package (Connected Machine)

Create a `tools-fetch` project to download tools and dependencies:

```bash
cargo new tools-fetch
cd tools-fetch

```

Edit `Cargo.toml:`

```shell
[dependencies]

[package]
name = "tools-fetch"
version = "0.1.0"
edition = "2021"

[workspace]
members = []

[profile.dev]
panic = "abort"

[workspace.metadata]
cargo-audit = { git = "https://github.com/rustsec/rustsec", package = "cargo-audit" }

```

Now run:  

```shell
rustup component add clippy
cargo install --locked --root ./offline-tools cargo-audit
cargo install --locked --root ./offline-tools cargo-deny

```

Then package:

```shell
tar -czvf rust-tools-offline.tar.gz offline-tools/

```
Copy this archive to your USB.  

---

## 💾 Step 8.2: Install on Target Workstation

1. Extract:

```shell
tar -xvzf rust-tools-offline.tar.gz -C C:\RustTools\

```
2. Add to PATH:

```shell
$env:PATH += ";C:\RustTools\bin"

```
You can now run:
```shell
cargo clippy
cargo audit
cargo deny

```
> `cargo-audit` needs the advisory DB. You can mirror it:

```shell
git clone https://github.com/RustSec/advisory-db.git

```
Place it in a known directory and set:

```shell
$env:RUSTSEC_ADVISORY_DB_PATH="C:\Path\to\advisory-db"

```
---

## 🧪 Step 9: Example Project + VSCode Debug + Lint Test

---
## ✅ Summary Checklist

| Component       | Status        |
| --------------- | ------------- |
| Rust Toolchain  | ✅ Installed |
| Cargo Packages  | ✅ Optional  |
| C++ Build Tools | ✅ Installed |
| VSCode          | ✅ Installed |
| Rust Extension  | ✅ Installed |
| Offline Docs    | ✅ Optional  |

📝 Notes

* You can script the install using PowerShell or batch files.
* Be sure to match architecture (x86_64).
* You can prepare multiple Rust versions by mirroring:

```shell
https://static.rust-lang.org/dist/channel-rust-stable.toml

```

---

📚 References

* [https://forge.rust-lang.org/infra/release-archives.html](https://forge.rust-lang.org/infra/release-archives.html)  
* [https://github.com/rust-lang/rustup](https://github.com/rust-lang/rustup)  
* [https://code.visualstudio.com/docs/editor/offline](https://code.visualstudio.com/docs/editor/offline)  
* [https://docs.rs](https://docs.rs)  

