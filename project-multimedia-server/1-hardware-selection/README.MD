# Hardware Selection & Design Rationale

## Objective

The goal of this project is to design and deploy a self-hosted home media server capable of streaming both video and music libraries locally and remotely.

- Video streaming: Plex
- Music streaming: Navidrome
- Mobile client: Symfonium (or any Subsonic capable streaming client)

The system must:
-Support up to 4 simultaneous Plex streams
-Support hardware transcoding
-Run 24/7
-Maintain low power consumption
-Remain quiet and compact
-Integrate with existing NAS storage

## Current Environment & Limitations

My current Plex server runs on a TerraMaster NAS with:
- 1.5GHz dual-core CPU
- 1GB RAM

This configuration struggles with transcoding and provides inconsistent performance under load. Software transcoding is CPU-intensive, and the existing hardware is not suitable for multiple concurrent streams.
While Navidrome is extremely lightweight (and could run comfortably on a Raspberry Pi), Plex introduces significantly higher compute requirements, particularly when transcoding is required for remote playback.

## Requirements Definition

Before selecting hardware, I defined the following constraints:
- Minimum 2–4 concurrent hardware transcodes
- Intel Quick Sync support
- Gigabit networking
- Expandable storage
- Low idle power draw (24/7 operation)
- Small form factor
- Budget-conscious (target under £150)

## Hardware Options Considered
1️) Raspberry Pi

Pros:
- Extremely low power usage
- Silent
- Compact

Cons:
- Weak or non-existent hardware transcoding support for Plex
- Would rely on software transcoding
- Poor value relative to performance gains

Decision: Rejected due to insufficient Plex performance.

2️) Brand New Mini PC (e.g., Intel NUC)

Pros:
- Excellent performance per watt
- Compact and quiet
- Modern CPU architectures

Cons:
- High upfront cost for this use case

Decision: Rejected due to budget constraints.

3️) Custom-Built PC

Pros:
- Maximum flexibility
- Upgrade potential

Cons
- Higher power draw
- Larger physical footprint
- Increased cost
- Overkill for defined requirements

Decision: Rejected due to efficiency and cost considerations.

## Final Decision: Refurbished Business Micro PC

Selected: Dell OptiPlex 3060 Micro

Rationale:
- Enterprise-grade reliability
- Designed for continuous office operation
- Small form factor
- Low power consumption
- Strong second-hand market value
- Significantly cheaper than new mini PCs

This approach achieves approximately 90–95% of the performance of a modern mini PC at roughly 25% of the cost.

## Hardware Specifications & Justification
CPU:
- Intel Core i3-8100T
- 4 cores / 4 threads
- 3.1GHz base clock
- Intel Quick Sync support

The key requirement is Intel Quick Sync Video, enabling hardware-accelerated H.264/H.265 transcoding within Plex. This dramatically reduces CPU load compared to software transcoding and should comfortably support 2 simultaneous hardware transcodes, alongside additional direct-play streams.

Memory:
- 8GB DDR4 RAM

A substantial improvement over the previous 1GB system and more than sufficient for Plex, Navidrome, and reverse proxy services.

Networking:
- Gigabit Ethernet

More than adequate for multiple simultaneous local and remote streams.

Storage:
- 128GB M.2 SSD (OS drive)
- 2.5" SATA expansion bay

The OS will reside on the M.2 SSD.
The 2.5" drive bay will host the Navidrome music library and serve as an on-site backup of the primary music collection stored on my main workstation.

Video media will remain hosted on the existing NAS and mounted over the network.

## Operating System Strategy

The system ships with Windows 11 Home, which will be removed.

I will deploy a headless installation of Ubuntu Server to:
- Reduce resource overhead
- Improve stability for 24/7 uptime
- Enable SSH-based remote administration
- Provide a clean foundation for containerized services (future enhancement)

After initial configuration, the server will operate entirely via SSH from my primary workstation.

## Remote Access & Security Design

While Plex supports UPnP-based port forwarding, I prefer a more controlled and secure configuration.

To securely expose services externally I shall utlize caddy to create a reverse proxy

This will allow for automatic HTTPS with SSL certificates and controlled port exposure.

Caddy will handle TLS termination and reverse proxy routing for externally accessible services, ensuring encrypted remote access without exposing unnecessary ports directly.

## Backup Strategy

The internal 2.5" drive will serve dual purposes:
- Navidrome music library
- Incremental backup target for my primary workstation's music library

Incremental backups will be configured to:

- Automatically sync new media on a scheduled basis
- Minimise network overhead
- Reduce unnecessary disk writes
- Avoid full-library transfers after initial seeding

## Summary

This build balances:
- Performance
- Power efficiency
- Cost
- Expandability
- Reliability

By selecting refurbished enterprise hardware with Intel Quick Sync support, the system meets defined streaming requirements while remaining efficient enough for continuous 24/7 operation.
