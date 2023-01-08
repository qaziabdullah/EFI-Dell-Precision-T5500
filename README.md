# Dell Precision T5500 Hackintosh 
# This repo is for Ventura Experiments

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.8.4-green.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/)
[![GitHub issues](https://img.shields.io/github/issues/qaziabdullah/EFI-Dell-Precision-T5500.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/issues/)

### This repo should hopefully have all the fixes for Dell T5500 with RX570 and Xeon X5675 on macOS Ventura.
#### This should also work on T3500 system due to them being almost the same. Try and let us know

The EFI folder should not be used on older versions of macOS.


Testing on:

Model | Precision T5500
------------- | ---------------
CPU | Intel Xeon 5675
GPU | AMD Radeon RX 570 4GB (Sapphire) 
RAM | 32GB DDR3 1600Mhz
Storage | Samsung EVO 870 1TB
Ethernet | Broadcom 5761 Gigabit Ethernet (GbE) PCI-Express 
Software | macOS Ventura
BIOS | A17
Serial Port | Disabled
SATA Operation | AHCI
Bluetooth | Orico BCM20702 - CSR8510 didn't work

## What works?

-----

## What doesn’t work?

1. Sleep will be broken.
2. Disabled CPUPM, since X5675 doesn’t have any PM SSDT or I was unable to get it working but enabling CPUPM results in low freq and cpu doesn’t go at turbo freq. This is the same issue from monterey and will most likely stay with us for eternity.

## How To Use

Copy the EFI folder to your EFI partition. That's it.

## Kepler Card (UPGRADED TO RX570)

Check OLCP, I upgraded to Rx570 and working on it now.


## RTC

This will cause BIOS to reset after every shutdown. B*tch took long to get fixed. With Bios A17, the RTC fixes are added in config.plist.
However, with a different version of BIOS, the sectors might differ. Either Upgrade/Downgrade to A17 or find bad sectors if bios resets.
This will also get changed. F. I spent a lot of time fixing this.

## CpusTSCSync

It’s just required for sleep atm. Everything else isn’t affected because of this.
Wait for a newer version, possibly 1.10.0 as current version won’t let you boot.

## Bluetooth

The default SMBIOS is MacPro5,1. BluetoolFixup kexts and brcmpatchram things won't work. Change to iMac14,2. 
