---
title: "Linux /dev/ttyUSB for FTDI devices of custom PID and VID"
tags: Tech
---

Usually if you connect a FTDI device to a Linux machine the FTDI driver in the Kernel recognises it and launches /dev/ttyUSB* so that you can have automatic UART output on the device.

This works with a huge lookup table in [ftdi_sio.c](http://lxr.free-electrons.com/source/drivers/usb/serial/ftdi_sio.c#L152) which has a list of known FTDI devices. If your device is not in there here's how you get it working nevertheless:

```bash
# Remove the module if currently loaded
rmmod ftdi_sio

# Load with your additional custom device
modprobe ftdi_sio vendor=0x123 product=0xABC
```

## Update
Newer versions of Linux don't accept the parameters anymore, so if `dmesg` shows the following messages

```bash
# dmesg | grep ftdi
ftdi_sio: unknown parameter 'vendor' ignored
ftdi_sio: unknown parameter 'product' ignored
```

try the following

```bash
modprobe ftdi_sio
echo "123 ABC" > /sys/bus/usb-serial/drivers/ftdi_sio/new_id
```
