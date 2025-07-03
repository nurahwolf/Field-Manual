# Libvirt - USB

#### Make passed through USB device optional

Set `startupPolicy='optional'` on `source` to make a device optional (not required) for the VM to start.

```xml
<hostdev mode='subsystem' type='usb' managed='yes'>
  <source startupPolicy='optional'>
    <vendor id='0x054c'/>
    <product id='0x05c4'/>
  </source>
  <address type='usb' bus='0' port='1'/>
</hostdev>
```
