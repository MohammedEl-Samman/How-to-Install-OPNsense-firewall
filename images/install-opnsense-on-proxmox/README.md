# Proxmox VE — Brief and Installation Guide

## What is Proxmox VE?
Proxmox Virtual Environment (Proxmox VE) is an open‑source platform for server virtualization. It combines KVM for full virtual machines and LXC for lightweight containers, and includes a web UI, software‑defined storage (ZFS, LVM, Ceph), clustering, backup, and HA features. It's well suited for home labs, testing, and small datacenters.

## Typical minimum hardware
- 64‑bit CPU with virtualization (Intel VT‑x / AMD‑V)  
- 8 GB RAM minimum (16+ GB recommended for ZFS or multiple VMs)  
- 1+ NICs (1 for management, extra for VM networks)  
- SSD or HDD for storage (SSD recommended)

## Quick install overview

1. Download the ISO  
   - Get the latest Proxmox VE ISO from https://www.proxmox.com/en/downloads

2. Prepare bootable media  
   - Windows: use Rufus or balenaEtcher to write the ISO to a USB stick.  
   - Linux/macOS: use dd (replace /dev/sdX with your USB device):
     ```bash
     sudo dd if=proxmox-ve-*.iso of=/dev/sdX bs=4M status=progress && sync
     ```

3. Boot the target machine  
   - Insert USB, boot from it, and choose "Install Proxmox VE".

4. Walk through the interactive installer  
   - Accept the EULA → select target disk → set country/timezone → set root password & email → configure management network (IP, gateway, DNS) → confirm and install.  
   - The installer usually creates a Linux bridge (vmbr0) for VM networking.

5. Access the web UI  
   - After reboot, open: https://<proxmox-ip>:8006  
   - Login as root with the password set during installation.

## Post‑install essentials
- Update system:
  ```bash
  apt update && apt full-upgrade -y
  ```
- If you don't have a subscription, follow the official docs to enable the "no-subscription" repository for your Proxmox/Debian version (do not rely on random third‑party sources).
- Add storage, configure backups (Datacenter → Storage / Backup), and create VMs or containers via the web UI.
- For clusters: use a dedicated network for corosync and ensure time sync (chrony/ntp).

## Notes and best practices
- Use ZFS when you need snapshots and data integrity, and you have enough RAM.  
- Keep regular backups and test restores.  
- For VLANs, bridges, and firewall rules, configure networking in the Proxmox web UI before deploying critical VMs.  
- Read the official docs for version‑specific steps: https://pve.proxmox.com/wiki/Main_Page