# AxisForge — Releases

Public download distribution for **[AxisForge](https://www.axisforge.app)** —
the all-in-one Linux HOTAS toolkit. Action-graph remapping, live response
curves, mode stacks, AI setup assistant — one app, one profile, native.

→ **Website:** https://www.axisforge.app
→ **Latest version:** see the [Releases page](../../releases) on this repo.

---

## What this repo is

Every AxisForge build (AppImage, .deb, .rpm, future Windows installer)
is published here under [GitHub Releases](../../releases). The repo
itself only carries this README and the installation instructions —
release binaries live in the Releases page so they don't bloat the git
history.

This is **not** the source repo. AxisForge is a commercial closed-source
product; the source lives in a private repo. Bug reports, feature
requests, and support questions come in through the website.

## Which file do I download?

Each release on the [Releases page](../../releases/latest) carries five
files. `<version>` is the release number, for example `1.0.6`.

| File | For |
|---|---|
| `AxisForge_<version>_amd64.deb` | Ubuntu / Debian (recommended there) |
| `AxisForge-<version>-1.x86_64.rpm` | Fedora / openSUSE (recommended there) |
| `AxisForge-<version>-x86_64.AppImage` | Any modern distro (glibc 2.39 or newer) |
| `AxisForge-<version>-compat-x86_64.AppImage` | Older distros (glibc 2.35 or newer), such as Ubuntu 22.04 and Debian 12 |
| `AxisForge-<version>-steamos-x86_64.AppImage` | SteamOS / Steam Deck |

`SHA256SUMS` lists the checksum of all five.

Not sure which AppImage? Run `ldd --version`. If the number on the first
line is 2.39 or higher, take the plain AppImage. If it is lower, take the
compat one. On a Steam Deck, take the SteamOS one.

## Install

### AppImage (any distro)

```bash
# 1. Set this to the latest version shown on the Releases page.
VERSION=1.0.6

# 2. Download. For the compat or SteamOS build, add -compat or -steamos
#    after the version: AxisForge-$VERSION-compat-x86_64.AppImage
mkdir -p ~/Applications
curl -L -o ~/Applications/AxisForge.AppImage \
  https://github.com/silentcid/axisforge-releases/releases/download/v$VERSION/AxisForge-$VERSION-x86_64.AppImage

# 3. Make it executable
chmod +x ~/Applications/AxisForge.AppImage

# 4. Run it
~/Applications/AxisForge.AppImage
```

First-launch banner will walk you through the udev / `input` group setup.
Two terminal commands, log out and back in, done.

### SteamOS / Steam Deck

Use `AxisForge-<version>-steamos-x86_64.AppImage` and follow the AppImage
steps above. It loads the system's Wayland library before the app
starts, which the plain AppImage does not do on SteamOS.

### Arch / Manjaro (AUR)

```bash
yay -S axisforge-bin
```

The PKGBUILD extracts the plain AppImage and installs the binaries + udev
rules + desktop entry into system paths. AppImageLauncher not required.

### Ubuntu / Debian (.deb)

Download `AxisForge_<version>_amd64.deb` from the Releases page, then:

```bash
sudo apt install ./AxisForge_<version>_amd64.deb
```

### Fedora / openSUSE (.rpm)

Download `AxisForge-<version>-1.x86_64.rpm` from the Releases page, then:

```bash
sudo dnf install ./AxisForge-<version>-1.x86_64.rpm      # Fedora
sudo zypper install ./AxisForge-<version>-1.x86_64.rpm   # openSUSE
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

Avoid layering the `.rpm` with `rpm-ostree install` unless you
specifically want the package on your base image: it works, but it adds a
reboot to each install and re-layering to every base update. The AppImage
path stays cleanly outside the OS image.

## Verifying downloads

Every release carries a `SHA256SUMS` file. Download it into the same
folder as the file you took, then verify before running:

```bash
sha256sum -c SHA256SUMS --ignore-missing
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
are licensed for personal use only — see the Terms of Service presented
on first launch. The AUR package also installs them under
`/usr/share/licenses/axisforge-bin/`.

Re-distribution of the AppImage / .deb / .rpm artifacts is not
permitted. Link to https://www.axisforge.app or to this Releases page
instead.
