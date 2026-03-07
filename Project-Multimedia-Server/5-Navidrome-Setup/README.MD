# Navidrome Installation & Music Synchronisation

## Objective

This stage focuses on depoloying Navidrome, a lightweight self-hosted music streaming server, and configuring an automated process to synchronise the music library from the primary workstation.

By the end of this stage the system will:

- Host a dedicated music library on the server
- Allow initial transfer of music files from the workstation
- Automate incremental backups of the workstation music library
- Deploy Navidrome to stream the music collection locally and remotely

## Preparing the Music Storage

First, a destination directory was created for the music library on the server.

-"mkdir -p /home/matt/Music"

This folder will store the full music collection and will later be used by Navidrome as its media library.

## Initial Music Transfer via Samba

The easiest way to perform the initial transfer of music from the workstation to the server was using the Windows GUI.

To enable this, the server directory was shared over the network using Samba. 

The process started out by installing Samba on the server.

- "sudo apt install samba"

## Configuring Samba

The Samba configuration file needed to be edited to allow the directory to be shared via SMB.

- "sudo nano /etc/samba/smb.conf"

The following section was added to the bottom of the file:

