# Contributing to latest-debs

Thanks for your interest in improving latest-debs! This file is the
org-wide default — it applies to any repo under
[latest-debs](https://github.com/latest-debs) that doesn't have its own
`CONTRIBUTING.md`.

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
