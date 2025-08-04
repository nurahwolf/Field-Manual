# Network Manager CLI (nmcli)

## Commands

### Show Wireless Access Points

```h
nmcli device wifi list

*  SSID               			MODE    CHAN  RATE       SIGNAL  BARS  SECURITY
   NASA							Infra   6     54 Mbit/s  37      ▂▄__  WEP
*  NASA_CORP					Infra   11    54 Mbit/s  98      ▂▄▆█  WPA1
   NASA_GUEST					Infra   1     54 Mbit/s  62      ▂▄▆_  WPA2 802.1X
   ncsc							Infra   6     54 Mbit/s  29      ▂___  WPA1
   ncsc-but-cool				Ad-Hoc  6     54 Mbit/s  22      ▂___  --
   GCHQ DATA COLLECTOR			Infra   1     54 Mbit/s  19      ▂___  WEP
   FREE WIFI (NOT A SCAM)		Infra   1     54 Mbit/s  20      ▂___  WEP
   PREMIUM WIFI (20$ an hour)	Infra   4     54 Mbit/s  32      ▂▄__  WPA2
```
