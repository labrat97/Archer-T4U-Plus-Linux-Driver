# Archer T4U Plus Driver for Linux (rtl8822bu)
### _Finally_, one that doesn't randomly disconnect.

<br/>

## Linux 5.18+ and RTW88 Driver
I'm just blacklisting it. I straight up don't want to deal with my WiFi radio
failing anymore. Check out `archer-t4u.conf` in the repo's root directory.

## Technically Supported Devices
<details>
  <summary>
    ASUS
  </summary>

* ASUS AC1300 USB-AC55 B1
* ASUS U2
* ASUS USB-AC53 Nano
* ASUS USB-AC58
</details>

<details>
  <summary>
    Dlink
  </summary>

* Dlink - DWA-181
* Dlink - DWA-182
* Dlink - DWA-183 D Version
* Dlink - DWA-185
* Dlink - DWA-T185
</details>

<details>
  <summary>
    Edimax
  </summary>

* Edimax EW-7822ULC
* Edimax EW-7822UTC
* Edimax EW-7822UAD
</details>

<details>
  <summary>
    NetGear
  </summary>

* NetGear A6150
</details>

<details>
  <summary>
    TP-Link
  </summary>

* TP-Link Archer T3U
* TP-Link Archer T3U Plus
* TP-Link Archer T3U Nano
* TP-Link Archer T4U V3
* TP-Link Archer T4U Plus
</details>

<details>
  <summary>
    TRENDnet
  </summary>

* TRENDnet TEW-808UBM
</details>

<details>
  <summary>
    ZYXEL
  </summary>

* ZYXEL NWD6602
</details>


And more.

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
cp ./archer-t4u.conf /etc/modprobe.d/.
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
rm /etc/modprobe.d/archer-t4u.conf
```

# USB 3.0 Support
You can try use `modprobe 88x2bu rtw_switch_usb_mode=1` to force the adapter run under USB 3.0. But if your port/motherboard not support it, the driver will be in restart loop. Remove the parameter and reload the driver to restore. Alternatively, `modprobe 88x2bu rtw_switch_usb_mode=2` let\'s it run as USB 2 device.

Notice: If you had already loaded the moduel, use `modprobe -r 88x2bu` to unload it first.

If you want to force a given mode permanently (even when switching the adapter across devices), create the file `/etc/modprobe.d/99-RTL88x2BU.conf` with the following content:
`options 88x2bu rtw_switch_usb_mode=1`


# Debug
Set debug log use `echo 5 > /proc/net/rtl88x2bu/log_level` or `modprobe 88x2bu rtw_drv_log_level=5`
