# Windows as a virtual machine

```powershell
powercfg /h OFF #disable Hybernation / fast startup
compact /compactos:always #Enable CompactOS
wmic computersystem where name="%computername%" set AutomaticManagedPagefile=False #No page file
```

## VFIO Optimisations

Make sure to follow the latest guidance on https://looking-glass.io/docs/B7 as well. I'm planning to build a script to make some of this a little easier.
