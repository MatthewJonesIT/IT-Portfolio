# Home Network Upgrade with Wired Backhaul

## Problem
Wi-Fi coverage was poor to non-existent in the house extension due to distance and an external wall.
As a result, smart home devices (Hive system / Google Home network) were unreliable and effectively unusable.

## Solution
- Installed powerline adapters to extend the wired network to the extension
- Added a switch in the garage to support multiple wired devices
- Connected an access point in AP mode
- Disabled DHCP on the AP
- Configured the same SSID and password as the main router to allow seamless client-based roaming
- Left wireless channels on automatic selection, as the main router supports 802.11ax with BSS Colouring, making manual channel tuning unnecessary in this environment
- Connected the Hive Hub to the switch to allow the Hive system to communicate with the boiler in the garage via Zigbee

## Outcome
- Stable Wi-Fi coverage throughout the house
- Devices connect automatically to the strongest access point
- Hive system now works correctly with Zigbee and Ethernet connectivity

## What I Learned
- Network design and troubleshooting
- Difference between AP mode and extender mode
- DHCP management in multi-device networks
- Integrating wired, wireless, and IoT systems

## Network Diagram

![Home Network Diagram](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/project-home-network/home-network-diagram.jpg)
