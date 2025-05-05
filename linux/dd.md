# DD

### DD Remote Disk to Local

```shell
# NOTE: Direct root access both sides! Local -> Remote; Remote -> Local
dd if=/dev/<DISK> | gzip -1 - | ssh <TARGET> dd of=<OUTPUT>.gz
ssh <TARGET> "dd if=/dev/<DISK> | gzip -1 -" | dd of=<OUTPUT>.gz
```
It is recommended to combine the flag `status=progress` to know how the transfer is going, and buffer size `bs=64k` for better speed.
Alternatively you can do `pkill -USR1 dd`.

Pipe Viewer (`pv`) is also a nice method to monitor progress. Just add it to the chain:
`dd if=/dev/<DISK> | gzip -1 - | pv | ssh <TARGET> dd of=<OUTPUT>.gz`

Consider adding yourself to the `disks` group to directly modify block devices with something like `usermod -aG <USER> disk`!

Otherwise, a very questionable work around to abuse sudo passwordless:
```
cat /etc/sudoers/10-dd.conf

<USER> ALL=(ALL) NOPASSWD: /bin/dd if=/dev/<TARGET>
```
