# systemd-cryptsetup

## Troubleshooting

### Failed to unseal secret using TPM2: State not recoverable

This means that the TPM is in lockout mode.

TODO: How to identify lockout mode?

You can reset the TPM (this will wipe all secrets) with the following snippet:

```shell
echo 5 | tee /sys/class/tpm/tpm0/ppi/request
```
