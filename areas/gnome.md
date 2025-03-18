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
