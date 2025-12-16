# Secure SSH Access to Raspberry Pi via Cloudflare Tunnel

This guide documents the process of setting up secure remote SSH access to a Raspberry Pi from anywhere in the world using Cloudflare Tunnel and Cloudflare Access.

## Overview

This setup allows you to:
- SSH into your Raspberry Pi from any location
- Avoid exposing your home IP address or opening router ports
- Secure access with email-based OTP verification
- Access via both browser and terminal

## Prerequisites

- Raspberry Pi with SSH enabled
- Domain name registered through Cloudflare
- Docker installed on Raspberry Pi
- Cloudflare account

## Setup Process

### 1. Configure Static IP Address

Before setting up the tunnel, I assigned a static IP address to my Raspberry Pi. This ensures that the device maintains the same local IP address even after power failures or network disruptions, making the tunnel configuration permanent and reliable.

### 2. Install and Configure Cloudflare Tunnel

I set up a Cloudflare Tunnel to create a secure connection between my Raspberry Pi and the outside world through my Cloudflare-registered domain.

**Docker Configuration:**
The tunnel was configured to run as a Docker container with the `restart: always` policy. This ensures the tunnel automatically reconnects after system restarts without manual intervention.

**Tunnel Setup:**
- Created a tunnel in the Cloudflare dashboard
- Configured the tunnel to accept SSH connections
- Linked the tunnel to my domain
- Set up routing to forward SSH traffic to the Raspberry Pi's static IP

### 3. Configure Cloudflare Access Policy

To add an authentication layer, I created a Cloudflare Access application:

1. **Access Policy:** Configured to allow SSH connections only from my registered email address
2. **Authentication:** Email-based OTP verification ensures that only authorized users can access the SSH connection
3. **Browser Rendering:** Set to SSH protocol to enable proper handling of SSH connections

### 4. Browser-Based SSH Access

After completing the configuration, I can access my Raspberry Pi through a web browser:

1. Navigate to the configured domain (e.g., `https://ssh.yourdomain.com`)
2. Enter the registered email address
3. Verify identity using the OTP code sent to email
4. Access the Pi through the browser-based SSH interface

### 5. Terminal-Based SSH Access

For a more traditional SSH experience, I set up terminal access on my client machine:

**Installation:**
```bash
# Install cloudflared on your client machine
# (Installation method varies by OS)
```

**Authentication:**
```bash
cloudflared access ssh-gen --hostname ssh.yourdomain.com
```

This command initiates the authentication flow:
1. Opens a browser window for verification
2. Prompts for email verification via OTP
3. Generates SSH configuration upon successful authentication

**Connecting via Terminal:**
```bash
ssh username@yourdomain.com
```

The authentication process triggers automatically, and after email verification, terminal access to the Raspberry Pi is established.

## Benefits

- **No Port Forwarding:** Eliminates the need to open ports on your home router
- **Enhanced Security:** Email-based OTP adds a strong authentication layer
- **Persistent Connection:** Docker restart policy ensures tunnel stays active
- **Flexible Access:** Choose between browser or terminal-based SSH
- **Static Configuration:** Fixed IP prevents connection issues after restarts

## Notes

- Keep your Cloudflare credentials secure
- The tunnel runs continuously in the background via Docker
- OTP verification is required for each new session
- The static IP configuration should be backed up for disaster recovery

## Troubleshooting

- If the connection fails after a restart, verify the Docker container is running
- Ensure the Raspberry Pi's static IP hasn't changed
- Check Cloudflare Access logs for authentication issues
- Verify the tunnel status in the Cloudflare dashboard

---

**Security Reminder:** Never share your OTP codes or Cloudflare credentials. This setup is designed for personal use and should be configured according to your security requirements.
