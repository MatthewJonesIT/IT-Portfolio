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

![SMB](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/1.png)

The Samba service was then restarted to implement the configuration change.

- "sudo systemctl restart smbd"

## Mounting the Share in Windows

On the Windows workstation, the Map Network Drive feature was used.

The share was mapped as the B: drive using:

- \\\192.168.0.26\Music

![Windows](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/2.png)

This allowed the music collection to be copied directly from the workstation to the server using the Windows file explorer. 

While this method is ideal for the initial bulk transfer, it requires manual updates whenever the library changes.

## Automating Music Synchronisation

To automate updates to the music library, an incremental backup system was implemented using rsync.

The design goals were:

- Only transfer new or modified files
- Do not delete files removed from the workstation
- Allow the server to pull files from the workstation rather than the workstation pushing them.

## Installing rsync

To install rsync on the server the following command was run.

- "sudo apt install rsync -y"

## Enabling SSH on the Windows Workstation

To allow the server to access the workstation, OpenSSH Server was installed on Windows using Optional Features.

![SSHServer](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/3.png)

Port 22 then needed to be opned in the Windows firewall.

The following command was ran in an elevated PowerShell window:

- "New-NetfirewallRule -Name sshd -DisplayName "OpenSSH Server(sshd)" -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22"

This was then checked in the Windows Defender Firewall with Advanced Settings applet in the Inbound Rules section to make sure it was implemented.

![Firewall](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/4.png)

The SSH service was then started in the PowerShell window and configured to launch automatically on startup with the following commands.

- "Start-Service sshd"
- "Set-Service -Name sshd - StartupType Automatic"

At this point, the Linux server could successfully SSH into the workstation.

![SSH](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/5.png)

## Mounting the Windows Music Share

A mount point was created on the server for the drive to be mounted to.

- "sudo mkdir -p /mnt/windows_music"

Because the workstation password contains special characters, much like the NAS, a credentials file was used to stop any parsing errors with inline credentials.

- "sudo nano /root/.smbcredentials2"

The credentials file was then secured

- "sudo chmod 600 /root/.smbcredentials2"

The music folder was then set to be shared on the Windows workstation. 

![Share](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/6.png)

This then allowed it to be mounted using CIFS. 

- "sudo mount -t cifs //192.168.0.100/Music /mnt/windows_music -o credentials=/root/.smbcredentials2,uid=100,gid=1000"

To verify the mount:

- "ls /mnt/windows_music"

The command displayed the full file structure of the workstation music folder, confirming the mount was successful.

![Files}(https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/7.png)

## Performing the Initial Sync

The first incremental backup was performed using:

- "rsync -av --update /mnt/windows_music /home/matt/Music"

This successfully transfered the music library to the server.

## Automating the Backup Process

To automate the process, a script was created to:

- Mount the Windows share
- Run the backup
- Unmount the share
- Log the results.

To begin the script the following command was used to create the file.

- "sudo nano /usr/local/bin/music_backup.sh"

The following was then added to the file.

![Script](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/8.png)

This script tells the server to attempt to mount the Windows share. If the mount succeeded, to then run the backup and then unmount the drive. Whilst doing this, it creates a log file which details if the backup was successful or not, and what files were transfered over.

Once the script was saved, it needed to be made executable with the following command:

- "sudo chmod +x /usr/local/bin/music_backup.sh"

## Testing the Script

The script was then ran, then the log viewed.

- "sudo /usr/local/bin/music_backup.sh"
- "cat /home/matt/music_backup.log"

![TestSync](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/9.png)

The log can also be viewed on the Windows workstation through the mounted drive with notepad.

![WindowsLog](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/10.png)

The script successfully transfers only new or updated files while preserving older files on the server.

## Scheduling the Backup with Cron

The script was automating using cron.

- "sudo crontab -e"

The follwoing entry was added.

![Cron](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/11.png)

This will run the script at 0 minutes and 20 hours every day, backing up the workstations music library at 8PM every day providing the workstation is on and connected to the network.

## Installing Navidrome

Navidrome was installed using the official release package.

- "wget https://github.com/navidrome/navidrome/releases/download/v0.60.3/navidrome_0.60.3_linux_amd64.deb"
- "sudo apt install ./navidrome_0.60.3_linux_amd64.deb"

The configuration file for Navidrome was then created and edited.

- "sudo nano /etc/navidrome/navidrome.toml"

![NAviConfig](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/5-Navidrome-Setup/12.png)

## Starting the Navidrome Service

The service was started using the following command.

- "sudo systemctl enable --now navidrome"

The status was then checked with:

- "sudo systemctl status navidrome"

Live logs can then be viewed using:

- "sudo journalctl -u navidrome -f"

At this point, Navidrome can now be accessed via 192.168.0.26:4533 to do the initial setup in the web interface.

## Fixing Library Permission Issues

Initially, Navidrome was unable to scan the music library due to insufficient permissions.

This was resolved by updating the directory permissions.

- "sudo chmod 755 /opt /opt/navidrome"

After applying the fix, Navidrome was able to successfully scan and index the music library.

## Temporary Remote Access

For testing purposes, port 4533 was forwarded on the router to the server.

This allowed remote access using mobile clients such as Symfonium.

This setup will later be replaced by a reverse proxy configuration using Caddy, which will provide:

- HTTPS encryption
- Cleaner domain access
- Improves security

## Summary

At this stage, the music streaming system has been successfully deployed.

Completed tasks:

- Music storage directory created
- Samba configured for initial file transfer and easy Windows access
- Automated incremental sync implemented with rsync
- Backup script and logging system created
- Daily scheduled backups configured via cron
- Navidrome installed and configured
- Music library successfully scanned and indexed
- Temporary remote access enabled

The server can now host and stream the full music collection while automatically synchronising updates from the workstation. 
