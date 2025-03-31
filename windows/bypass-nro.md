# Bypass NRO

TL:DR: Spawn a terminal with SHIFT+F10 and type the following

```shell
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\OOBE /v BypassNRO /t REG_DWORD /d 1 /f
shutdown /r /t 0
```

Previously, you were able to do `bypassnro.cmd` during OOBE. This has been removed as of the following insider version:

https://blogs.windows.com/windows-insider/2025/03/28/announcing-windows-11-insider-preview-build-26200-5516-dev-channel
