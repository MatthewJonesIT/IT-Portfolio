# Hardware Assembly

## Objective

This section documents the unboxing, initial inspection, and setup of the Dell OptiPlex 3060 Micro for the Multimedia Server project. It covers hardware verification, installation of additional storage, and BIOS configuration to prepare the system for Ubuntu Server deployment.

## Unboxing & initial Impressions

![Unboxing](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/2-Hardware-Assembly/01.jpeg)

The Dell OptiPlex 3060 Micro arrived as a compact, enterprise-grade system. Its small footprint is ideal for a quiet, 24/7 home server setup.
The case features a tool-less design, secured with a single thumb screw on the top panel. Removing the panel exposes the internal components, allowing easy access to the storage bays and M.2 slots.

![Top Off Case](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/2-Hardware-Assembly/02.jpeg)

One of the M.2 slots was occupied by a drive, the other was empty but labelled for WLAN which could open the doors for interesting projects in the future.

## Storage Inspection & Installation

Existing Configuration:
- M.2 Slot: Occupied by a 128GB SSD (planned as OS drive)
- 2.5" Sata Drive Bay: Empty, toolless design

Installation
A 2.5" SATA HDD was installed into the empty bay without requiring tools. The drive will later host the Navidrome music library and serve as an incremental backup target for the primary workstation music library.

![HDD](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/2-Hardware-Assembly/03.jpeg)
![HDD In Tray](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/2-Hardware-Assembly/04.jpeg)
![Assembled Machine](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/2-Hardware-Assembly/05.jpeg)

The case was then reassembled ready for the initial boot.

## Initial BIOS Configuration

A keyboard, monitor and Ethernet cable were connected to the system to access the BIOS.

Key steps included:
1) Drive Verification:
- 128GB M.2 SSD correctly recognised
- 1000GB 2.5" HDD correctly recognised
2) Intel Quick Sync Activation:
Intel Quick Sync was enabled in the BIOS. This will allow hardware-accelerated transcoding in Plex, reducing CPU load for video streaming.
3) Other Checks:
- Gigabit Ethernet detected
- Default boot order verified for planned OS installation via USB

![BIOS](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/2-Hardware-Assembly/06.jpeg)

## Summary

The dell OptiPlex 3060 Micro has been successfully unboxed, inspected, and prepared for Ubuntu Server installation. All drives are recognised, and key hardware features, including Intel Quick Sync, are enabled.

Next steps include
- installing headless Ubuntu Server
- Configuring SSH and storage mounts
- Preparing the system for Plex and Navidrome service deployment.



