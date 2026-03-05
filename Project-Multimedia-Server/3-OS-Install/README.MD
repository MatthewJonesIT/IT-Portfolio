# Operating System Installation & Initial Configuration

## Objective

This stage focuses on deploying the operating system to the server hardware, configuring the storage layout, and enabling remote management via SSH.

By the end of this step the system will:
- Have Ubuntu Server installed on the primary SSD
- Mount /home to the secondary HDD
- Be fully updated
- Allow remote administration from the main workstation via SSH

## Creating installation Media

The first step was to download the latest Ubuntu Server ISO from the official Ubuntu website.

This ISO image was then written to a USB drive using Rufus, creating a bootable installation medium.

Steps:
- Download the latest Ubuntu Server ISO
- Insert a USB flash drive above 8GB in size
- Use Rufus to write the ISO to the USB
- Safely eject the drive

This USB drive is then used to boot the server and begin the installation process.

## Installing Ubuntu Server

The server was restarted with the USB drive inserted into a USB port. 
During the boot up process, F12 was repeatedly pressed to bring up the boot order menu, making it possible to boot from the USB rather than the pre-installed Windows 11 Home installation.

Once booted into the Ubuntu installer, the OS installation process began.

![OS Install](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/3-OS-Install/1.jpeg)

## Storage Configuration

- 128GB M.2 SSD - Operating System
- 1TB HDD - User files and media storage

The storage layout was configured so that:
- The operating system is installed on the 128GB SSD
- The /home directory is mounted to the 1TB HDD

This allows the system files and user data to remain seperated, making storage easier to manage and expand in the future.

Both drives were formatted using the ext4 filesystem, which offers:
- Journaling for improved reliability
- Good performance
- Low maintenance requirements
- Automatic defragmentation behaviour

## System Naming

One of the final steps during installation is assigning a hostname to the system.

The server was named...

Benzaiten

This name comes from Benzaiten, the Japanese Lucky God of art, music, and things that flow. Given the server's purpose as a multimedia streaming platform, the name felt particularly fitting.

Following this naming convention, the NAS used for file storage (including the media files to be used with Plex later on) was renamed to Bishamonten, the Japanese Lucky God associated with protection, an appropriate role for a system responsible for safeguarding storage data.

## First Boot & System Updates

After completing the installation, the system was booted for the first time and logged into using the user account created during setup.

![OS First Boot](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/3-OS-Install/2.jpeg)

The first step was to update the operating system and installed packages.

To do this, the following commands were run:

- "sudo apt update"
- "sudo apt upgrade -y"

This ensures the server is running the most recent security patches and package versions.

![OS Update](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/3-OS-Install/3.jpeg)

## Installing and Enabling SSH

To allow remote management from the primary workstation, OpenSSH Server was installed.

- "sudo apt install openssh-server"

After installation, the SSH service was verified to ensure it was running correctly.

- "sudo systemctl status ssh"

This confirms that the system is ready to accept remote SSH connections.

## Testing Remote Access

With SSH enabled, the next step was to confirm that remote access was functioning correctly.

Before testing, the server was set a static IP address to allow for consistant SSH access as well as a static address for access to future services.

Using Command Prompt with elevated permissions on the main workstation, a connection was attempted using:

- "ssh matt@192.168.0.26"

After entering the account password, the login was successful, confirming that the server could now be managed remotely. 

![Remote Access](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/3-OS-Install/4.png)

From this point onward, the server can be administered entirely vis SSH without needing a dedicated monitor or keyboard connected.

## Summary

At this stage the system has been successfully prepared for service deployment

Completed tasks:

- Ubuntu Server installed
- Storage configured across SSD and HDD
- System fully updated
- SSH enabled for remote management

The server is now ready for the next phase of the project: installing and configuring the multimedia services.


