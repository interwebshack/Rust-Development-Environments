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
