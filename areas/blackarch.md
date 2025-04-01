# Blackarch

#### signature from "Levon 'noptrix' Kayan (BlackArch Developer) <noptrix@nullsecurity.net>" is unknown trust

Locally sign Levon's key for now, then pull down `blackarch-keyring`

```shell
pacman-key --lsign-key F9A6E68A711354D84A9B91637533BAFE69A25079
```

NOTE: if you get the following error and install fails, set gpg to ignore weak sha1

```
gpg: Note: third-party key signatures using the SHA1 algorithm are rejected
gpg: (use option "--allow-weak-key-signatures" to override)
```

set `allow-weak-key-signatures` in `/etc/pacman.d/gnupg/gpg.conf`.
