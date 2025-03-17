# systemd-homed

#### Send home directory to another machine

If the machine trusts the home directory (same `local.private`, or `hostname.public`) then you just need to send / connect the home directory with everything else being automatic.

```shell
rsync --existing --inplace --sparse --progress $SOURCE:/home/nurah.home $TARGET:/home
```

Remove `--existing` for initial sync. For big home directories, it may be worthwhile doing a resize first:

```shell
homectl resize nurah 20G
```
