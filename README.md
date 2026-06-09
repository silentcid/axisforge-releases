# AxisForge — Releases

Public download distribution for **[AxisForge](https://www.axisforge.app)** —
the all-in-one Linux HOTAS toolkit. Action-graph remapping, live response
curves, mode stacks, AI setup assistant — one app, one profile, native.

→ **Website:** https://www.axisforge.app
→ **Latest version:** see the [Releases page](../../releases) on this repo.

---

## What this repo is

Every AxisForge build (AppImage, .deb, .rpm, AUR-sidecar, future
Windows installer) is published here under
[GitHub Releases](../../releases). The repo itself only carries the
README, installation instructions, and changelog — release binaries
live in the Releases page so they don't bloat the git history.

This is **not** the source repo. AxisForge is a commercial closed-source
product; the source lives in a private repo. Bug reports, feature
requests, and support questions come in through the website.

## Install

### AppImage (any modern distro)

```bash
# 1. Grab the latest from the Releases page
curl -L -o ~/Applications/AxisForge.AppImage \
  https://github.com/silentcid/axisforge-releases/releases/latest/download/AxisForge_1.0.0_amd64.AppImage

# 2. Make it executable
chmod +x ~/Applications/AxisForge.AppImage

# 3. Run it
~/Applications/AxisForge.AppImage
```

First-launch banner will walk you through the udev / `input` group setup.
Two terminal commands, log out and back in, done.

### Arch / Manjaro (AUR)

```bash
yay -S axisforge-bin
```

The PKGBUILD extracts the same AppImage and installs the binaries + udev
rules + desktop entry into system paths. AppImageLauncher not required.

### Fedora / openSUSE (Copr)

```bash
sudo dnf copr enable silentcid/axisforge
sudo dnf install axisforge
```

### Bazzite / Silverblue / Bluefin / Aurora (Fedora atomic)

rpm-ostree based distros seal `/usr`, so `dnf install` doesn't apply.
**Use the AppImage** — it runs unmodified on every atomic image, and the
two pieces of system setup AxisForge needs both touch `/etc`, which stays
writable.

```bash
# 1. Follow the AppImage section above to download + chmod +x.

# 2. Add yourself to the input group (one-time).
sudo usermod -aG input $USER

# 3. Install the udev rule. The app's first-launch banner prints the
#    exact line + destination path; just paste it. /etc/udev/rules.d/
#    is writable even under the immutable base.
sudo udevadm control --reload-rules && sudo udevadm trigger

# 4. Log out and back in (or reboot — required either way for the
#    new group membership to apply).
```

Avoid `rpm-ostree install axisforge` from the Copr unless you specifically
want the package layered onto your base image: it works, but it adds a
reboot to each install and re-layering to every base update. The AppImage
path stays cleanly outside the OS image.

### Ubuntu / Debian (.deb)

Download `axisforge_1.0.0_amd64.deb` from the Releases page, then:

```bash
sudo apt install ./axisforge_1.0.0_amd64.deb
```

## Verifying downloads

Every release tarball is accompanied by `SHA256SUMS.txt`. Verify before
running:

```bash
sha256sum -c SHA256SUMS.txt --ignore-missing
```

## Wine / Proton games (Star Citizen, DCS, IL-2, …)

Linux's joydev caps each virtual joystick at 80 buttons. To expose all
128 buttons under Wine, you'll need one extra udev line — AxisForge's
in-app permissions banner has a one-click "copy install command" button
that handles it. Full explanation in the
[Wine compatibility section](https://www.axisforge.app#wine) of the
website.

## Support

- **Bug reports / feature requests:** https://www.axisforge.app
  (contact link in the footer)
- **Discord:** https://discord.gg/KjNpuKXhYS
- **Email:** hello@axisforge.app

## License

AxisForge is a commercial closed-source product. The bundled binaries
are licensed for personal use only — see `LICENSE` inside the AppImage
or the EULA presented on first launch.

Re-distribution of the AppImage / .deb / .rpm artifacts is not
permitted. Link to https://www.axisforge.app or to this Releases page
instead.
