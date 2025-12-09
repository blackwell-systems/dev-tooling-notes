# Blackwell Dev Tools

My personal, evolving list of development tools I use across **macOS**, **Linux (Lima/Ubuntu)**, and **WSL2**.
This is a living reference for future-me and anyone who wants a practical, modern, multi-machine setup.

---

## Philosophy

I keep tools that:
- reduce friction across machines,
- support cloud-first workflows,
- improve terminal ergonomics,
- are stable enough to trust daily,
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

---

## Language Tooling

### Go
Core:
- **Go toolchain** (via Homebrew on macOS)
- **golangci-lint**
- **gofumpt** / **goimports** (if you want stricter formatting)

Notes:
- Installing Go via Homebrew is totally fine for day-to-day dev.
- If you start needing multiple Go versions for different repos, consider adding a version manager workflow later.
  (This repo tracks tools, not policy.)

### Python
I’m standardizing around:
- **uv** — dependency + environment workflow
- **ruff** — lint/format (fast enough to be default)
- **black** — formatter (if/when you want explicit Black parity in projects)
- **hatchling** — builds

Optional but common:
- **pytest**
- **mypy** / **pyright** (when type-checking matters)

### Rust
- **rustup**
- **cargo**
- **clippy**
- **rustfmt**
- **cargo-watch**

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
- **“Fresh machine” setup tests**  
  Run install + setup in a clean container to catch missing dependencies early.

This category pairs naturally with:
- **dotfiles doctor**
- **vaultmux** backend sanity checks
- CLI release pipelines

---

## Databases & API Testing

- **DBeaver** — mostly because it’s the standard at work
- (Add your preferred local DB clients here if you want parity across machines)

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

## Suggested Repo Structure

```text
.
├── README.md
├── Brewfile               # optional
├── tooling/
│   ├── terminal.md
│   ├── go.md
│   ├── python.md
│   ├── rust.md
│   ├── aws.md
│   ├── windows.md
│   ├── containers.md
│   └── cicd.md
└── notes/
    └── decisions.md       # why I chose X over Y
```

Keep the README concise and push deep details into `tooling/*`.

---

## What I’m Evaluating Next

A short list of “maybe soon” items I’ll only add when they earn a permanent spot:

* additional Go version strategy (if multi-version pain becomes real)
* deeper Windows parity for vault + hooks
* more standardized local testing harness around LocalStack + ngrok
* a repeatable dev auth stack (Keycloak + small helper scripts / JWT tooling) for realistic local auth flows

---

## License

MIT (or whatever you choose for your meta-doc repos).

::contentReference[oaicite:0]{index=0}
```
