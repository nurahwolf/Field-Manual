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

#### Keep Directory Size Small

Allow discards to trim any data that is deleted, and let the container shrink and grow as needed.

```shell
homectl update nurah --auto-resize-mode=shrink-and-grow --luks-discard=true --luks-offline-discard=true
```

Note that if you are sending the home file to a system with a smaller root partition, you might want to do a resize first.
