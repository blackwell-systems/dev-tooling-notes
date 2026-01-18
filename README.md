# Blackwell Dev Tools

My personal, evolving list of development tools I use across **macOS**, **Linux (Lima/Ubuntu)**, and **WSL2**.
This is a living reference for future-me and anyone who wants a practical, modern, multi-machine setup.

## Table of Contents

- [Philosophy](#philosophy)
- [At-a-glance Stack](#at-a-glance-stack)
- [Editors & IDEs](#editors--ides)
- [Terminal & Shell](#terminal--shell)
- [Documentation & Diagrams](#documentation--diagrams)
  - [Tools](#tools)
  - [Philosophy](#philosophy-1)
  - [Syntax Highlighting](#syntax-highlighting-in-code-blocks)
  - [README as Landing Page](#readme-as-landing-page)
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

## Editors & IDEs

### Heavy-Weight IDEs (JetBrains Ecosystem)

For full-featured, modern GUI IDE work, I'm a big fan of the **JetBrains ecosystem**:

- **PyCharm** — Python (Professional or Community)
- **GoLand** — Go development
- **RustRover** — Rust development
- **IntelliJ IDEA** — Java/Kotlin (Ultimate or Community)
- **WebStorm** — JavaScript/TypeScript
- **DataGrip** — Database client (all SQL dialects + NoSQL)

**Why JetBrains:**
- **Deep language integration:** Not just LSP - native language analysis, refactoring, debugging
- **Powerful refactoring:** Rename, extract method, inline, move - all work reliably across languages
- **Built-in tools:** Database clients, HTTP client, terminal, Git, Docker - everything integrated
- **Consistent UX:** Learn once, apply to all languages (PyCharm to GoLand feels natural)
- **Intelligent completion:** Context-aware suggestions that understand frameworks (Django, Spring, etc.)
- **Debugger excellence:** Best-in-class debugging experience with evaluate expression, conditional breakpoints
- **Search everything:** Double-shift to search files, classes, symbols, actions - incredibly fast navigation

**Trade-offs:**
- Heavy resource usage (1-2GB RAM per IDE instance)
- Expensive (but worth it for professional work - All Products Pack recommended)
- Slower startup than lightweight editors
- Can feel sluggish on older machines

**When I use JetBrains:**
- Large codebases (10k+ lines)
- Complex refactoring tasks
- Learning new frameworks (smart completion teaches you the API)
- Debugging complex issues (breakpoint conditions, expression evaluation)
- Working with databases (DataGrip built into all IDEs)
- Production work where mistakes are expensive

### VS Code

**I can work with VS Code, but I've never felt really at home or comfortable with it.**

**What VS Code does well:**
- Fast startup
- Huge extension ecosystem
- Good remote development (SSH, WSL, containers)
- Lower resource usage than JetBrains
- Free and open source
- Cross-platform consistency

**Why I don't prefer it:**
- Extension quality varies wildly (some excellent, many mediocre)
- Configuration complexity (need to find and install right extensions)
- Refactoring not as robust as JetBrains
- TypeScript/JavaScript focus - other languages feel second-class
- Electron-based (feels less native than JetBrains)

**When VS Code makes sense:**
- Resource-constrained machines
- Quick edits (faster startup)
- Remote development workflows
- Team standardized on VS Code
- Budget is a concern (free vs JetBrains subscription)

### Terminal Editors (Helix)

**I've recently begun getting into Helix and have been making a significant effort to use that on a daily basis.**

**Why Helix:**
- **Zero configuration:** Works out-of-box with LSP
- **Selection-first:** See what you're changing before you change it
- **Fast:** No Electron, no GUI overhead
- **Modern:** Tree-sitter, LSP, multiple selections built-in
- **Terminal-native:** SSH, remote servers, lightweight environments
- **No plugins needed:** Batteries included

**Current workflow:**
- Small projects and quick edits
- Server-side work (SSH sessions)
- When I want distraction-free editing
- Learning phase (building muscle memory)

**Helix trade-offs:**
- Modal editing learning curve (2-3 weeks to comfortable)
- Terminal-only (no GUI affordances)
- Less powerful refactoring than JetBrains
- Limited debugging compared to IDEs

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

---

## Documentation & Diagrams

**Tools:**
- **glow** — Markdown rendering in terminal
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

- **docsify** — Zero-config documentation site generator. Converts markdown to beautiful, searchable sites without build step. Used across my projects (dotclaude, vaultmux, gcp-secret-manager-emulator) with custom Blackwell theme at [blackwell-docs-theme](https://github.com/blackwell-systems/blackwell-docs-theme)

### Philosophy

Documentation is a first-class citizen in my development process. Outdated documentation is worse than no documentation—at least with no docs, you're encouraged to find answers yourself rather than rely on false confidence in something that isn't true.

**Syntax highlighting in code blocks:**

GitHub (and most markdown renderers) support syntax highlighting via the info string after opening triple backticks. Most renderers pass this into a syntax highlighter and colorize appropriately—adding language hints dramatically improves documentation readability.

```markdown
```rust
fn main() {
    println!("Hello, world!");
}
` ``

```python
def main():
    print("Hello, world!")
` ``

```bash
echo "Hello, world!"
` ``
```

**Common language identifiers:**
- `rust`, `go`, `python`, `javascript`, `typescript`, `bash`, `sh`, `zsh`
- `json`, `yaml`, `toml`, `xml`, `html`, `css`
- `sql`, `dockerfile`, `makefile`, `nginx`
- `diff`, `patch`, `log`, `plaintext`

**Why this matters:**
- Syntax highlighting makes code blocks 10x more readable
- It signals to readers what language/format they're looking at
- It catches basic syntax errors visually (wrong quotes, missing brackets)
- It's zero-effort once you remember to add the language identifier
- Good documentation accelerates onboarding and reduces support burden

**Documentation hygiene:**
- Keep docs in sync with code (treat as part of the same PR)
- Use code blocks with language hints for all examples
- Include "Why" and "When" sections, not just "How"
- Add runnable examples that can be copy-pasted
- Delete or update outdated docs immediately—don't let them linger

**README as landing page:**

For GitHub/crates.io/PyPI repositories, treat the README as a landing page that sells the idea quickly and points to real documentation. Documentation sprawl is real and needs to be kept in check.

One of the best ways to combat sprawl: treat your main README like a thin landing page and navigation hub with short, powerful descriptions of features which then link out to the real documentation.

**README structure:**
- **Top section** - One-sentence value proposition, badges, quick example
- **Quick Start** - Minimal working example (5-10 lines)
- **Features** - Bullet list with short descriptions + links to detailed docs
- **Installation** - One command, link to detailed setup guide
- **Documentation** - Links to API docs, guides, examples
- **Contributing** - Link to CONTRIBUTING.md

**Keep READMEs short:**
- Aim for 200-400 lines max
- If README exceeds 500 lines, it's doing too much
- Move detailed content to docs/ directory (guides, architecture, API reference)
- Use ARCHITECTURE.md for visual/diagram-heavy explanations
- Use separate files for migration guides, comparisons, tutorials

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
- **ipython** — enhanced interactive shell with tab completion, magic commands, object introspection

Interactive Development:
- **IPython** is the standard enhanced REPL
  - Tab completion with context-aware suggestions
  - Object introspection: `object?` for docstring, `object??` for source code
  - Magic commands: `%timeit` (benchmarking), `%run` (execute scripts), `%debug` (post-mortem debugging)
  - Multi-line editing and syntax highlighting
  - Install: `uv tool install ipython` or `pip install ipython`
- Alternative REPLs: `bpython` (inline suggestions), `ptpython` (dropdown menus)

Notes:
- FastAPI + pydantic is my preferred Python API stack: automatic validation, serialization, and OpenAPI docs from type hints. Feels closer to strongly-typed languages while staying Pythonic.
- Mangum lets you deploy the same FastAPI app to Lambda without code changes—just wrap the ASGI app.
- IPython is essential for API exploration, quick testing, and debugging—use it instead of standard `python` REPL for day-to-day work.

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
