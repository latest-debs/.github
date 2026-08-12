# Security Policy

This file is the org-wide default — it applies to any repo under
[latest-debs](https://github.com/latest-debs) that doesn't have its own
`SECURITY.md`.

## Scope

latest-debs repackages upstream releases as `.deb` packages; it does not
patch or modify upstream source. Please keep that distinction in mind when
reporting:

- **Vulnerability in the packaged tool itself** (e.g. a bug in `ripgrep` or
  `uv`) — report it upstream, directly to that project. Packaging repos
  here just build and ship what upstream releases.
- **Vulnerability in the packaging or distribution pipeline** (e.g. the
  build workflow, `apt-repo` signing/serving, a compromised release
  artifact, or a `package.yaml`/`tools.yaml` supply-chain issue) — report it
  to us using the instructions below.

## Reporting a vulnerability

Please **do not** open a public issue for a packaging/distribution security
report. Instead, use
[GitHub Security Advisories](https://github.com/latest-debs/apt-repo/security/advisories/new)
on the `apt-repo` repo, or email
**latest-debs@users.noreply.github.com**.

Include:

- Affected repo(s) and package version(s)
- Steps to reproduce or evidence of the issue
- Impact, if known (e.g. malicious artifact, broken signature verification)

We aim to acknowledge reports within a few days. As a small, volunteer-run
project we don't offer a bug bounty, but we'll credit reporters in the fix
if desired.

## Package integrity

apt-repo indexes are GPG-signed; verify packages via the `signed-by=`
keyring as documented in
[apt-repo's README](https://github.com/latest-debs/apt-repo#install) rather
than disabling signature checks.
