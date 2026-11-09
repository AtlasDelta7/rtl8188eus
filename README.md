## Fork note: builds on Linux 6.13 – 6.18+

This fork adds compatibility fixes so the driver compiles on current kernels
(tested on **Raspberry Pi OS 6.18.60+rpt-rpi-v6** on an original Raspberry Pi 1, and
test-built on 6.17). Every change is wrapped in a kernel-version check, so older
kernels build the same code as before.

| Kernel | Change | Fix |
|---|---|---|
| 6.2+ | `del_timer_sync()` renamed | uses `timer_delete_sync()` |
| 6.13+ | `set_monitor_channel` gained a `net_device` argument | argument added |
| 6.14+ | `get_tx_power` gained `link_id` | argument added |
| 6.15+ | `EXTRA_CFLAGS` no longer read by kbuild (headers not found) | Makefiles use `ccflags-y` / `ldflags-y` |
| 6.16+ | `from_timer()` replaced | uses `timer_container_of()` |
| 6.17+ | `set_wiphy_params`, `set_tx_power`, `get_tx_power` gained `radio_idx` | argument added |

Version boundaries follow mainline Linux. Some distro kernels (e.g. Ubuntu's 6.8)
backport newer Wi-Fi APIs and may still need adjusting.

AP (hotspot) mode works with `hostapd`. NetworkManager's built-in hotspot did not
work reliably with this driver in testing. Used in
[pi1-offline-wiki](https://github.com/AtlasDelta7/pi1-offline-wiki).

---

## rtl8188eus v5.3.9


# THESE DRIVERS IS DEPRECATED.
# Use the mac80211 drivers over at [https://github.com/lwfinger/rtw88](https://github.com/lwfinger/rtw88)



# Realtek rtl8188eus &amp; rtl8188eu &amp; rtl8188etv WiFi drivers

[![Monitor mode](https://img.shields.io/badge/monitor%20mode-supported-brightgreen.svg)](#)
[![Frame Injection](https://img.shields.io/badge/frame%20injection-supported-brightgreen.svg)](#)
[![MESH Mode](https://img.shields.io/badge/mesh%20mode-supported-brightgreen.svg)](#)
[![GitHub issues](https://img.shields.io/github/issues/aircrack-ng/rtl8188eus.svg)](https://github.com/aircrack-ng/rtl8188eus/issues)
[![GitHub forks](https://img.shields.io/github/forks/aircrack-ng/rtl8188eus.svg)](https://github.com/aircrack-ng/rtl8188eus/network)
[![GitHub stars](https://img.shields.io/github/stars/aircrack-ng/rtl8188eus.svg)](https://github.com/aircrack-ng/rtl8188eus/stargazers)
[![GitHub license](https://img.shields.io/github/license/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8188eus/blob/master/LICENSE)<br>
[![Android](https://img.shields.io/badge/android%20(8)-supported-brightgreen.svg)](#)
[![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](#)


# Supports
* Android 12/13
* MESH Support
* Monitor mode
* Frame injection
* Up to kernel v6.5+
... And a bunch of various wifi chipsets

# Howto build/install
1. Compile and install the driver:
```
cd rtl8188eus
make && sudo make install
```
2. Blacklist another drivers in order to use this one:
```
echo 'blacklist r8188eu' | sudo tee -a '/etc/modprobe.d/realtek.conf'
echo 'blacklist rtl8xxxu' | sudo tee -a '/etc/modprobe.d/realtek.conf'
```
3. `reboot` or remove all drivers related to RTL8188 and reload this one:
```
rmmod r8188eu rtl8xxxu 8188eu
modprobe 8188eu
```

# MONITOR MODE howto
Use these steps to enter monitor mode.
```
sudo airmon-ng check kill
sudo ip link set <interface> down
sudo iw dev <interface> set type monitor
```
Frame injection test may be performed with
(after kernel v5.2 scanning is slow, run a scan or simply an airodump-ng first!)
```
sudo aireplay-ng -9 <interface>
```

# NetworkManager configuration
Add these lines below to "NetworkManager.conf" and ADD YOUR ADAPTER MAC below [keyfile]
This will make the Network-Manager ignore the device, and therefore don't cause problems.
```
[device]
wifi.scan-rand-mac-address=no

[ifupdown]
managed=false

[connection]
wifi.powersave=0

[main]
plugins=keyfile

[keyfile]
unmanaged-devices=mac:A7:A7:A7:A7:A7
```

# Credits
Realtek       - https://www.realtek.com<br>
Alfa Networks - https://www.alfa.com.tw<br>
aircrack-ng.  - https://www.aircrack-ng.org<br>
<br>
And all those who may be using or contributing to it of anykind. Thanks!<br>
