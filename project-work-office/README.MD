# Office Workstation & VoIP Setup with PoE Passthrough Switch

## Problem
The manager's office required two VoIP phones and two computers to be connected, but there was only one RJ-45 wall port available.  
The VoIP phones required PoE, and the existing wall port supported PoE.

## Solution
- Purchased a 1-in-4-out PoE passthrough switch to expand the single RJ-45 connection
- Connected both VoIP phones to the switch to receive PoE from the wall port
- Used the passthrough ports on each phone to supply Ethernet (802.3) to the computers; the computers receive power externally
- Left two free RJ-45 ports on the switch for future expandability (e.g., printer or additional workstation)

## Outcome
- Two VoIP phones and two computers successfully connected through a single wall port
- Preserved PoE functionality for phones while providing network connectivity to computers
- Office now supports additional devices without extra cabling and allows for future expansion

## What I Learned
- Practical use of PoE and passthrough functionality
- Efficient network expansion using minimal infrastructure
- Planning for scalability in small office networks
- Integration of VoIP devices and workstations in a constrained environment

## Network Diagram
![Office VoIP PoE Diagram](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/project-work-office/office-voip-workstation-setup.jpg)
