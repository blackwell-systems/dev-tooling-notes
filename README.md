# Blackwell Dev Tools

My personal, evolving list of development tools I use across **macOS**, **Linux (Lima/Ubuntu)**, and **WSL2**.
This is a living reference for future-me and anyone who wants a practical, modern, multi-machine setup.

## Table of Contents

- [Philosophy](#philosophy)
- [At-a-glance Stack](#at-a-glance-stack)
- [Terminal & Shell](#terminal--shell)
- [Language Tooling](#language-tooling)
  - [Go](#go)
  - [Python](#python)
  - [Java](#java)
  - [Rust](#rust)
  - [Node.js](#nodejs-as-needed)
- [AWS & Infra](#aws--infra)
- [Docker-based Testing](#docker-based-testing)
- [Databases & API Testing](#databases--api-testing)
- [CI/CD](#cicd)
- [Installation (Optional)](#installation-optional)
- [License](#license)

---

## Philosophy

I keep tools that:
- reduce friction across machines,
- support cloud-first workflows,
- improve terminal ergonomics,
- or enable local emulation of cloud/services.

I try to avoid duplicating setup steps that my dotfiles bootstrap already automates.

---

## At-a-glance Stack

### Terminal + workflow
- **Ghostty** — primary terminal on macOS.
- **Zellij** — multiplexer with a modern UX (especially nice across machines).
- **tmux** — still useful for compatibility and remote minimalism.
- **Zsh** — default shell.
- **Powerlevel10k** — prompt theme.

### Containers & local runtime
- **Docker** — baseline for reproducible local dev + CI parity.
- **Alpine** — my go-to minimal image for sanity checks and “does this really work in a tiny Linux?” validation.

### Cloud/dev services
- **ngrok** — expose local services for testing webhooks, mobile clients, etc.
- **LocalStack** — local AWS service emulation.  
  - Docker Hub: https://hub.docker.com/r/localstack/localstack/
- **Keycloak** — local OIDC/identity provider to exercise “real” auth flows in dev (login redirects, JWTs, roles/scopes) without touching production IdPs.

### Languages/toolchains
- **Go**
- **Python** (with uv)
- **Rust**
- Java
- **Node.js** (as needed)
- Bash
- Zsh
- **PowerShell (pwsh)** — cross-platform PowerShell for dev/testing scripts on macOS/Linux with Windows parity.

### CI/CD
- **GitHub Actions** — used across OSS repos.

---

## Terminal & Shell

### Shell UX
- zsh-autosuggestions
- zsh-syntax-highlighting
- zsh-you-should-use

### Shell quality
- **ShellCheck** — static analysis for shell scripts (especially helpful for anything that survives beyond a quick one-off).

### Modern CLI essentials
- **bat** — better `cat`
- **glow** — Markdown rendering in terminal
- **dust** — better `du`
- **eza** — better `ls`
- **ripgrep**
- **fzf**
- **zoxide**

### File management
- **yazi** — fast terminal file manager

### Documentation & Diagrams
- **glow** — Markdown rendering in terminal (already listed above)
- **mermaid-cli** — Render and validate mermaid diagrams
  ```bash
  npm install -g @mermaid-js/mermaid-cli

  # Validate diagram syntax
  mmdc -i diagram.mmd --validate

  # Render to SVG/PNG (catches layout errors GitHub might miss)
  mmdc -i diagram.mmd -o output.svg
  ```

  **Why this matters:**
  - Mermaid diagrams in README.md files render on GitHub, but complex diagrams can fail silently
  - Validating locally catches syntax errors before pushing
  - Rendering locally catches layout complexity issues (nested subgraphs, long chains)
  - GitHub's mermaid version may differ from latest CLI, so test both

  **Common gotchas:**
  - `--validate` only checks syntax, not rendering feasibility
  - Deep nesting (subgraphs within subgraphs) often fails on GitHub
  - Keep diagrams simple: flat structure > nested complexity

---

## Language Tooling

### Go
Core:
- **Go toolchain** (via Homebrew on macOS)
- **golangci-lint**
- **gofumpt** / **goimports** (if you want stricter formatting)
- **GoReleaser** — automated release tool for Go projects
  - Builds binaries for all major OS/arch combinations (Linux, macOS, Windows × x86_64/ARM64)
  - Generates checksums and SBOMs (Software Bill of Materials)
  - Creates GitHub/GitLab releases with release notes
  - Builds Docker images, Homebrew formulas, Snap/APT/RPM packages
  - Handles code signing for macOS/Windows
  - Typical pipeline: Tag a release (v1.2.3) → CI triggers GoReleaser → Binaries, checksums, and packages are built → Assets uploaded to GitHub Releases
  - Eliminates manual cross-compilation setup and platform-specific toolchain configuration
  - Standard practice for distributing Go CLI tools

Notes:
- Installing Go via Homebrew is totally fine for day-to-day dev.
- If you start needing multiple Go versions for different repos, consider adding a version manager workflow later.
  (This repo tracks tools, not policy.)

### Python
I'm standardizing around:
- **uv** — dependency + environment workflow
- **ruff** — lint/format (fast enough to be default, Black-compatible, replaces multiple tools)
- **black** — formatter (if/when you need explicit Black parity, though Ruff's formatter is 10-100x faster)
- **hatchling** — builds
- **pytest**
- **mypy** / **pyright** (when type-checking matters)
- **pydantic** — data validation and serialization
- **fastapi** — modern async web framework
- **mangum** — AWS Lambda adapter for ASGI apps (FastAPI, Starlette)

Notes:
- FastAPI + pydantic is my preferred Python API stack: automatic validation, serialization, and OpenAPI docs from type hints. Feels closer to strongly-typed languages while staying Pythonic.
- Mangum lets you deploy the same FastAPI app to Lambda without code changes—just wrap the ASGI app.

### Java
Core:
- **OpenJDK** (via Homebrew or SDKMAN)
- **Maven** — dependency management and build tool
- **Gradle** — modern build automation

Optional:
- **SDKMAN** — manage multiple JDK versions
- **Spring Boot** — production-ready Java framework
- **Quarkus** — cloud-native Java (fast startup, low memory)

Notes:
- SDKMAN is useful if you work across projects with different Java versions
- Gradle is generally faster than Maven, but Maven has broader adoption

### Rust
- **rustup**
- **cargo**
- **clippy**
- **rustfmt**
- **cargo-watch**
- **cargo-dist** — distribution and release tool for Rust projects. Similar to GoReleaser but Rust-native. Builds binaries for multiple platforms, generates installers (shell scripts, PowerShell, npm packages), creates GitHub releases, and handles checksum generation. Integrates directly with GitHub Actions for automated releases on tag push

Testing:
- **rstest** — parameterized testing framework for consolidating similar test cases. Instead of writing separate test functions for each variant, use `#[rstest]` with `#[case]` attributes to test multiple inputs/outputs in a single test function
- **proptest** — property-based testing library that generates random test inputs to find edge cases. Useful for testing invariants (e.g., "parsing then serializing should return the original input") rather than specific examples
- **insta** — snapshot testing tool that captures output as "snapshots" and detects changes. Perfect for testing serialization formats (JSON, YAML), rendered output (SVG, HTML), or CLI output where you want to catch any unintended changes

Documentation:
- **Doc comments (`///`)** are first-class in Rust and render on [docs.rs](https://docs.rs) when published
- **Crate-level docs (`//!`)** - Use inner doc comments at the top of `src/lib.rs` to document the entire crate
  - First paragraph becomes the crate description on docs.rs
  - Should include: purpose, quick example, feature flags, links to guides
  - Example: `//! A tiny, framework-agnostic error envelope for HTTP APIs.`
- **Check coverage locally before publishing:**
  ```bash
  # Generate and open docs locally (catches rendering issues)
  cargo doc --open --no-deps
  
  # Check documentation coverage with nightly (requires unstable flag)
  RUSTDOCFLAGS="-Z unstable-options --show-coverage" cargo +nightly doc --no-deps
  ```
- **Why this matters:**
  - docs.rs is the public face of your crate—low coverage looks unfinished
  - Checking locally catches missing docs before publishing (no "oops, forgot that pub field" moments)
  - Documentation coverage is basic project hygiene, like test coverage or clippy warnings
  - Many projects aim for 100% coverage on public items (especially libraries meant for external use)
- **What gets counted:**
  - Public modules, structs, enums, traits, functions, methods
  - Public enum variants (often forgotten!)
  - Public struct fields (another common miss)
  - Re-exported items (via `pub use`)
  - Crate-level documentation (the `//!` block in lib.rs)
- **Common gotchas:**
  - Struct-level docs don't count toward field coverage—each `pub field` needs its own `///`
  - Enum-level docs don't cover variants—document each variant separately
  - Private items aren't counted (good—focus on your public API)
  - Module-level docs use `//!` (inner) not `///` (outer)

### Node.js (as needed)
- **nvm** (or a lazy-load integration)

---

## AWS & Infra

- **AWS CLI v2**
- **AWS CDK**
- **LocalStack**
- **ngrok**
- **Docker**

Things I often want available in fresh environments:
- **jq**
- **curl**
- **wget**

---

## Docker-based Testing

I like using containers to prove my tools and scripts are truly portable.

Patterns I reach for:
- **Smoke tests in Alpine**  
  Validate that binaries and shell scripts don’t secretly depend on heavyweight OS assumptions.
- **LocalStack-driven integration tests**  
  Only what I need, with minimal AWS surface area.
- **"Fresh machine" setup tests**
  Run install + setup in a clean container to catch missing dependencies early.

---

## Databases & API Testing

### Database Systems
- **DynamoDB** — AWS NoSQL database
- **MongoDB** — Document database
- **PostgreSQL** — Advanced open source SQL
- **MySQL** — Popular open source SQL
- **SQLite** — Embedded SQL database
- **Amazon Redshift** — Data warehouse for analytics

### Database Clients & Tools
- **DBeaver** — universal database client (supports all SQL variants)
- **TablePlus** — modern database client (macOS/Windows/Linux)
- **psql** — PostgreSQL command-line client
- **mongosh** — MongoDB shell
- **AWS CLI** — for DynamoDB operations
- **NoSQL Workbench** — DynamoDB visual client

---

## CI/CD

### GitHub Actions
This is a first-class part of the stack:
- lint
- test
- release automation
- multi-platform builds for CLI binaries

---

## Installation (Optional)

I usually let my dotfiles framework handle most of this,
but if you want a standalone quick start on macOS:

### Brew basics
```sh
brew install \
  bat glow dust eza ripgrep fzf zoxide \
  zsh-autosuggestions zsh-syntax-highlighting zsh-you-should-use \
  yazi \
  shellcheck \
  go \
  uv ruff black
````

### Docker on macOS

```sh
brew install --cask docker
```

If you prefer a reproducible approach:

* Add these to a **Brewfile** and run:

```sh
brew bundle
```

---

## License

MIT (or whatever you choose for your meta-doc repos).

::contentReference[oaicite:0]{index=0}
```
