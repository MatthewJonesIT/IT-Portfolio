# Plex Installation & NAS Media Integration

## Objective

This stage focuses on installing Plex Media Server and configuring it to access media stored on a seperate network storage device (NAS).

Because the libraries are not stored locally on the server, the NAS shares must be mounted as network drives before Plex can access them.

By the end of this stage the system will:

- Have Plex Media Server installed
- Mount NAS storage via SMB
- Allow Plex to access remote media libraries
- Enable remote streaming access

## Installing Plex Media Server

Plex Media Server was installed using the .deb package provided on the official Plex website.

Installation was performed using:

- "sudo dpkg -i plexmediaserver_1.19.4.2935-79e214ead_amd64.deb"

Once installed, Plex automatically starts its background service and opens port 32400, which hosts the Plex web interface.

The setup page can then be accessed via 192.168.0.26:32400/web

Because Plex has been previously configured on the NAS, the server was linked to an existing Plex account by logging in with existing credentials.

## NAS Storage Challenge

The media libraries are stored on a TerraMaster NAS running TOS4 rather than locally on the server.

TOS4 has some limitations with how shared folders are handled:

- Only top-level directories can be configured as shared folders
- All subfolders inherit the same share permissions

This means individual folders cannot easily be shared independently.

Although reorganising the NAS file structure could have simplified the setup, doing so would have:

- Broken existing mapped network drives on the primary workstation
- Required reorganising several terabytes of media
- Taken a significant amount of time to complete

Instead, the existing structure was preserved and the NAS directories were mounted directly on the server. 

Certain options had to be enabled on the NAS first to allow the shares to be accessed by the server, including enabling the SMB/CIFS file service, allowing the server IP to access the files and turning the folder into a shared folder.

![SMB](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/1.png)

![IP](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/2.png)

## Installing CIFS Utilites

To mount SMB shares on Linux, the CIFS utilites package must first be installed.

- "sudo apt install cifs-utils -y"

This package provides the tools required for mounting Windows/SMB network shares.

## Creating a Mount Point

A directory was created to serve as the mount location for the NAS share.

- "sudo mkdir -p /mnt/share/media"

This directory will act as the entry point where the NAS media storage becomes accessible within the Linux filesystem.

## Configuring Persistent Mount with fstab

To ensure the NAS share mounts automatically at boot, an entry was added to the filesystem table (fstab).

- "sudo nano /etc/fstab"

The following entry was initially added:

"# NAS Media
//192.168.0.251/Volume#1/User/admin/home/ /mnt/nas/media cifs -o username=XXXXXXX,password=XXXXXXX,iocharset=utf8,_netdev,file_mode=0775,dir_mode=0755 0 0"

However, when attempting to mount the share the system returned the following error:

- "cifs_mount failed w/return code= -13"

This error indicates that the NAS is denying authentication.

## Troubleshooting Authentication Issues

The issue was traced to the NAS account password containing special characters.

Certain characters can be interpreted by Bash as modifiers, preventing the password from being parsed correctly when included directly in the fstab entry.

To resolve this, a seperate credential file was created.

## Creating a Secured Credentials File

A credentials file was created in the root directory:

- "sudo nano /root/.nas-credentials"

The file contains the NAS login credentials:

![Login](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/6.png)

To protect the credentials, permissions were restricted so only the root user can read the file.

- "sudo chmod 600 /root/.nas-credentials"

## Updating fstab to Use the Credentials File

The fstab entry was then modified to reference the credentials file instead of embedding the password directly.

"# NAS Media
//192.168.0.251/Volume#1/User/admin/home/ /mnt/nas/media cifs credentials=/root/.nas-credentials,iocharset=utf8,_netdev,file_mode=0775,dir_mode=0755 0 0"

![fstab](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/7.png)

The configurateion was tested using:

- "sudo mount -a"

This successfully mounted the NAS share, making the media directories accessible from the server.

Plex was then able to detect the mounted directories and create the required media libraries.

![Drive](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/3.png)

## Remote Access Issue

Although Plex was working correctly on the local network, the server was not accessible remotely. 

The issue was traced to a port conflict.

Both the original NAS-based Plex server and the new server were attempting to use the default Plex port 32400.

Because the port was already in use by the NAS server, the new server was reconfigured to use a different port.

The Plex server port was changed to 32401. The router was then configured to forward port 32401 to the new server, allowing external access.

![Port](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/4.png)

This solution is temporary and will later be replaced by a reverse proxy configuration using Caddy, which will provide:

- HTTPS encryption
- Cleaner external access
- More secure exposure of services

## Plex Performance Test

One of the main reasons for swapping over from the NAS server to this dedicated server was performance, namely hardware transcoding.

Using the Plex web interface, hardware transcoding was enabled in the settings, then a test transcode was attempted.

The goal is to see (hw) after the transcoding marker when steaming at a different resolution or codec to the original media.

![Test](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/4-Plex-%26-NAS-Setup/5.png)

This was shown to be working without anymore configuration, providing a substantial performance increase compared to the previous NAS-based server.
## Summary

At this stage, Plex Media Server has been successfully installed and configured. 

Completed tasks:

- Plex Media Server installed
- NAS media storage mounted via SMB
- Secure credential management implemented
- Persistent mounts configured via fstab
- Plex libraries connected to NAS storage
- Temporary remote access configured via port fowarding

The server can now stream media both locally and remotely. 

The next stage of the project will focus on deploying the music streaming service.
