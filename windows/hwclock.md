# Windows - Hardware Clock

https://wiki.archlinux.org/title/System_time#UTC_in_Microsoft_Windows

TL:DR, if dual booting windows and windows time continues to be fucky, set windows to use UTC time for the hwclock.

```reg
reg add "HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation" /v RealTimeIsUniversal /d 1 /t REG_DWORD /f
```
