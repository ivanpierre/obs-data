# Enable snaps on Arch Linux

## Enable snaps on Arch Linux and install Red - Youtube Client for Linux

Snaps are applications packaged with all their dependencies to run on all popular Linux distributions from a single build. They update automatically and roll back gracefully.

Snaps are discoverable and installable from the [Snap Store](https://snapcraft.io/store), an app store with an audience of millions.

![](https://res.cloudinary.com/canonical/image/fetch/f_auto,q_auto,fl_sanitize,w_169,h_159/https://assets.ubuntu.com/v1/feca0fc0-Distro_Logo_ArchLinux.svg)

### Enable snapd

On Arch Linux, snap can be installed from the [Arch User Repository (AUR).](https://aur.archlinux.org/packages/snapd/) The [manual build process](https://wiki.archlinux.org/index.php/Arch_User_Repository#Installing_packages) is the Arch-supported install method for AUR packages, and you’ll need the [prerequisites](https://wiki.archlinux.org/index.php/Arch_User_Repository#Prerequisites) installed before you can install any AUR package. You can then install snap with the following:

```
git clone https://aur.archlinux.org/snapd.git
cd snapd
makepkg -si
```

Once installed, the _systemd_ unit that manages the main snap communication socket needs to be enabled:

```
sudo systemctl enable --now snapd.socket
```

To enable _classic_ snap support, enter the following to create a symbolic link between `/var/lib/snapd/snap` and `/snap`:

```
sudo ln -s /var/lib/snapd/snap /snap
```

Either log out and back in again, or restart your system, to ensure snap’s paths are updated correctly.