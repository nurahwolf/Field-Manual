# CachyOS

### Zen 4+ repos - 2025-03-16
https://discuss.cachyos.org/t/zen-4-5-optimized-repository-testing/713

At time of writing, supports the features: `abm, adx, aes, avx512bf16, avx512bitalg, avx512ifma, avx512vbmi, avx512vbmi2, avx512vnni, avx512vpopctndq, clflushopt, clwb, clzero, fsgsbase, gfni, mwaitx, pclmul, pku. prfchw, rpdid, rdrnd, rdseed, sha, sse4a, vaes, vockmulqdq, wbnoinvd, savec, xsaveopt, xsaves`

```
[cachyos-znver4]
Include = /etc/pacman.d/cachyos-v4-mirrorlist
[cachyos-core-znver4]
Include = /etc/pacman.d/cachyos-v4-mirrorlist
[cachyos-extra-znver4]
Include = /etc/pacman.d/cachyos-v4-mirrorlist
[cachyos]
Include = /etc/pacman.d/cachyos-mirrorlist
[core]
Include = /etc/pacman.d/mirrorlist
[extra]
Include = /etc/pacman.d/mirrorlist
[multilib]
Include = /etc/pacman.d/mirrorlist
```

Support can be checked with `gcc -march=native -Q --help=target 2>&1 | grep -Po "^\s+-march=\s+\K(\w+)\$"` for if it can be enabled.

To force a package update when changing repos:

```shell
pacman -Scc								# Clear ALL packages
pacman -Sy								# Update Repositories
pacman -Qqn | pacman -S -	# Query list of installed pkacages, then reinstall them
```
