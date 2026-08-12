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
so `apt install` and `apt upgrade` just work, across 8 architectures, with
source packages available, within hours of each upstream release. Native
packages, native tooling, no compromises.

## The real mission: make Debian packaging fast and easy

Debian packaging has a reputation — and it deserves it. `debhelper`, Policy,
`control`/`rules` files, multi-arch builds, signing, repo hosting: powerful,
but a steep and time-consuming detour when you just want to ship a CLI. Many
developers, teams, and orgs skip `.deb` entirely — and their Debian users pay
for it with `curl | sh` scripts and stale copies.

**The main goal of this initiative is to remove that cost:**

- **One reusable builder** —
  [debian-multiarch-builder](https://github.com/ranjithrajv/debian-multiarch-builder),
  a GitHub Action that turns an upstream release into signed, multi-arch
  `.deb` packages (plus source packages) in a single workflow run. No
  packaging expertise required.
- **Distribution included** — releases flow into this apt repo automatically.
  Nothing to host, sign, or babysit.
- **Your tool here** — add it to
  [`tools.yaml`](https://github.com/latest-debs/apt-repo/blob/main/tools.yaml)
  or ask in [Discussions](https://github.com/orgs/latest-debs/discussions),
  and every future upstream release ships as a `.deb` within hours.

Everything in this org is built with that same pipeline — it's the demo as
well as the product.

## New here? Start in 30 seconds

On Debian (Bookworm or Trixie), with [extrepo](https://salsa.debian.org/extrepo-team/extrepo):

```sh
sudo extrepo enable latest-debs
sudo apt update
sudo apt install uv eza lazygit
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

> **Status: early days.** The org is new and the apt repo is rolling out
> suite by suite. If `apt update` 404s for your suite, every package is also
> published as a plain `.deb` on each repo's Releases page — grab it and
> `sudo dpkg -i <file>.deb`.

## Packages

| Tool | What it is | Latest .deb | Install |
|------|-----------|-------------|---------|
| [uv](https://github.com/astral-sh/uv) | Extremely fast Python package & project manager | [![release](https://img.shields.io/github/v/release/latest-debs/uv-debian?display_name=tag&label=)](https://github.com/latest-debs/uv-debian/releases) | `sudo apt install uv` |
| [eza](https://github.com/eza-community/eza) | Modern, maintained `ls` replacement | [![release](https://img.shields.io/github/v/release/latest-debs/eza-debian?display_name=tag&label=)](https://github.com/latest-debs/eza-debian/releases) | `sudo apt install eza` |
| [lazygit](https://github.com/jesseduffield/lazygit) | Simple terminal UI for git | [![release](https://img.shields.io/github/v/release/latest-debs/lazygit-debian?display_name=tag&label=)](https://github.com/latest-debs/lazygit-debian/releases) | `sudo apt install lazygit` |

Want another tool packaged? **[Request it](https://github.com/orgs/latest-debs/discussions)** — that's how the list grows.

## Supported systems

- **Debian:** Bookworm (12) and Trixie (13) live now; Forky (14/testing) and Sid (unstable) as builds land
- **Architectures:** amd64, arm64, armel, armhf, i386, ppc64el, s390x, riscv64
- **Source packages** (`.dsc`) are published alongside every binary
- **Updates:** the repo rebuilds automatically, roughly every 6 hours after an upstream release

Ubuntu and derivatives are *not* officially targeted yet — the packages may
install fine, but they're only tested on Debian.

## Trust & verification

All packages are built in public GitHub Actions runs from public upstream
sources — no blobs, no private steps. The apt repo is signed with:

```
ed25519  2FD5 D141 A92D 0202 CA5D  2889 A2D5 15CF C75D D552
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
| [uv-debian](https://github.com/latest-debs/uv-debian) · [eza-debian](https://github.com/latest-debs/eza-debian) · [lazygit-debian](https://github.com/latest-debs/lazygit-debian) | Per-tool packaging and multi-arch `.deb` releases |
| [latest-debs.github.io](https://github.com/latest-debs/latest-debs.github.io) | The landing page at [latest-debs.github.io](https://latest-debs.github.io/) |
| [.github](https://github.com/latest-debs/.github) | This profile and org-wide defaults |

## FAQ

**Is this official Debian?**
No — unofficial, community packaging. For issues with a tool itself, report
upstream; for packaging issues, open an issue on the tool's `-debian` repo.

**How fresh are the packages?**
A scheduled workflow checks upstream releases and rebuilds; expect new
versions within ~6 hours of the upstream tag.

**`apt update` says the Release file is missing / unsigned?**
The repo is still being rolled out per suite. Check the status note above,
or install the `.deb` directly from Releases in the meantime.

**How do I add my favorite tool?**
Add an entry to
[`tools.yaml`](https://github.com/latest-debs/apt-repo/blob/main/tools.yaml)
in a PR against `apt-repo`, or just ask in
[Discussions](https://github.com/orgs/latest-debs/discussions).

## Contributing & support

Packaging fixes, new tools, and build-config reviews are all welcome — see
[apt-repo](https://github.com/latest-debs/apt-repo) for how the pipeline
works. If latest-debs saves you time,
[sponsor the automation](https://github.com/sponsors/latest-debs).
