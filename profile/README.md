<p align="center">
  <img src="https://raw.githubusercontent.com/latest-debs/.github/main/assets/latest-debs-logo.svg" width="128" alt="latest-debs">
</p>

<h1 align="center">latest-debs</h1>

<p align="center">
  <b>Latest stable releases of developer tools, packaged as <code>.deb</code> and served over <code>apt</code>.</b><br>
  Debian freezes package versions for years. We track upstream and publish new releases within hours.
</p>

<p align="center">
  <a href="https://latest-debs.github.io/"><img src="https://img.shields.io/badge/website-latest--debs.github.io-1f6feb" alt="website"></a>
  <a href="https://github.com/orgs/latest-debs/discussions"><img src="https://img.shields.io/badge/chat-org%20discussions-238636" alt="discussions"></a>
  <a href="https://github.com/sponsors/latest-debs"><img src="https://img.shields.io/badge/sponsor-%E2%9D%A4-ea4aaa" alt="sponsor"></a>
</p>

---

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
