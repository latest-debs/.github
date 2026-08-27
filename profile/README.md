<p align="center">
  <img src="https://raw.githubusercontent.com/latest-debs/.github/main/assets/latest-debs-logo.svg" width="128" alt="latest-debs">
</p>

<h1 align="center">latest-debs</h1>

<p align="center">
  <b>Latest stable releases of developer tools, packaged as <code>.deb</code> and served over <code>apt</code>.</b><br>
  Debian freezes package versions for years. We track upstream and publish new releases within hours —<br>
  and we've made the Debian packaging itself fast and easy, so any team can do the same.
</p>

<p align="center">
  <a href="https://latest-debs.github.io/"><img src="https://img.shields.io/badge/website-latest--debs.github.io-1f6feb" alt="website"></a>
  <a href="https://latest-debs.github.io/apt-repo/dists/freshness.json"><img src="https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$._meta" alt="freshness"></a>
  <a href="https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml"><img src="https://img.shields.io/badge/request-a%20package-blueviolet" alt="request a package"></a>
  <a href="https://github.com/orgs/latest-debs/discussions"><img src="https://img.shields.io/badge/chat-org%20discussions-238636" alt="discussions"></a>
  <a href="https://github.com/sponsors/latest-debs"><img src="https://img.shields.io/badge/sponsor-%E2%9D%A4-ea4aaa" alt="sponsor"></a>
</p>

---

## The problem with dev tools on Debian

Debian stable is an excellent base — but its packages freeze at release time
and stay frozen for years. Developer tools move far faster, often releasing
weekly, so the versions shipping in `apt` are routinely months or years
behind upstream. Every common workaround trades away something real:

| Today's workaround | The catch |
|--------------------|-----------|
| Wait for Debian stable | Years behind; missing features and fixes |
| Run Sid, or pull from testing/backports | See below — Sid solves a different problem |
| `curl … \| sh` install scripts | No signature verification, no upgrades, no clean uninstall |
| Ad-hoc third-party repos | Often single-arch, unsigned, silently abandoned |
| `cargo install` / `pipx` / manual builds | Needs a toolchain; no system-wide updates; you become the packager |
| Snap / Flatpak | Extra runtime and sandboxing overhead for a simple CLI |

**"But Debian Sid already has fresh packages — what are you solving?"**
Sid is a full rolling *development branch*: every package on your system
moves, all the time. It's the right base for working on Debian itself, and
the wrong base for a workstation or server you depend on. Installing a
single tool from Sid onto stable usually drags in a newer glibc and half
the toolchain with it. And even Sid trails upstream — maintainer upload
cycles, NEW-queue reviews, and release freezes regularly add weeks of lag.

What we're solving is deliberately narrower: **keep Debian stable as your
OS, and get just your dev tools at upstream speed** — built as proper
packages *for* the stable suites, so nothing else on your system has to
move.

**latest-debs exists to close that gap.** We do proper Debian packaging of
upstream releases in fully public CI and publish to a signed apt repository —
so `apt install` and `apt upgrade` just work, across every architecture each
upstream actually ships for Linux, within hours of each upstream release.
Native packages, native tooling, no compromises.

## The Mission

**Make Debian packaging fast and easy.**

Shipping software for Debian shouldn't require becoming a Debian packager —
yet today it does: `debhelper`, Policy, `control`/`rules` files, multi-arch
builds, signing, repo hosting. A steep, time-consuming detour when you just
want to ship a CLI. Most developers, teams, and orgs skip `.deb` entirely,
and their Debian users pay for it in `curl | sh` scripts and stale copies.

latest-debs removes that cost:

