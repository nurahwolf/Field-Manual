# Arch Linux

## Troubleshooting

### unknown key '%INSTALLED_DB%' in local database

```shell
find /var/lib/pacman/local/ -type f -name "desc" -exec sed -i '/^%INSTALLED_DB%$/,+2d' {} \;
```

### signature from "XYZ" is unknown trust

If the keys actually are known to pacman, then you can ask pacman to refresh the keys:

```shell
pacman-key --refresh-keys
pacman -Sy
```

If not... Then you will want to manually get the keys or rebuild the pacman key store. An example (for CachyOS) could look like this:

```shell
rm -rf /etc/pacman.d/gnupg/
pacman-key --init
pacman-key --populate
pacman-key --recv-keys F3B607488DB35A47 --keyserver keyserver.ubuntu.com
pacman-key --lsign-key F3B607488DB35A47
```
### AUR Down / Unaccessible

In case the AUR is down, or cannot be accessed (network restrictions), there is a mirror hosted on github.

```bash
git clone --branch <package_name> --single-branch https://github.com/archlinux/aur.git <package_name>
```

### Arch Wiki

`arch-wiki-docs` and `arch-wiki-lite` can be used to access the wiki offline
