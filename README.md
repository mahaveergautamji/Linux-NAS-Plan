# Linux-NAS-Plan

---

# Self-Hosted Cloud Infrastructure

Transform a fleet of Ubuntu desktops into a centralized private cloud with Remote Management & Monitoring (RMM), Network Attached Storage (NAS), and remote desktop access. This project provides a fully automated solution to create a secure, scalable, and cost-effective cloud using open-source tools.



## Overview

This project enables you to build a private cloud using one Ubuntu desktop as the **Controller Node** and up to 12 **Client Nodes**. The Controller orchestrates services, aggregates storage, and provides a single web interface, while Clients contribute storage and remote access capabilities.

- **Controller Node**: Hosts core services (RMM, NAS, remote access) and acts as the entry point.
- **Client Nodes**: Provide distributed storage via SSH and support remote desktop access.

## Core Service Stack

The solution leverages a robust stack of open-source tools, containerized or installed via Snap for stability:

- **MeshCentral**: Centralized RMM dashboard for managing client nodes (similar to AWS EC2 console).
- **Nextcloud**: Web-based NAS interface for accessing pooled storage (similar to Google Drive).
- **Apache Guacamole**: Browser-based remote desktop access to clients via SSH.
- **Nginx & Certbot**: Reverse proxy with Let's Encrypt SSL/TLS for secure HTTPS routing.
- **Cockpit (Optional)**: Web dashboard for monitoring the Controller's system resources.

## Data & Control Flow

The system pools storage from Client Nodes into a unified interface using:

1. **Client Machines**: Store data in `/home/ubuntu/data`.
2. **SSHFS Mounts**: Controller securely mounts client data to `/mnt/clients/` via SSH.
3. **MergerFS Pool**: Combines mounts into a single virtual directory (`/mnt/pooled_data`).
4. **Nextcloud Access**: Serves the pooled storage via a web interface at `cloud.yourdomain.org`.

## Security Features

- **UFW Firewall**: Restricts access to essential ports (SSH, HTTP, HTTPS).
- **Fail2Ban**: Blocks brute-force attacks by banning malicious IPs.
- **Let's Encrypt SSL**: Encrypts all web traffic with auto-renewing certificates.
- **SSH Key-Based Authentication**: Secures client storage mounts without passwords.

## Deployment

A single Bash script automates the entire setup process, including:

1. System preparation and security hardening.
2. Installation of Docker and service containers (MeshCentral, Apache Guacamole).
3. Nextcloud and Dynamic DNS (DDNS) configuration.
4. Nginx reverse proxy setup with SSL.
5. LAN discovery and SSH key distribution.
6. Storage pooling with SSHFS and MergerFS.

## Prerequisites

- **Controller Node**: Ubuntu desktop (20.04 LTS or later) with internet access.
- **Client Nodes**: 10-12 Ubuntu desktops with SSH enabled and storage in `/home/ubuntu/data`.
- **Domain Name**: For Nextcloud and service access (e.g., `cloud.yourdomain.org`).
- **Dependencies**: Bash, Docker, Snap, and an active internet connection.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/self-hosted-cloud.git
   cd self-hosted-cloud
   ```

2. Configure environment variables:
   - Edit `config.env` to specify your domain, SSH keys, and client IPs.

3. Run the deployment script:
   ```bash
   chmod +x deploy.sh
   ./deploy.sh
   ```

4. Access services:
   - **RMM**: `https://rmm.yourdomain.org` (MeshCentral)
   - **NAS**: `https://cloud.yourdomain.org` (Nextcloud)
   - **Remote Desktop**: `https://remote.yourdomain.org` (Apache Guacamole)

## Infographic

The included [infographic](index.html) provides a visual overview of the architecture, service stack, data flow, security, and deployment process. Open `index.html` in a browser to explore the interactive charts and diagrams.

## Contributing

Contributions are welcome! Please submit issues or pull requests for bug fixes, enhancements, or documentation improvements.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built with open-source tools: [MeshCentral](https://meshcentral.com), [Nextcloud](https://nextcloud.com), [Apache Guacamole](https://guacamole.apache.org), [Nginx](https://nginx.org), [Certbot](https://certbot.eff.org), and more.
- Inspired by the need for affordable, self-hosted cloud solutions.

---

### Notes for Customization
- Replace `yourusername/self-hosted-cloud` with your actual GitHub repository URL.
- Add a screenshot of the infographic (`infographic-screenshot.png`) to the repository for the README's visual.
- Ensure the `deploy.sh` script and `config.env` file exist in the repository, or update the instructions to match your setup.
- If specific versions or additional dependencies are required, list them in the Prerequisites section.

Let me know if you need help generating a screenshot, refining the README, or creating the deployment script!
