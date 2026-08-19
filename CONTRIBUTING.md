# Contributing to latest-debs

Thanks for your interest in improving latest-debs! This file is the
org-wide default — it applies to any repo under
[latest-debs](https://github.com/latest-debs) that doesn't have its own
`CONTRIBUTING.md`.

## 🎙️ Outreach — sign up to help spread the word

The tooling, the pipeline, and the packages all work. What the initiative
needs most right now is **people** — people who'll help bring latest-debs
and these packages to the developers and projects who'd benefit from them.

If you'd like to **sign up for outreach**, here's what that looks like (any
subset, any time — no commitment beyond what you pick):

- **Introduce a project to the packaging service.** A tool you use or admire
  that ships a Linux binary but has no Debian package (or a stale one) — tell
  its maintainers about [latest-debs](https://github.com/latest-debs). The
  [service pitch](https://github.com/latest-debs/apt-repo/blob/main/SERVICE.md)
  is written for exactly that conversation.
- **Drive a package request.** Find the tool, open the
  [request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml)
  yourself (name + upstream URL + license is all it takes), and follow up
  until it's live.
- **Spread the word.** A blog post, a Hacker News/Reddit thread, a talk, a
  tweet — explain the problem (Debian's frozen dev tools) and the fix
  (`apt install` gets current versions, signed and test-gated). The
  [org profile](https://github.com/latest-debs) and
  [SERVICE.md](https://github.com/latest-debs/apt-repo/blob/main/SERVICE.md)
  are the source material; link back to
  [latest-debs.github.io](https://latest-debs.github.io/).
- **Help in conversations.** Answer questions in
  [org discussions](https://github.com/orgs/latest-debs/discussions), on the
  landing page, or on package-request issues — "is my distro supported?",
  "how do I install X?", "is this official Debian?".
- **Review and polish the message.** The profile, the READMEs, SERVICE.md,
  and the landing page are copy we want to keep sharp. Suggest edits, file
  issues, open PRs.

**Sign up:** reply in
[org discussions](https://github.com/orgs/latest-debs/discussions) ("count me
in for outreach" + pick an item), or just start — open an issue or PR here,
or in [apt-repo](https://github.com/latest-debs/apt-repo). We'll point you
at whatever needs a hand.

## 📦 Upstreaming — help get a tool into Debian proper

Some tracked tools have no official Debian package, or a stale one;
latest-debs papers over that gap, but the real fix is a package in the
Debian archive itself — and it's the highest-leverage contribution here.
The tool ships via `apt` with zero extra repos, keys, or trust decisions to
explain; it gets Debian's own QA (autopkgtest, the security tracker, freeze
policy) instead of ours; and once a tool lands in Debian proper, latest-debs
stops being needed for it.

Ready to help? Check our
[Packages](https://github.com/latest-debs/apt-repo#packages) table for a
tool that's missing from Debian or outdated there, then file an ITP (Intent
to Package) bug — `reportbug wnpp`, severity `wishlist`, subject
`ITP: <package> -- <description>` — to claim it. From there,
[mentors.debian.net](https://mentors.debian.net/) and the
[New Maintainers' Guide](https://www.debian.org/doc/manuals/maint-guide/)
walk through packaging and finding a sponsor. Open an issue or discussion to
let us know you're picking one up too.

## Requesting a new package

Open a
[package request](https://github.com/latest-debs/apt-repo/issues/new?template=package-request.yml)
on [apt-repo](https://github.com/latest-debs/apt-repo) with the tool name,
upstream URL, and license. Tracked under the
[`package-request` label](https://github.com/latest-debs/apt-repo/labels/package-request).

## Reporting a packaging bug

Open an issue on the specific `<tool>-debian` repo (not `apt-repo`) — that's
where the build definition (`package.yaml`) and workflows live. For issues
with the tool itself rather than its packaging, report upstream instead.

## Adding a package yourself

1. Fork the relevant `<tool>-debian` repo (or create a new one from an
   existing `<tool>-debian` repo as a template) and adjust `package.yaml`.
2. Add an entry to
   [`tools.yaml`](https://github.com/latest-debs/apt-repo/blob/main/tools.yaml)
   in `apt-repo` pointing at the new repo.
3. Open a pull request. The scheduled `apt-repo` build picks up new/updated
   entries automatically once merged.

Release assets must be named so the Debian suite is embedded, e.g.:

```
<package>_<version>-<build>.<suite>_<arch>.deb
```

## Improving the pipeline

- **Fix packaging issues** on any `<tool>-debian` repo — a glob, an arch
  pattern, a lintian tag, a failing smoke test.
- **Improve the builder.** The
  [debian-multiarch-builder](https://github.com/ranjithrajv/debian-multiarch-builder)
  action is the engine; a fix there ships to every package via the
  [feature channel](https://github.com/latest-debs/apt-repo/blob/main/README.md#shipping-a-template-change-the-feature-channel).
- **Harden the vet/approval flow** — provenance pinning, license/SPDX
  pre-checks, per-arch coverage checks, draft-before-publish. See
  [apt-repo](https://github.com/latest-debs/apt-repo).

## Pull requests

- Keep changes scoped and explain the "why" in the description.
- Packaging changes should be tested with the repo's
  [Build workflow](../../actions) before opening a PR where possible.
- Be respectful and constructive in reviews and discussion.

## Questions

Open a [discussion or issue](https://github.com/latest-debs/apt-repo/issues)
on `apt-repo`, or check the
[landing page](https://latest-debs.github.io/) for an overview of the
project.

## Code of conduct

Be kind, be constructive, and assume good faith. This is a volunteer project;
maintainers are people with limited time.

## Supporting the project

If latest-debs saves you time,
[sponsor the automation](https://github.com/sponsors/latest-debs).
