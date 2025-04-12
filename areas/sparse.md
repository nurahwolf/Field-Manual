# Sparse Files

Notes and useful information about sparse files.

### Converting a file to sparse

This is a process of digging holes (changing a `0` to a `seek` or `skip`).
Generally, `fallocate` is a good way to do it, as holes can be dug in place, though there are other tools that can do the same.

For example, below is the comparison of a 1TB GPT disk image holding a Windows 11 image:

```shell
fallocate -v --dig-holes WIN11-sparse.img
dd if=WIN11.img of=WIN11-dd.img conv=sparse status=progress


du -sh --apparent-size *
932G    WIN11-dd.img
932G    WIN11-sparse.img
932G    WIN11.img

du -sh *
653G    WIN11-dd.img
653G    WIN11-sparse.img
932G    WIN11.img
```

`fallocate` took 7m8s while `dd` took 20m59s to complete.
