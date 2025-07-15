# Rust-Development-Environments
How to setup development environments for the Rust programming language.

[![Markdown Lint](https://github.com/interwebshack/Rust-Development-Environments/actions/workflows/markdown-lint.yml/badge.svg)](https://github.com/interwebshack/Rust-Development-Environments/actions/workflows/markdown-lint.yml)  

## 📝 Documentation Linting

We use [PyMarkdown](https://github.com/jackdewinter/pymarkdown) to lint all Markdown files in `docs/`.

## Install the Markdown Lint tool locally (powershell):
```shell
python -m venv .venv
.venv\Scripts\activate.ps1  # On Linux: source .venv/bin/activate
pip install -r requirements.txt

```

## Run Lint Locally
```shell
pymarkdown scan docs

```
For **auto-fix:**  
```shell
pymarkdown --fix scan docs

```
