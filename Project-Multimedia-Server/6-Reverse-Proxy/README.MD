# Reverse Proxy & HTTPS with Caddy

## Objective

This stage focuses on configuring Caddy as a reverse proxy to expose the internal services securely over the inetner.

Instead of forwarding multiple service ports externally, Caddy listens on the standard web ports:

- 80 (HTTP)
- 443 (HTTPS)

Caddy then forwards incoming traffic to the correct inernal service ports on the server.

This setup also provides automatic SSL certificate management, ensuring all services are accessible securely over HTTPS.

By the end of this stage the system will:

- Use a public domain name to access services
- Automatically manage HTTPS certificates
- Forward requests to the correct internal services
- Replace temporary port forwarding with a cleaner access method.

## Installing Caddy

Caddy was installed using the official repository.

First, update the package lists and install the required dependencies.

- "sudo apt update"
- "sudo apt install -y curl grupg"

Add the official Caddy repository GPG key:

- "curl -fsSL https://dl.cloudsmith.io/public/caddy/stable/gpg.key | \"
- "sudo gpg --yes --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg"

Add the Caddy repository to the system sources:

- "echo "deb [signed-by=/usr/share/keyrings/caddy-stable-archive-keyring.gpg] \
https://dl.cloudsmith.io/public/caddy/stable/deb/debian any-version main" | \
sudo tee /etc/apt/sources.list.d/caddy-stable.list"

Update the package list again and install Caddy.

- "sudo apt update"
- "sudo apt install caddy"

![Caddy](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/6-Reverse-Proxy/04.png)

Once installed, the Caddy service is automatically enabled and running.

This can be tested using:

- "sudo systemctl status caddy"

![status](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/6-Reverse-Proxy/05.png)

## Preparing External Access

Before configuring the reverse proxy, a public domain name was required.

For this project, a free dynamic DNS service was used.

A hostname was created using DuckDNS, which point a domain name to the server's public IPv4 address.

The router was then configured to forward the following ports to the server:

- Port 80 > Server
- Port 443 > Server

During configuration, it was discovered that the CCTV DVR on the network had automatically claimed these ports using UPnP.

Since the DVR did not require these ports, and automatic port forwarding can present security risks, UPnP was disabled on the router and the ports were manually forwarded to the server instead.

![Ports](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/6-Reverse-Proxy/03.png)

## Configuring the Caddy Reverse Proxy

Caddy uses a configuration file called the Caddyfile.

This file defines which domain names should route to which internal services.

The configuration file was edited using:

- "sudo nano /etc/caddy/Caddyfile"

The file was then setup to forward the traffic to the correct ports from the public addresses.

![Nano](https://github.com/MatthewJonesIT/IT-Portfolio/blob/main/Project-Multimedia-Server/6-Reverse-Proxy/06.png)

In this configuration:

- Requests to matify.duckdns.org are forwarded to Navidrome
- Requests to plex.matify.duckdns.org are forwarded to Plex.

Caddy automatically handles:

- HTTPS certificate generation
- Certificate renewal 
- HTTP > HTTPS redirection

## Reloading the Configuration

After updating the configuration file, Caddy must be reloaded:

- "sudo systemctl reload caddy"

This applies the configuration without restarting the service.


## Result

The services can now be accesed remotely through the configured domain names.

Caddy automatically provisions and renews SSL certificates, ensuring all connections are encrypted.

## Summary

At this stage the server infrastructure has been fully deployed and securely exposed to the internet.

Completed tasks:

- Installed and configured Caddy reverse proxy
- Configured domain routing via DuckDNS
- Disabled insecure UPnP router configuration
- Forwarded ports 80 and 443 to the server
- Enabled automatic HTTPS with certificate renewal
- Routed Plex and Navidrome traffic through Caddy

The multimedia server is now accessible securely from anywhere using standard HTTPS connections.
