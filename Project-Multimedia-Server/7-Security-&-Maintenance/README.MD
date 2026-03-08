# Security & Maintenance

## Objective

The final stage focuses on securing the server and enabling automatic maintenance.

Specifically, the goals are:

- Enable and configure the firewall to only allow necessary traffic
- Set up automatic system updates via unattended-upgrades
- Configure Plex Media Server to automatically update alongside system updates

By the end of this stage, the server will be secured, up to date, and able to maintain Plex automatically without manual intervention.

## Firewall Configuration

Ubuntu comes with the Uncomplicated Firewall (UFW) pre-installed. Its status can be checked with:

- "sudo ufw status"

Thiis initially returns inactive.

Before enabling the firewall, rules were configured to avoid losing SSH access to the server, thus locking the user out of the system remotely. 

Two blanket rules were added to close all incoming and open all outgoing ports.

- "sudo ufw default deny incoming"
- "sudo ufw default allow outgoing"

![Default](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/1.png)

Next, specific ports were allowed to enable server services.

- "sudo ufw allow 22/tcp"
- "sudo ufw allow 80/tcp"
- "sudo ufw allow 443/tcp"

This enables SSH access via TCP port 22, HTTP access via TCP port 80 and HTTPS access via TCP port 443.

Logging was then enabled.

- "sudo ufw logging on"

![Rules](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/2.png)

Finally, the firewall was activated

- "sudo ufw enable"

Because TCP port was explicitly allowed, the SSH connection remained stable and uninterrupted when the firewall was enabled. 

![Enable](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/3.png)

## Automatic System Updates

Ubuntu provides unattended-upgrades for automatic updates. This ensures the server stays secure without manual intervention.

The installation can be checked by running:

- "sudo apt install unattended-upgrades"

This shows the feature is already installed but not enabled.

To configure unattended upgrades to enable automatic updates, the following command is run.

- "sudo dpkg-reconfigure --priority=low unattended-upgrades"

![Up](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/4.png)

This prompts a configuration screen where automatic upgrades can be enabled. Once activated, Ubuntu security updates will install automatically.

![Access](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/5.png)

## Automatic Plex Updates

To keep Plex Media Server up to date automatically, the offical Plex repository was added.

- "curl -O https://repo.plex.tv/scripts/setupRepo.sh"

This downloads a script that automatically updates Plex to the latest official release.

The file downloaded needs to be made into an executable file, this can be done with the following command.

- "chmod +x setupRepo.sh"

Now it is executable, the script can be ran.

- "sudo ./setupRepo.sh"

This script replaces the previous .deb installation with a repository-based installation that can receive automatic updates.

![Plex](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/6.png)

## Configure Repository Preferences

To configure the preferences, a preferences file needs to be made, and made editable. 

- "sudo mkdir -p /etc/apt/preferences.d"
- "sudo chmod 755 /etc/apt/preferences.d"

This file can then be edited to update Plex.

- "sudo tee /etc/apt/preference.d/plex >> /dev/null <<EOF
Package: *
Pin: origin "repo.plex.tv"
Pin-Priority: 1001
EOF"

This can then be verified with:

- "cat /etc/apt/preferences.d/plex"

Now that this has been verified, the package list can be updated to see if Plex is included in with the updates.

- "sudo apt update"

Plex now appears in the update list, ensuring it will receive automatic updates alongside Ubuntu security patches. 

![Final](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/7-Security-%26-Maintenance/7.png)

## Summary

At this stage, the server is fully secured and maintained automatically.

- Firewall enabled with only necessary ports open
- Loggin enabled for firewall activity
- Unattended-upgrades configured for automatic system updates
- Plex repository added for automatic Plex updates.

The multimedia server is now fully operational, secure, and self-maintaining. 
