# Gnome

#### Disable Gnome Extension Version Validation (Last updated 2025-03-09)

```shell
gsettings set org.gnome.shell disable-extension-version-validation true
```

#### Enable Adaptive Refresh (Last updated 2025-03-09)
[Arch Wiki - Variable Refresh Rate - GNOME](https://wiki.archlinux.org/title/Variable_refresh_rate#GNOME)

```shell
gsettings set org.gnome.mutter experimental-features "['variable-refresh-rate']"
```

#### Enable HDR (Last updated 2025-03-09)
[Arch Wiki - High Dynamic Range - GNOME](https://wiki.archlinux.org/title/HDR_monitor_support#GNOME)

**Requirements**

Ensure you have GNOME 48 beta or higher. Prior to 48 stable you may use Official_repositories#gnome-unstable.

Install `vk-hdr-layer-kwin6-git`.

---

Start GNOME with the `--debug-control` flag and enable `ColorManagementProtocol`:

```shell
gnome-shell --debug-control
dbus-send --session --print-reply --dest=org.gnome.Mutter.DebugControl /org/gnome/Mutter/DebugControl org.freedesktop.DBus.Properties.Set string:org.gnome.Mutter.DebugControl string:ColorManagementProtocol variant:boolean:true
```
HDR is now fully enabled. To use HDR see KDE#Games and KDE#Video on https://wiki.archlinux.org/title/HDR_monitor_support.

You can make this the default behavior by editing `/usr/lib/systemd/user/org.gnome.Shell@wayland.service`:
```shell
ExecStart=/usr/bin/gnome-shell --debug-control
ExecStartPost=/usr/bin/dbus-send --session --print-reply --dest=org.gnome.Mutter.DebugControl /org/gnome/Mutter/DebugControl org.freedesktop.DBus.Properties.Set string:org.gnome.Mutter.DebugControl string:ColorManagementProtocol variant:boolean:true
```

#### Use Seahorse (Passwords & Keys) as SSH ask pass (Last updated 2025-03-16)

```shell
# Temporary
export SSH_ASKPASS=/usr/lib/seahorse/seahorse-ssh-askpass

# Permanent
ln -s /usr/lib/seahorse/seahorse-ssh-askpass /usr/lib/ssh/ssh-askpass
```

#### TODO: Modify gdm with machinectl

Source: https://www.reddit.com/r/Fedora/comments/vqwype/machinectl_a_simpler_way_to_modify_gdm

 First, connect to the gdm user with bash as the shell. This will prompt you for your password.
machinectl shell gdm@ /bin/bash

Now you are in gdm's "userspace" and you can run normal gsettings commands to modify the interface. Below are some commands I have personally used to make gdm work how I want.

Change to 12hr time format
gsettings set org.gnome.desktop.interface clock-format '12h'

Enable tap-to-click
gsettings set org.gnome.desktop.peripherals.touchpad tap-to-click true

Enable night light
gsettings set org.gnome.settings-daemon.plugins.color night-light-enabled true

Enable numlock
gsettings set org.gnome.desktop.peripherals.keyboard numlock-state true
gsettings set org.gnome.desktop.peripherals.keyboard remember-numlock-state true

Disable Accessibility Icon
gsettings set org.gnome.desktop.a11y always-show-universal-access-status false

I hope this information is useful to the community!

EDIT: It may not actually be installed by default in Fedora. It is provided by the package systemd-container 
