# mount

#### ID mapping

I quite enjoy `systemd-homed`, and its really nice that it does `idmapping` automatically.
However, sharing files between homes can be a pain at times, especially when its not a good practice to make a generic `users` group and add everyone to that.

So, a way to get around that is by using `idmapping`. Note that this is a kernel 5.15 and above feature!

A good example is the below:

```shell
systemd-mount -o X-mount.idmap=65534:(id -u):1,subvol=games /dev/mapper/big_ssd_for_games /var/games
```

In particular, it maps the nobody (`65534`) user to the user who ran the command (`id -u`). This mount command is more specific to `btrfs`, where it is then mount a particular subvolume.
