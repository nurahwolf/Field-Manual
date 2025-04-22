# Windows as a virtual machine

```powershell
powercfg /h OFF #disable Hybernation / fast startup
compact /compactos:always #Enable CompactOS
wmic computersystem where name="%computername%" set AutomaticManagedPagefile=False #No page file
```
