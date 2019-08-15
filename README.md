# rtl8811cu Based on rtl8821cu 
## Background
I bought an USB-wifi-adapter "**TP-Link TL-WDN5200H**" for my **ubuntu** server, whose core chip is "**RTL8811CU**" (ID: 0x0bda:0xc811) by The Realtek Corp., but this product only has official driver for **WINDOWS** system.

To make it work on **ubuntu**, I have searched for many relavent guides on the Internet, such as [Driver for rtl8811cu? - Ubuntu Forums](https://ubuntuforums.org/showthread.php?t=2389602) and [TP-LINK TL-WDN5200 usb wifi adapter install on ubuntu?](http://forum.ubuntu.org.cn/viewtopic.php?t=484598). But they didn't solve my problem until I found that "rtl8821cu driver source code contains some support for rtl8811cu" in [alteman's rtl8821cu repo](https://github.com/alteman/rtl8821cu/blob/master/os_dep/linux/usb_intf.c) as follow.

```
#ifdef CONFIG_RTL8821C
	/*=== Realtek demoboard ===*/
	...
	{USB_DEVICE_AND_INTERFACE_INFO(USB_VENDER_ID_REALTEK, 0xC811, 0xff, 0xff, 0xff), .driver_info = RTL8821C}, /* 8811CU */
	...
	/*=== Customer ID ===*/
#endif
```

I changed a little in Makefile of alteman's repo for compiling on i386-PC platform.

And finally, the compiled driver can drive my USB-wifi-adapter on ubuntu.

## How to do
### Install driver
copy the whole repo to your PC, enter (your_dir)/rtl8821cu/, open a terminal and type:
```
make
sudo make install
sudo modprobe 8821cu
```
the driver installed.
### Plug your USB-wifi-adapter into your PC
If wifi can be detected, congratulations.
If not, maybe you need to switch your device usb mode by the following steps in terminal:
1. find your usb-wifi-adapter device ID, like "0bda:1a2b", by type:
```
lsusb
```
2. switch the mode by type: (the device ID must be yours.)
```
sudo usb_modeswitch -KW -v 0bda -p 1a2b
```

It should work.

## Something more
I think this driver source code can be compiled for a series of Realtek wifi devices by minor changes with Makefile.
