# Cloud Computing Project 2 — 

**Intern:** Buhari Musa Liman
**Program:** DecodeLabs Cloud Computing Internship (AWS/Azure) — Batch 2026
**Platform:** AWS EC2 

> Provisioned, secured, and configured a live AWS EC2 Linux server from scratch — including SSH key-based access, a custom firewall policy, and a deployed Nginx web server — without relying on any managed hosting service.

## Overview

This project simulates provisioning a dedicated server environment for a startup that needs full control over its operating system to install custom software and security patches. The goal was to move beyond passive object storage (Project 1) and into active compute: launching, securing, and administering a real virtual server from the ground up.

## What I Built

I provisioned a Linux virtual machine on AWS EC2, connected to it securely over SSH, installed and configured a web server (Nginx), and deployed a custom webpage — all through the command line, with no managed services abstracting the work away.

## Steps Taken

### 1. IAM Permissions
Granted the `AmazonEC2FullAccess` policy to my existing least-privilege IAM user (`Buhari-Admin`), so EC2 management didn't require using the AWS root account.

### 2. Provisioning the Server
- Launched an EC2 instance (`Server-Commander-01`) running **Amazon Linux 2023**
- Instance type: `t3.micro` (Free Tier eligible)
- Created a new SSH key pair (`buhari-liman-project.pem`) for authentication — no password login is used

### 3. Securing the Perimeter
Configured a custom Security Group (`buhari-liman-sg`) following the principle of least privilege:

| Type  | Port | Source         | Purpose                        |
|-------|------|----------------|---------------------------------|
| SSH   | 22   | My IP only     | Restrict server admin access to me |
| HTTP  | 80   | Anywhere (0.0.0.0/0) | Allow public website traffic |
| HTTPS | 443  | Anywhere (0.0.0.0/0) | Reserved for future SSL setup |

### 4. Connecting Securely
Converted the `.pem` private key into `.ppk` format using **PuTTYgen**, then connected to the server from Windows using **PuTTY** over SSH, authenticating with the key pair instead of a password.

### 5. Deploying the Web Server
Ran the following commands directly on the server:

```bash
sudo dnf update -y
sudo dnf install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 6. Customizing the Deployment
Edited the default Nginx webpage directly on the server:

```bash
sudo nano /usr/share/nginx/html/index.html
```

Replaced the default content with a custom welcome page confirming successful deployment.

### 7. Verification
Confirmed the server was live and publicly accessible by visiting its public IP address in a browser, and verified Nginx's running status via `systemctl status nginx`.

## Proof of Completion

*(Screenshots below)*

1. Nginx running status
2. EC2 instance summary (running state, public IP)
3. Security Group inbound rules
4. SSH connection succeeding via PuTTY
5. Final live webpage

## Key Concepts Demonstrated

- **Infrastructure as a Service (IaaS):** Full responsibility for OS, patching, and security configuration, unlike Project 1's fully managed object storage
- **Shared Responsibility Model:** AWS secures the physical infrastructure; I secured the operating system, firewall rules, and access management
- **Key-based authentication:** No passwords — access is controlled entirely through a private SSH key
- **Least privilege firewall rules:** SSH restricted to a single IP; only web traffic (80/443) exposed publicly
- **Command-line system administration:** Installed, configured, and managed a production web server entirely via terminal

## Reflections / What I Learned

This project shifted my perspective from "renting storage" to "owning a machine." Unlike Project 1, where AWS handled everything below the file level, here I was responsible for the entire software stack above the hypervisor — patching the OS, configuring the firewall, and managing a running service myself.

As someone working in cybersecurity, the Security Group configuration stood out most. Restricting SSH to a single IP address, while leaving web ports open only where necessary, reinforced a principle I apply professionally: expose the minimum necessary surface, and authenticate with keys rather than passwords wherever possible. I also noted that EC2, unlike S3, is billed by the hour regardless of usage — a reminder that compute resources carry an ongoing cost and operational responsibility that storage does not.

This project gave me hands-on experience with the shared responsibility model in practice, not just in theory: AWS secured the physical infrastructure, and everything from that point upward was mine to configure correctly.

## Tools Used

AWS EC2, Amazon Linux 2023, Nginx, PuTTY, PuTTYgen, SSH

Part of the DecodeLabs Cloud Computing Internship (AWS/Azure), 2026 Batch.