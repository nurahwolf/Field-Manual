# USB/IP

Refer to https://wiki.archlinux.org/title/USB/IP for the most up to date info.

### MacOS / Windows Server

https://github.com/jiegec/usbip is quite a nice rust based project for exposing USB devices over the network.
One rather nice usecase is to 'forward' a YubiKey or other security key, so that it can be used 'remotely' over the network.

## Arch Wiki (Last Updated 2025-05-05)

The USB/IP Project aims to develop a general USB device sharing system over IP network. To share USB devices between computers with their full functionality, USB/IP encapsulates "USB I/O messages" into TCP/IP payloads and transmits them between computers.

The package exists in the standard repos (on arch). Get it with `usbip`.

### References
* [Arch Wiki - USB/IP](https://wiki.archlinux.org/title/USB/IP)
* [Official USB/IP project site](https://usbip.sourceforge.net)
* [Linux Kernel "README for usbip-utils"](https://www.kernel.org/doc/readme/tools-usb-usbip-README)
* [How To Setup and use USB/IP](https://developer.ridgerun.com/wiki/index.php?title=How_to_setup_and_use_USB/IP)
* [USB/IP for Windows](https://github.com/cezanne/usbip-win)
* [Rust based, cross platform USB/IP](https://github.com/jiegec/usbip)
* `man|8|usbip`
* `man|8|usbipd`
