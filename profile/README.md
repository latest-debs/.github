<p align="center">
  <img src="https://raw.githubusercontent.com/latest-debs/.github/main/assets/latest-debs-logo.svg" width="128" alt="latest-debs">
</p>

<h1 align="center">latest-debs</h1>

<p align="center">
  <b>Latest stable releases of developer tools, packaged as <code>.deb</code> and served over <code>apt</code>.</b><br>
  Current within hours of upstream — on <b>nine architectures</b>, including the ones<br>
  nobody else builds for — and with a written policy to <b>retire ourselves</b> when Debian catches up.
</p>

<p align="center">
  <a href="https://latest-debs.github.io/"><img src="https://img.shields.io/badge/website-latest--debs.github.io-1f6feb" alt="website"></a>
  <a href="https://latest-debs.github.io/apt-repo/dists/freshness.json"><img src="https://img.shields.io/endpoint?url=https%3A%2F%2Flatest-debs.github.io%2Fapt-repo%2Fdists%2Ffreshness.json&query=$._meta" alt="freshness"></a>
  <a href="https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml"><img src="https://img.shields.io/badge/request-a%20package-blueviolet" alt="request a package"></a>
  <a href="https://github.com/orgs/latest-debs/discussions"><img src="https://img.shields.io/badge/chat-org%20discussions-238636" alt="discussions"></a>
  <a href="https://github.com/sponsors/latest-debs"><img src="https://img.shields.io/badge/sponsor-%E2%9D%A4-ea4aaa" alt="sponsor"></a>
</p>

---

## Three things that make this different

Plenty of projects will hand you a newer binary. These are the parts that are
hard to copy.

### 1. Current, without moving your OS

New upstream release in, signed `.deb` out, usually within hours — built *for*
the stable suites, so `apt upgrade` gets you a current toolchain while
everything else on the system stays where it is. That is the table stakes, and
it is the only one of the three anyone else offers.

### 2. The architectures nobody else covers

Most third-party channels are amd64, sometimes plus arm64, and stop. We build
every architecture each upstream actually publishes a Linux binary for — so
the catalogue reaches hardware the alternatives simply do not serve. Measured
on `trixie` today:

| Architecture | Tools | Who else ships these |
|---|---|---|
| `amd64` / `arm64` | 50 / 51 | everyone |
| `armhf` / `i386` | 23 / 18 | rarely |
| `riscv64` | 14 — incl. `uv`, `ruff`, `fzf`, `just`, `starship`, `zoxide` | almost nobody |
| `ppc64el` | 10 | almost nobody |
| `s390x` | 9 — incl. `uv`, `ripgrep`, `trivy`, `k9s` | almost nobody |
| `loong64` | 6 | almost nobody |
| `armel` | 1 | almost nobody |

**32 of the packaged tools ship on at least one architecture beyond
amd64/arm64.** If you are on a RISC-V board, an s390x LPAR, or a POWER
machine, that is the difference between `apt install` and a from-source build
with a toolchain you have to maintain yourself.

### 3. A policy that ends with us gone

We retire ourselves when Debian catches up — written into the pipeline, not
left to good intentions:

- A tool reaches its latest upstream version in a **released** suite
  (bookworm, trixie) → we **retire our package**. You already have it.
- Parity in a **rolling** suite (forky, sid) → we **drop that suite only** and
  keep shipping to the rest, because a stable-suite user cannot reach sid
  without pinning.
- Every hand-off is appended to a public
  [ledger](https://github.com/latest-debs/apt-repo/blob/main/graduated.json)
  saying where to go instead, so a package that leaves is never a dead end.

The build applies those drops automatically from a daily parity report, and
the [dashboard](https://latest-debs.github.io/status.html) counts suites
handed back as **wins**, not as a backlog. No vendor apt repo and no Homebrew
tap has an equivalent — nothing in either shrinks its own scope when the
distribution catches up.

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

On Debian or Ubuntu, one line:

```sh
curl -fsSL https://latest-debs.github.io/install.sh | sh
```

Or via [extrepo](https://salsa.debian.org/extrepo-team/extrepo):

```sh
sudo extrepo enable latest-debs
sudo apt update
sudo apt install uv eza lazygit ruff bun deno duckdb lazydocker
```

> [!IMPORTANT]
> **`extrepo` is temporarily broken — use one of the other two above.** The
> policy published in extrepo-data still points at our old base URI, which
> serves the package indexes but no longer the packages themselves. `apt
> update` succeeds and the tools appear, then `apt install` 404s. An update
> is pending upstream.

Or add the repository manually:

```sh
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://raw.githubusercontent.com/latest-debs/apt-repo/main/latest-debs.asc \
  | sudo gpg --dearmor --yes -o /etc/apt/keyrings/latest-debs.gpg
echo "deb [signed-by=/etc/apt/keyrings/latest-debs.gpg] https://latest-debs.ranjithraj.workers.dev/ $(lsb_release -sc) main" \
  | sudo tee /etc/apt/sources.list.d/latest-debs.list
sudo apt update
```

> **Status: early days, but working.** All five Debian suites (Bullseye,
> Bookworm, Trixie, Forky, Sid) are live and signed today, plus four Ubuntu
> releases served as aliases. The catalog grows one
> [request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml)
> at a time — 55 tools and counting.

## Packages

<!-- packages:start -->

**0 tools**, each a signed, test-gated `.deb` rebuilt automatically on every upstream release — .

Browse the full catalogue live — searchable, with the version we ship next to what Debian and Ubuntu ship — at **[latest-debs.github.io](https://latest-debs.github.io/#packages)**.

<!-- packages:end -->

## Supported systems

- **Debian:** Bullseye (11), Bookworm (12), Trixie (13), Forky (14/testing), Sid (unstable) — all live
- **Ubuntu:** Jammy (22.04 LTS), Noble (24.04 LTS), Questing (25.10), Resolute (26.04 LTS) — served as aliases of a Debian suite with older-or-equal glibc, so no separate Ubuntu build is needed
- **Architectures:** amd64, arm64, armhf, armel, i386, ppc64el, riscv64, s390x, loong64 — whichever each upstream actually publishes a Linux build for, per suite. See [the architecture table above](#2-the-architectures-nobody-else-covers) for how far the catalogue actually reaches on each.
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
