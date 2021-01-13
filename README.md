# **Hackintosh for Lenovo Thinkpad X1 Extreme Gen II**

## Current OC Version: 0.6.5

##### This README is just a modified version for my X1 Extreme Gen 2 based on [1Revenger1's Gen 1](https://github.com/1Revenger1/X1-Extreme-OpenCore-Resources#what-works)

I have seen too many guides for this machine on Clover but little to zero information about X1 Extreme Gen 2 on OpenCore, so I decided to upload my EFI folder after I got it working pretty fine here using OpenCore. I will also try to maintain this repo as much as I can (In case I forgot but if you follow the guide you should just need this repo for reference)

Reason why you should use OpenCore can be found here: https://dortania.github.io/OpenCore-Install-Guide/why-oc.html

#### **This repo is meant to be used as a source of information more than just copy and use it, so please make your own version of SSDT! If you have any issue preparing your files, please join the Hackintosh Discord channel for help!**

I'd strongly suggest reading and follow https://dortania.github.io/OpenCore-Install-Guide/ (Dortania's OpenCore Guide) for pre-installation, and post-installation.

### **What works**

-   Booting to Windows and MacOS using OpenCore.
    -   Bootcamp is installed mainly for time consistency between MacOS and Windows
-   Sleeping in MacOS
    -   about 7% battery over 6 hrs (not sure if it's bad or not, don't really matter to me since I plug it in a lot of the time)
-   Backlight Control
-   Function keys (Using ThinkPad Assistant)
-   USB
-   Intel Wifi and Bluetooth
-   Camera/Mic
-   Function Keys
-   Keyboard and Trackpad (Before patching battery your trackpad will be recognized as mouse.)

### **Haven't Tested/Tried to fix**

-   Choosing starting up disk
    -   You could just reboot and choose boot option in OC
-   Booting to Linux using OpenCore:
    -   You could also just change boot order when you want to use Linux on boot. Sure it takes a minute or two but I haven't really tried to fix it
-   SD Card Reader
-   Thunderbolt 3 & USB-C
-   CFG Lock
    -   Honestly accessing anything on Lenovo's BIOS is a pain in the a\*\* since everything is hard lock. I even tried booting RE.efi but it just gave me black screen.
-   **Only onboard Mic detected**, even when I used earphones and Headphones that have mic, probably not gonna bother figure it out since if I have important calls I'll just use phone or switch to Windows if needed for video call.
-   Macs fan control can detect temp but cannot detect fan (might try reading to fix it but honestly the temp on Mac is pretty good compare to Windows so I might not bother)

## Specs

-   I7-9850h
-   GTX 1650 Max-Q
-   OEM Samsung m.2 SSD (SM 981) for Windows + EFI folder for Windows and Linux
-   Sabrent 2TB m.2 SSD - Boot disk for OC/MacOS
-   4k screen
-   Camera w/ Shutter
-   Intel AX200 Wifi card
    -   X1 Extreme Gen 2 has WLAN Whitelist, so don't bother buying a Broadcom wifi card
-   Intel Ethernet Connection I219-LM
-   CX8070 Analog Audio (Found it on Linux)

## SSDTs

-   AWAC: Fixing System Clocks
-   BATT: Battery (Used Battery Patcher and lots of help from [1Revenger1](https://github.com/1Revenger1))
-   dGPU-Off: Disables dGPU only in MacOS (still works in Windows and Linux)
-   GPI0: Fixing Trackpad
-   HPET: Fixing IRC Conflicts
-   PLUG: Fixing CPU Power Management
-   PMC: Fixing NVRAM
-   PNLFCFL: Fixing Backlight
-   RHUB: Fixing USB
    -   From Dortania's guide only 11th gen CPU need it but somehow without it MacOS doesn't recognize any USB Port evem with USBInjectAll Kext.
-   RMFC-PS2Map-LenovoIWL: Found it from other Clover users, use it to have map your screenshot button as F13 then you can assign it in MacOS for screenshot
-   SBUS-MCHC: Fixing SMBus support
    -   Honestly this is a part of [Fixing Sleep](https://dortania.github.io/OpenCore-Post-Install/universal/sleep.html#smbus), I don't actually know if it's needed since I went over fixing sleep section before even testing if it sleeps properly, you might not need it.
-   USBX: Fixing Embedded Controller
    -   Since my machine doesn't need to create SSDT-EC, I only need USBX for it.
-   X1C6-KBRD: Mapping function keys (F4-F12). Full installation can be found [here](https://github.com/MSzturc/ThinkpadAssistant)

## Device Properties

-   **iGPU**: Took me the longest time in the whole installation process since I've tried everything to fix the legendary error [IOConsoleUsers: gIOLockState (3...)](https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/extended/userspace-issues.html#stuck-on-or-near-ioconsoleusers-gioscreenlock-giolockstate-3)
    -   Turns out you need this to work (Found it by trying to convert a Clover repo to OC)
    ```
    framebuffer-unifiedmem: 00000080
    ```
    -   Other values are for dGPU to be disabled and for 4k display.\
-   Audio: depends on your layout, check [this](https://dortania.github.io/OpenCore-Post-Install/universal/audio.html#fixing-audio-with-applealc) for your device.
-   Keyboard Wake issues
    -   It's for fix sleep, I put it there just because I don't want to spend lots of time findout which can cause sleep issue.

## Kexts

-   Airportitlwm: Intel Wifi kexts. Full information can be found [here](https://openintelwireless.github.io/itlwm/)
-   AppleALC: Audio
-   CPU Friend: Patches CPU Data
-   CPU FriendDataProvider: provide Data for CPUFriend, made from CPUFriendFriend by CorpNewt
-   HibernationFixup: Hibernation
-   Zxystd's IntelBluetoohFirmware & IntelBluetoothInjector: Bluetooth functionality
-   IntelMausi: For ethernet
-   Lilu: Required Kext for many other kexts
-   NVMEFix: Fixes NVME power draw and power management
-   SMCBatteryManager: Battery Status
    -   You need battery patch before using this, otherwise you will stuck in pre-install for awhile.
-   SMCProcessor: Processor Info
-   SMCSuperIO: Fan speed/temperature
-   USBMap: Fix remapping USB properly ([I recommend to follow this.](https://dortania.github.io/OpenCore-Post-Install/usb/))
-   VirtualSMC: Fakes SMC
-   VoodooPS2Controller: Keyboard
-   VoodooRMI + VoodooSMBus: Trackpad
    -   Synaptics SMBUS trackpad driver
    -   VoodooPS2 version 2.2 and VoodooRMI only require VoodooPS2Mouse to be disabled
    -   Reliable gestures
    -   on Big Sur, tap to touch on VoodooRMI 1.2 requires hard tap while on VoodooRMI 1.3 can be too sensitive (Not sure why, hopefully it's fixed soon)
    -   On very rare cases, on bootup or after sleeping does stop responding - unknown reason
    -   If you find either one of the two above issues was too annoyed, just use VoodooPS2Controller only. It has worse 4 finger gesture but does not have either issues.
-   Whatevergreen: Graphic Patching
    -   Since updating to Big Sur some vert rare cases boot-up can cause screen really dark, but reboot fix the issue
-   XHCI-unspported: Probably not needed, but I just have it anyways for now.

## Other files on Drivers/Resources:

Those are just cosmetic files for GUI and Boot-chime, details can be found [here](https://dortania.github.io/OpenCore-Post-Install/cosmetic/gui.html).