- **One reusable builder** —
  [debian-multiarch-builder](https://github.com/ranjithrajv/debian-multiarch-builder),
  a GitHub Action that turns an upstream release into signed, multi-arch
  `.deb` packages in a single workflow run. No packaging expertise
  required.
- **Distribution included** — releases flow into this signed apt repo
  automatically. Nothing to host, sign, or babysit.
- **Open to your tool** — open a
  [package request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml),
  or add it to
  [`tools.yaml`](https://github.com/latest-debs/apt-repo/blob/main/tools.yaml)
  in a PR. Every future upstream release then ships as a `.deb` within hours.

Everything in this org is built with that same pipeline — the repo is the
demo as well as the product.

## New here? Start in 30 seconds

On Debian or Ubuntu, with [extrepo](https://salsa.debian.org/extrepo-team/extrepo):

```sh
sudo extrepo enable latest-debs
sudo apt update
sudo apt install uv eza lazygit ruff bun deno duckdb lazydocker
```

Or add the repository manually:

```sh
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://raw.githubusercontent.com/latest-debs/apt-repo/main/latest-debs.asc \
  | sudo gpg --dearmor --yes -o /etc/apt/keyrings/latest-debs.gpg
echo "deb [signed-by=/etc/apt/keyrings/latest-debs.gpg] https://latest-debs.github.io/apt-repo/ $(lsb_release -sc) main" \
  | sudo tee /etc/apt/sources.list.d/latest-debs.list
sudo apt update
```

> **Status: early days, but working.** All four suites (Bookworm, Trixie,
> Forky, Sid) are live and signed today. The catalog grows one
> [request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml)
> at a time — 51 tools and counting.

## Packages

| Tool | What it is | Latest .deb | Fresh | Install |
|------|-----------|-------------|-------|---------|
| [uv](https://github.com/astral-sh/uv) | Extremely fast Python package & project manager | [![release](https://img.shields.io/github/v/release/latest-debs/uv-debian?display_name=tag&label=)](https://github.com/latest-debs/uv-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.uv) | `sudo apt install uv` |
| [vite-plus](https://github.com/voidzero-dev/vite-plus) | The unified toolchain for the web (`vp` CLI) | [![release](https://img.shields.io/github/v/release/latest-debs/vite-plus-debian?display_name=tag&label=)](https://github.com/latest-debs/vite-plus-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.vite-plus) | `sudo apt install vite-plus` |
| [eza](https://github.com/eza-community/eza) | Modern, maintained `ls` replacement | [![release](https://img.shields.io/github/v/release/latest-debs/eza-debian?display_name=tag&label=)](https://github.com/latest-debs/eza-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.eza) | `sudo apt install eza` |
| [lazygit](https://github.com/jesseduffield/lazygit) | Simple terminal UI for git | [![release](https://img.shields.io/github/v/release/latest-debs/lazygit-debian?display_name=tag&label=)](https://github.com/latest-debs/lazygit-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.lazygit) | `sudo apt install lazygit` |
| [ruff](https://github.com/astral-sh/ruff) | Extremely fast Python linter & formatter | [![release](https://img.shields.io/github/v/release/latest-debs/ruff-debian?display_name=tag&label=)](https://github.com/latest-debs/ruff-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.ruff) | `sudo apt install ruff` |
| [bun](https://github.com/oven-sh/bun) | Fast all-in-one JavaScript runtime & toolkit | [![release](https://img.shields.io/github/v/release/latest-debs/bun-debian?display_name=tag&label=)](https://github.com/latest-debs/bun-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.bun) | `sudo apt install bun` |
| [deno](https://github.com/denoland/deno) | Modern runtime for JavaScript & TypeScript | [![release](https://img.shields.io/github/v/release/latest-debs/deno-debian?display_name=tag&label=)](https://github.com/latest-debs/deno-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.deno) | `sudo apt install deno` |
| [duckdb](https://github.com/duckdb/duckdb) | Analytical in-process SQL database | [![release](https://img.shields.io/github/v/release/latest-debs/duckdb-debian?display_name=tag&label=)](https://github.com/latest-debs/duckdb-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.duckdb) | `sudo apt install duckdb` |
| [lazydocker](https://github.com/jesseduffield/lazydocker) | The lazier way to manage everything docker | [![release](https://img.shields.io/github/v/release/latest-debs/lazydocker-debian?display_name=tag&label=)](https://github.com/latest-debs/lazydocker-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.lazydocker) | `sudo apt install lazydocker` |
| [ripgrep](https://github.com/BurntSushi/ripgrep) | Recursively search directories for a regex pattern | [![release](https://img.shields.io/github/v/release/latest-debs/ripgrep-debian?display_name=tag&label=)](https://github.com/latest-debs/ripgrep-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.ripgrep) | `sudo apt install ripgrep` |
| [fd](https://github.com/sharkdp/fd) | Simple, fast alternative to `find` | [![release](https://img.shields.io/github/v/release/latest-debs/fd-debian?display_name=tag&label=)](https://github.com/latest-debs/fd-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.fd) | `sudo apt install fd-find` |
| [fzf](https://github.com/junegunn/fzf) | Command-line fuzzy finder | [![release](https://img.shields.io/github/v/release/latest-debs/fzf-debian?display_name=tag&label=)](https://github.com/latest-debs/fzf-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.fzf) | `sudo apt install fzf` |
| [starship](https://github.com/starship/starship) | Minimal, fast, customizable shell prompt | [![release](https://img.shields.io/github/v/release/latest-debs/starship-debian?display_name=tag&label=)](https://github.com/latest-debs/starship-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.starship) | `sudo apt install starship` |
| [just](https://github.com/casey/just) | Handy command runner | [![release](https://img.shields.io/github/v/release/latest-debs/just-debian?display_name=tag&label=)](https://github.com/latest-debs/just-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.just) | `sudo apt install just` |
| [hyperfine](https://github.com/sharkdp/hyperfine) | Command-line benchmarking tool | [![release](https://img.shields.io/github/v/release/latest-debs/hyperfine-debian?display_name=tag&label=)](https://github.com/latest-debs/hyperfine-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.hyperfine) | `sudo apt install hyperfine` |
| [k9s](https://github.com/derailed/k9s) | Kubernetes CLI to manage your clusters in style | [![release](https://img.shields.io/github/v/release/latest-debs/k9s-debian?display_name=tag&label=)](https://github.com/latest-debs/k9s-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.k9s) | `sudo apt install k9s` |
| [atuin](https://github.com/atuinsh/atuin) | Magical shell history | [![release](https://img.shields.io/github/v/release/latest-debs/atuin-debian?display_name=tag&label=)](https://github.com/latest-debs/atuin-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.atuin) | `sudo apt install atuin` |
| [xh](https://github.com/ducaale/xh) | Friendly and fast tool for sending HTTP requests | [![release](https://img.shields.io/github/v/release/latest-debs/xh-debian?display_name=tag&label=)](https://github.com/latest-debs/xh-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.xh) | `sudo apt install xh` |
| [yq-go](https://github.com/mikefarah/yq) | Portable command-line YAML, JSON & XML processor | [![release](https://img.shields.io/github/v/release/latest-debs/yq-go-debian?display_name=tag&label=)](https://github.com/latest-debs/yq-go-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.yq-go) | `sudo apt install yq-go` |
| [dust](https://github.com/bootandy/dust) | A more intuitive version of `du` | [![release](https://img.shields.io/github/v/release/latest-debs/du-dust-debian?display_name=tag&label=)](https://github.com/latest-debs/du-dust-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.du-dust) | `sudo apt install du-dust` |
| [procs](https://github.com/dalance/procs) | A modern replacement for `ps` | [![release](https://img.shields.io/github/v/release/latest-debs/procs-debian?display_name=tag&label=)](https://github.com/latest-debs/procs-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.procs) | `sudo apt install procs` |
| [bottom](https://github.com/ClementTsang/bottom) | Cross-platform graphical process/system monitor | [![release](https://img.shields.io/github/v/release/latest-debs/bottom-debian?display_name=tag&label=)](https://github.com/latest-debs/bottom-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.bottom) | `sudo apt install bottom` |
| [bat](https://github.com/sharkdp/bat) | A `cat(1)` clone with wings | [![release](https://img.shields.io/github/v/release/latest-debs/bat-debian?display_name=tag&label=)](https://github.com/latest-debs/bat-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.bat) | `sudo apt install bat` |
| [zoxide](https://github.com/ajeetdsouza/zoxide) | A smarter `cd` command for your terminal | [![release](https://img.shields.io/github/v/release/latest-debs/zoxide-debian?display_name=tag&label=)](https://github.com/latest-debs/zoxide-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.zoxide) | `sudo apt install zoxide` |
| [delta](https://github.com/dandavison/delta) | Syntax-highlighting pager for git, diff, grep & blame | [![release](https://img.shields.io/github/v/release/latest-debs/git-delta-debian?display_name=tag&label=)](https://github.com/latest-debs/git-delta-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.git-delta) | `sudo apt install git-delta` |
| [jj](https://github.com/jj-vcs/jj) | Git-compatible VCS that is simple and powerful | [![release](https://img.shields.io/github/v/release/latest-debs/jj-debian?display_name=tag&label=)](https://github.com/latest-debs/jj-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.jj) | `sudo apt install jj` |
| [gitui](https://github.com/extrawurst/gitui) | Blazing-fast terminal UI for git | [![release](https://img.shields.io/github/v/release/latest-debs/gitui-debian?display_name=tag&label=)](https://github.com/latest-debs/gitui-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.gitui) | `sudo apt install gitui` |
| [fresh-editor](https://github.com/sinelaw/fresh) | Terminal-based IDE & text editor - easy, powerful and fast | [![release](https://img.shields.io/github/v/release/latest-debs/fresh-editor-debian?display_name=tag&label=)](https://github.com/latest-debs/fresh-editor-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.fresh-editor) | `sudo apt install fresh-editor` |
| [nushell](https://github.com/nushell/nushell) | A new type of shell | [![release](https://img.shields.io/github/v/release/latest-debs/nushell-debian?display_name=tag&label=)](https://github.com/latest-debs/nushell-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.nushell) | `sudo apt install nushell` |
| [dive](https://github.com/wagoodman/dive) | Explore each layer of a Docker image | [![release](https://img.shields.io/github/v/release/latest-debs/dive-debian?display_name=tag&label=)](https://github.com/latest-debs/dive-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.dive) | `sudo apt install dive` |
| [superfile](https://github.com/yorukot/superfile) | Fancy, modern terminal file manager | [![release](https://img.shields.io/github/v/release/latest-debs/superfile-debian?display_name=tag&label=)](https://github.com/latest-debs/superfile-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.superfile) | `sudo apt install superfile` |
| [pnpm](https://github.com/pnpm/pnpm) | Fast, disk-space-efficient JavaScript package manager | [![release](https://img.shields.io/github/v/release/latest-debs/pnpm-debian?display_name=tag&label=)](https://github.com/latest-debs/pnpm-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.pnpm) | `sudo apt install pnpm` |
| [act](https://github.com/nektos/act) | Run your GitHub Actions locally | [![release](https://img.shields.io/github/v/release/latest-debs/act-debian?display_name=tag&label=)](https://github.com/latest-debs/act-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.act) | `sudo apt install act` |
| [zed](https://github.com/zed-industries/zed) | High-performance multiplayer code editor | [![release](https://img.shields.io/github/v/release/latest-debs/zed-debian?display_name=tag&label=)](https://github.com/latest-debs/zed-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.zed) | `sudo apt install zed` |
| [rclone](https://github.com/rclone/rclone) | rsync for cloud storage | [![release](https://img.shields.io/github/v/release/latest-debs/rclone-debian?display_name=tag&label=)](https://github.com/latest-debs/rclone-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.rclone) | `sudo apt install rclone` |
| [k6](https://github.com/grafana/k6) | Modern load testing tool | [![release](https://img.shields.io/github/v/release/latest-debs/k6-debian?display_name=tag&label=)](https://github.com/latest-debs/k6-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.k6) | `sudo apt install k6` |
| [difftastic](https://github.com/Wilfred/difftastic) | Structural diff that understands syntax | [![release](https://img.shields.io/github/v/release/latest-debs/difftastic-debian?display_name=tag&label=)](https://github.com/latest-debs/difftastic-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.difftastic) | `sudo apt install difftastic` |
| [vhs](https://github.com/charmbracelet/vhs) | Your CLI home video recorder | [![release](https://img.shields.io/github/v/release/latest-debs/vhs-debian?display_name=tag&label=)](https://github.com/latest-debs/vhs-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.vhs) | `sudo apt install vhs` |
| [yazi](https://github.com/sxyazi/yazi) | Blazing-fast terminal file manager | [![release](https://img.shields.io/github/v/release/latest-debs/yazi-debian?display_name=tag&label=)](https://github.com/latest-debs/yazi-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.yazi) | `sudo apt install yazi` |
| [zellij](https://github.com/zellij-org/zellij) | Terminal workspace with batteries included | [![release](https://img.shields.io/github/v/release/latest-debs/zellij-debian?display_name=tag&label=)](https://github.com/latest-debs/zellij-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.zellij) | `sudo apt install zellij` |
| [neovim](https://github.com/neovim/neovim) | Vim fork focused on extensibility and usability | [![release](https://img.shields.io/github/v/release/latest-debs/neovim-debian?display_name=tag&label=)](https://github.com/latest-debs/neovim-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.neovim) | `sudo apt install neovim` |
| [gh](https://github.com/cli/cli) | GitHub's official command line tool | [![release](https://img.shields.io/github/v/release/latest-debs/gh-debian?display_name=tag&label=)](https://github.com/latest-debs/gh-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.gh) | `sudo apt install gh` |
| [mise](https://github.com/jdx/mise) | Dev tools, env vars, task runner | [![release](https://img.shields.io/github/v/release/latest-debs/mise-debian?display_name=tag&label=)](https://github.com/latest-debs/mise-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.mise) | `sudo apt install mise` |
| [gum](https://github.com/charmbracelet/gum) | Tool for glamorous shell scripts | [![release](https://img.shields.io/github/v/release/latest-debs/gum-debian?display_name=tag&label=)](https://github.com/latest-debs/gum-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.gum) | `sudo apt install gum` |
| [fastfetch](https://github.com/fastfetch-cli/fastfetch) | Feature-rich, neofetch-like system information tool | [![release](https://img.shields.io/github/v/release/latest-debs/fastfetch-debian?display_name=tag&label=)](https://github.com/latest-debs/fastfetch-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.fastfetch) | `sudo apt install fastfetch` |
| [buf](https://github.com/bufbuild/buf) | The best way of working with Protocol Buffers | [![release](https://img.shields.io/github/v/release/latest-debs/buf-debian?display_name=tag&label=)](https://github.com/latest-debs/buf-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.buf) | `sudo apt install buf` |
| [sd](https://github.com/chmln/sd) | Intuitive find & replace CLI (sed alternative) | [![release](https://img.shields.io/github/v/release/latest-debs/sd-debian?display_name=tag&label=)](https://github.com/latest-debs/sd-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.sd) | `sudo apt install sd` |
| [scc](https://github.com/boyter/scc) | Very fast, accurate code counter with complexity estimates | [![release](https://img.shields.io/github/v/release/latest-debs/scc-debian?display_name=tag&label=)](https://github.com/latest-debs/scc-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.scc) | `sudo apt install scc` |
| [trivy](https://github.com/aquasecurity/trivy) | Find vulnerabilities, misconfigurations, secrets and SBOMs | [![release](https://img.shields.io/github/v/release/latest-debs/trivy-debian?display_name=tag&label=)](https://github.com/latest-debs/trivy-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.trivy) | `sudo apt install trivy` |
| [helix](https://github.com/helix-editor/helix) | A post-modern modal text editor | [![release](https://img.shields.io/github/v/release/latest-debs/helix-debian?display_name=tag&label=)](https://github.com/latest-debs/helix-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.helix) | `sudo apt install helix` |
| [fish](https://github.com/fish-shell/fish-shell) | The user-friendly command line shell | [![release](https://img.shields.io/github/v/release/latest-debs/fish-debian?display_name=tag&label=)](https://github.com/latest-debs/fish-debian/releases) | ![freshness](https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$.fish) | `sudo apt install fish` |

`delta`, `dust`, and `fd` ship under the names `git-delta`/`du-dust`/`fd-find` —
Debian's own archive already has packages under their plain names, so ours are
disambiguated the same way upstream (or other distros) already do.

Want another tool packaged? **[Request it](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml)** — that's how the list grows. Track all open requests under the
[`package-request` label](https://github.com/latest-debs/apt-repo/labels/package-request).

## Supported systems

- **Debian:** Bookworm (12), Trixie (13), Forky (14/testing), Sid (unstable) — all live
- **Ubuntu:** Jammy (22.04 LTS), Noble (24.04 LTS), Questing (25.10), Resolute (25.10+) — live
- **Architectures:** amd64, arm64, armhf, armel, i386, ppc64el, riscv64, s390x, loong64 — whichever each upstream actually publishes a Linux build for, per suite (see each package's repo for its exact list)
- **Updates:** the repo rebuilds automatically, roughly every 6 hours after an upstream release

## Trust & verification

All packages are built in public GitHub Actions runs from public upstream
sources — no blobs, no private steps. The apt repo is signed with:

```
ed25519  9329 C48E AEC0 0249 5619  50D4 C34D 4229 518A B96A
uid      latest-debs <latest-debs@users.noreply.github.com>
```

Verify the key you downloaded before enabling the repo:

```sh
gpg --show-keys /etc/apt/keyrings/latest-debs.gpg
```

Every build is produced by
[debian-multiarch-builder](https://github.com/ranjithrajv/debian-multiarch-builder),
a reusable GitHub Action you can inspect and reuse.

Signing is only part of the story — every release also passes through:

- **Policy + runtime gate** — lintian (Debian's packaging policy checker)
  plus a smoke test that runs the packaged binary in a container and
  confirms it reports the expected version. Nothing ships that fails
  either check.
- **Provenance pin** — at vet time, the upstream asset's SHA-256 is
  cross-checked against the vendor's published checksum and pinned; the
  builder re-verifies those exact bytes on every rebuild, so a release
  altered after vetting fails the build instead of shipping silently.
- **Human gate** — every build lands as a draft GitHub release; a
  maintainer reviews and publishes it by hand. Nothing is auto-published.

See apt-repo's
[Supply chain & provenance](https://github.com/latest-debs/apt-repo#supply-chain--provenance)
for the full chain.

## Where things live

| Repo | Purpose |
|------|---------|
| [apt-repo](https://github.com/latest-debs/apt-repo) | The aggregate apt repository: signing key, `pool/`, `dists/`, build scripts, and the tool registry (`tools.yaml`) |
| `<tool>-debian` (one per package) | Per-tool packaging and multi-arch `.deb` releases — see the **Latest .deb** column in the Packages table above for each one |
| [latest-debs.github.io](https://github.com/latest-debs/latest-debs.github.io) | The landing page at [latest-debs.github.io](https://latest-debs.github.io/) |
| [.github](https://github.com/latest-debs/.github) | This profile and org-wide defaults |
| [debian-multiarch-builder](https://github.com/ranjithrajv/debian-multiarch-builder) | The reusable GitHub Action every `<tool>-debian` repo builds with |

## FAQ

**Is this official Debian?**
No — unofficial, community packaging. For issues with a tool itself, report
upstream; for packaging issues, open an issue on the tool's `-debian` repo.

**How fresh are the packages?**
A scheduled workflow checks upstream releases and rebuilds; expect new
versions within ~6 hours of the upstream tag. The **Fresh** badge on each
package above shows the packaged version and its age as of the last
rebuild (updated every 6 hours).

**How do I add my favorite tool?**
Open a
[package request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml) —
name, upstream URL, and license is all we need. Or add an entry to
[`tools.yaml`](https://github.com/latest-debs/apt-repo/blob/main/tools.yaml)
in a PR against `apt-repo`.

## Contributing & support

**Outreach is the thing we need most right now** — the tooling works and the
packages exist; what's missing is people bringing them to the projects and
developers who'd benefit. If you'd like to **sign up for outreach** — pitching
the packaging service to a project's maintainers, driving package requests,
writing about the project, or fielding questions in
[discussions](https://github.com/orgs/latest-debs/discussions) — see
[CONTRIBUTING.md](https://github.com/latest-debs/.github/blob/main/CONTRIBUTING.md).

Packaging fixes, new tools, and build-config reviews are also welcome — see
[apt-repo](https://github.com/latest-debs/apt-repo) for how the pipeline
works. If latest-debs saves you time,
[sponsor the automation](https://github.com/sponsors/latest-debs).
