# Archer T4U Plus Driver for Linux (rtl8822bu)
### _Finally_, one that (_hopefully_) doesn't randomly disconnect.
I'm sorry that I am even the one making this driver for this finniky ass device with **UNBELIEVABLY** no documentation available online about the underlying radio chipset. Almost every single repository/blog that describes this chip, describes it as an rtl8812au. The only driver that has worked on it with some tweaking (the one this is from), references it as an rtl8822bu. This has to be much closer to the truth than the prior option, but there are still some glitches that are incredibly hard to track down because the code currently doesn't mark them as glitches. The other problem is that TP-Link upgraded the T4U-Plus from `v1.0` to `v1.3` to `v1.8`. I have no idea what they changed between these versions; nobody does. It's very possible that they correspond to the `rtl8822au`, `rtl8822bu`, and `rtl8822cu` chipsets respectively, but I'm guessing.

The chipset is known to have a problem with overheating. This isn't a problem of the chipset, _really_, as it's just doing a burst-mode-like operation strategy. Power trimming (`rtw_pwrtrim_enable`) is the first thing that really helps here from what I can tell. It enables the system to stop going over its heat limit, but because of that bursting nature it's always going to be a bit of a problem.

<br/>

## Linux 5.18+ and RTW88 Driver
I'm just blacklisting it. I straight up don't want to deal with my WiFi radio
failing anymore. Check out `archer-t4u.conf` in the repo's root directory.

## Technically Supported Devices
<details>
  <summary>
    TP-Link
  </summary>

* TP-Link Archer T4U Plus
</details>



Or anything with an 8822bu chipset.

# How to use this kernel module
* Ensure you have C compiler & toolchains, e.g. `build-essential` for Debian/Ubuntu, `base-devel` for Arch, etc.
* Make sure you have installed the corresponding kernel headers
* All commands need to be run in the driver directory
* You need rebuild the kernel module everytime you update/change the kernel if you are not using DKMS


## Installing
Elevate your permissions with `sudo` or `doas` or whatever, then from the root
of the repository do:
```
cp -r ./ /usr/src/rtl88x2bu-t4u
dkms add -m rtl88x2bu -v t4u
dkms autoinstall
```
### Building module for other kernels
You should know this is for advanced users only,
**please don't just blindly copy paste this thinking it's part of the install process**.
```
make KSRC=/lib/modules/YOUR_KERNEL_VERSION/build
```

## Uninstalling
Elevate your permissions with `sudo` or `doas` or whatever, then from the root
of the repository do:
```
rm -r /usr/src/rtl88x2bu-t4u
dkms uninstall -m rtl88x2bu -v t4u
```

# USB 3.0 Support
You can try use `modprobe 88x2bu rtw_switch_usb_mode=1` to force the adapter run under USB 3.0. But if your port/motherboard not support it, the driver will be in restart loop. Remove the parameter and reload the driver to restore. Alternatively, `modprobe 88x2bu rtw_switch_usb_mode=2` let\'s it run as USB 2 device.

Notice: If you had already loaded the moduel, use `modprobe -r 88x2bu` to unload it first.

If you want to force a given mode permanently (even when switching the adapter across devices), create/edit the file `/etc/modprobe.d/99-RTL88x2BU.conf` with the following content:
`options 88x2bu rtw_switch_usb_mode=1`


# Debug
Set debug log use `echo 5 > /proc/net/rtl88x2bu/log_level` or `modprobe 88x2bu rtw_drv_log_level=5`
