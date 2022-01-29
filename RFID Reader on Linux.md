# RFID Reader on Linux

## Prerequistes
Linux, unlike Windows, has no winscard low-level library. Any C# library depends on winscard will not work outside Windows.

This guide explains how to get RFID card readers working on Linux.

### Disable kernel modules
More details on [this link](https://stackoverflow.com/questions/31131569/unable-to-claim-usb-interface-device-or-resource-busy). 
Create a new file in `/etc/modprobe.d/blacklist-libnfc.conf`

```bash
sudo nano /etc/modprobe.d/blacklist-libnfc.conf
```

Add in following lines
```
blacklist pn533_usb
blacklist pn533
blacklist nfc
```
Changes will take effect on next startup.

If the kernel module is already running, disable the module
```bash
$ modprobe -r pn533_usb pn533 nfc
```

### Install pcscd and tools
`pcscd` is the smart card service daemon for Linux.

```bash
$ sudo apt install pcscd pcsc-tools
```

### The smart card reader
In the past, ACR122U is used for RFID reading over USB. Use `pcsc_scan` to test if `pcscd` and the card reader is working properly

### The C# library and NuGet package
See also [this link](https://www.instructables.com/ACR122U-NFC-C-Project/).

The NuGet package: [NFC-ACR122U](https://www.nuget.org/packages/NFC-ACR122U/)

## Problems
On Ubuntu Linux, at the time of writting, is on version 20.04.3, pcscd does not start succesfully on boot. Look out for that on Google and we'll see many forums talking about pcscd failing to start on not only Ubuntu, but also RedHat, Fedora, and on computers with other hardware, related to USB driver and stuff.

I've been tinkering with it, editing the service, adding restarts and no luck.

### Solution
I'm no Linux expert. I don't know how to fix the thing related to LIBUSB errors shown in journalctl and searches point to nothing. Tried everything I got in configuring the service unit file, and I gave up trying to do that, but came with a workaround, to check the service and start it if it hasn't started. I wrote a bash shell script, general purpose, not only for pcscd but can also be used for any other services. Check out [systemd-service-keepalive](https://github.com/seikosantana/systemd-service-keepalive).
