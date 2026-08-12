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

On Debian (Bookworm or Trixie), with [extrepo](https://salsa.debian.org/extrepo-team/extrepo):

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
> at a time — 24 tools and counting.

## Packages

| Tool | What it is | Latest .deb | Install |
|------|-----------|-------------|---------|
| [uv](https://github.com/astral-sh/uv) | Extremely fast Python package & project manager | [![release](https://img.shields.io/github/v/release/latest-debs/uv-debian?display_name=tag&label=)](https://github.com/latest-debs/uv-debian/releases) | `sudo apt install uv` |
| [eza](https://github.com/eza-community/eza) | Modern, maintained `ls` replacement | [![release](https://img.shields.io/github/v/release/latest-debs/eza-debian?display_name=tag&label=)](https://github.com/latest-debs/eza-debian/releases) | `sudo apt install eza` |
| [lazygit](https://github.com/jesseduffield/lazygit) | Simple terminal UI for git | [![release](https://img.shields.io/github/v/release/latest-debs/lazygit-debian?display_name=tag&label=)](https://github.com/latest-debs/lazygit-debian/releases) | `sudo apt install lazygit` |
| [ruff](https://github.com/astral-sh/ruff) | Extremely fast Python linter & formatter | [![release](https://img.shields.io/github/v/release/latest-debs/ruff-debian?display_name=tag&label=)](https://github.com/latest-debs/ruff-debian/releases) | `sudo apt install ruff` |
| [bun](https://github.com/oven-sh/bun) | Fast all-in-one JavaScript runtime & toolkit | [![release](https://img.shields.io/github/v/release/latest-debs/bun-debian?display_name=tag&label=)](https://github.com/latest-debs/bun-debian/releases) | `sudo apt install bun` |
| [deno](https://github.com/denoland/deno) | Modern runtime for JavaScript & TypeScript | [![release](https://img.shields.io/github/v/release/latest-debs/deno-debian?display_name=tag&label=)](https://github.com/latest-debs/deno-debian/releases) | `sudo apt install deno` |
| [duckdb](https://github.com/duckdb/duckdb) | Analytical in-process SQL database | [![release](https://img.shields.io/github/v/release/latest-debs/duckdb-debian?display_name=tag&label=)](https://github.com/latest-debs/duckdb-debian/releases) | `sudo apt install duckdb` |
| [lazydocker](https://github.com/jesseduffield/lazydocker) | The lazier way to manage everything docker | [![release](https://img.shields.io/github/v/release/latest-debs/lazydocker-debian?display_name=tag&label=)](https://github.com/latest-debs/lazydocker-debian/releases) | `sudo apt install lazydocker` |
| [ripgrep](https://github.com/BurntSushi/ripgrep) | Recursively search directories for a regex pattern | [![release](https://img.shields.io/github/v/release/latest-debs/ripgrep-debian?display_name=tag&label=)](https://github.com/latest-debs/ripgrep-debian/releases) | `sudo apt install ripgrep` |
| [fd](https://github.com/sharkdp/fd) | Simple, fast alternative to `find` | [![release](https://img.shields.io/github/v/release/latest-debs/fd-debian?display_name=tag&label=)](https://github.com/latest-debs/fd-debian/releases) | `sudo apt install fd` |
| [fzf](https://github.com/junegunn/fzf) | Command-line fuzzy finder | [![release](https://img.shields.io/github/v/release/latest-debs/fzf-debian?display_name=tag&label=)](https://github.com/latest-debs/fzf-debian/releases) | `sudo apt install fzf` |
| [starship](https://github.com/starship/starship) | Minimal, fast, customizable shell prompt | [![release](https://img.shields.io/github/v/release/latest-debs/starship-debian?display_name=tag&label=)](https://github.com/latest-debs/starship-debian/releases) | `sudo apt install starship` |
| [just](https://github.com/casey/just) | Handy command runner | [![release](https://img.shields.io/github/v/release/latest-debs/just-debian?display_name=tag&label=)](https://github.com/latest-debs/just-debian/releases) | `sudo apt install just` |
| [hyperfine](https://github.com/sharkdp/hyperfine) | Command-line benchmarking tool | [![release](https://img.shields.io/github/v/release/latest-debs/hyperfine-debian?display_name=tag&label=)](https://github.com/latest-debs/hyperfine-debian/releases) | `sudo apt install hyperfine` |
| [k9s](https://github.com/derailed/k9s) | Kubernetes CLI to manage your clusters in style | [![release](https://img.shields.io/github/v/release/latest-debs/k9s-debian?display_name=tag&label=)](https://github.com/latest-debs/k9s-debian/releases) | `sudo apt install k9s` |
| [atuin](https://github.com/atuinsh/atuin) | Magical shell history | [![release](https://img.shields.io/github/v/release/latest-debs/atuin-debian?display_name=tag&label=)](https://github.com/latest-debs/atuin-debian/releases) | `sudo apt install atuin` |
| [xh](https://github.com/ducaale/xh) | Friendly and fast tool for sending HTTP requests | [![release](https://img.shields.io/github/v/release/latest-debs/xh-debian?display_name=tag&label=)](https://github.com/latest-debs/xh-debian/releases) | `sudo apt install xh` |
| [yq](https://github.com/mikefarah/yq) | Portable command-line YAML, JSON & XML processor | [![release](https://img.shields.io/github/v/release/latest-debs/yq-debian?display_name=tag&label=)](https://github.com/latest-debs/yq-debian/releases) | `sudo apt install yq-go` |
| [dust](https://github.com/bootandy/dust) | A more intuitive version of `du` | [![release](https://img.shields.io/github/v/release/latest-debs/du-dust-debian?display_name=tag&label=)](https://github.com/latest-debs/du-dust-debian/releases) | `sudo apt install du-dust` |
| [procs](https://github.com/dalance/procs) | A modern replacement for `ps` | [![release](https://img.shields.io/github/v/release/latest-debs/procs-debian?display_name=tag&label=)](https://github.com/latest-debs/procs-debian/releases) | `sudo apt install procs` |
| [bottom](https://github.com/ClementTsang/bottom) | Cross-platform graphical process/system monitor | [![release](https://img.shields.io/github/v/release/latest-debs/bottom-debian?display_name=tag&label=)](https://github.com/latest-debs/bottom-debian/releases) | `sudo apt install bottom` |
| [bat](https://github.com/sharkdp/bat) | A `cat(1)` clone with wings | [![release](https://img.shields.io/github/v/release/latest-debs/bat-debian?display_name=tag&label=)](https://github.com/latest-debs/bat-debian/releases) | `sudo apt install bat` |
| [zoxide](https://github.com/ajeetdsouza/zoxide) | A smarter `cd` command for your terminal | [![release](https://img.shields.io/github/v/release/latest-debs/zoxide-debian?display_name=tag&label=)](https://github.com/latest-debs/zoxide-debian/releases) | `sudo apt install zoxide` |
| [delta](https://github.com/dandavison/delta) | Syntax-highlighting pager for git, diff, grep & blame | [![release](https://img.shields.io/github/v/release/latest-debs/git-delta-debian?display_name=tag&label=)](https://github.com/latest-debs/git-delta-debian/releases) | `sudo apt install git-delta` |

`yq` and `delta`/`dust` ship under the names `yq-go`/`git-delta`/`du-dust` —
Debian's own archive already has unrelated packages under their plain
names, so ours are disambiguated the same way upstream (or other distros)
already do.

Want another tool packaged? **[Request it](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml)** — that's how the list grows. Track all open requests under the
[`package-request` label](https://github.com/latest-debs/apt-repo/labels/package-request).

## Supported systems

- **Debian:** Bookworm (12), Trixie (13), Forky (14/testing), and Sid (unstable) — all live
- **Architectures:** amd64, arm64, armhf, armel, i386 (Bookworm/Trixie only), ppc64el, riscv64, s390x, loong64 — whichever each upstream actually publishes a Linux build for (not every tool covers all nine; see each package's repo for its exact list)
- **Updates:** the repo rebuilds automatically, roughly every 6 hours after an upstream release

Ubuntu and derivatives are *not* officially targeted yet — the packages may
install fine, but they're only tested on Debian.

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
versions within ~6 hours of the upstream tag.

**How do I add my favorite tool?**
Open a
[package request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml) —
name, upstream URL, and license is all we need. Or add an entry to
[`tools.yaml`](https://github.com/latest-debs/apt-repo/blob/main/tools.yaml)
in a PR against `apt-repo`.

## Contributing & support

Packaging fixes, new tools, and build-config reviews are all welcome — see
[apt-repo](https://github.com/latest-debs/apt-repo) for how the pipeline
works. If latest-debs saves you time,
[sponsor the automation](https://github.com/sponsors/latest-debs).
