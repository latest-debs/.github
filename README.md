<p align="center">
  <img src="https://raw.githubusercontent.com/latest-debs/.github/main/assets/latest-debs-logo.svg" width="128" alt="latest-debs">
</p>

<h1 align="center">latest-debs</h1>

<p align="center">
  <b>Latest stable releases of developer tools, packaged as Debian packages.</b><br>
  Debian and Ubuntu freeze package versions for years. We don't.
</p>

<p align="center">
  <a href="#packages">Packages</a> ·
  <a href="#install">Install</a> ·
  <a href="https://github.com/orgs/latest-debs/discussions">Discussions</a> ·
  <a href="https://github.com/sponsors/latest-debs">Sponsor</a>
</p>

---

**latest-debs** tracks popular developer tools and publishes each new
upstream release as properly packaged `.deb` files within hours — signed
and served over `apt`, ready for Debian (Bookworm, Trixie, Forky, Sid).

## Install

Enable the repository:

```sh
sudo extrepo enable latest-debs
sudo apt update
```

Or add it manually:

```sh
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://raw.githubusercontent.com/latest-debs/apt-repo/main/latest-debs.asc | sudo gpg --dearmor --yes -o /etc/apt/keyrings/latest-debs.gpg
echo "deb [signed-by=/etc/apt/keyrings/latest-debs.gpg] https://latest-debs.github.io/apt/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/latest-debs.list
sudo apt update
```

Then install any tool:

```sh
sudo apt install uv eza lazygit
```

## Packages

| Tool | Description | Install |
|------|-------------|---------|
| [uv](https://github.com/astral-sh/uv) | Fast Python package & project manager | `sudo apt install uv` |
| [eza](https://github.com/eza-community/eza) | Modern `ls` replacement | `sudo apt install eza` |
| [lazygit](https://github.com/jesseduffield/lazygit) | Simple terminal UI for git | `sudo apt install lazygit` |

All packages are built with the
[debian-multiarch-builder](https://github.com/ranjithrajv/debian-multiarch-builder)
GitHub Action and published from source on GitHub.

## Contributing

Open to adding tools, fixing packaging, and reviewing build configs. See
[apt-repo](https://github.com/latest-debs/apt-repo) to add or update a tool,
or start a [discussion](https://github.com/orgs/latest-debs/discussions).

## Support

This project is free and open source. If it saves you time,
[sponsor the automation](https://github.com/sponsors/latest-debs).
