# Arch Linux: Post-Install Setup Guide

This repository documents the steps to configure a fresh Arch Linux installation — from system updates and mirrors to AUR helpers, firewall, and a curated set of applications.

---

## Prerequisites

- A freshly installed Arch Linux system
- Internet access
- A non-root user with `sudo` privileges

---

## Step 1 — Update the System

Run a full system upgrade and install core tools:

```bash
sudo pacman -Syu
```

```bash
sudo pacman -S nano git vim fastfetch
```

---

## Step 2 — Configure Pacman

Open the Pacman configuration file:

```bash
sudo nano /etc/pacman.conf
```

Make the following changes:
- Uncomment `Color`
- Uncomment `VerbosePkgLists`
- Uncomment the `[multilib]` section (required for Steam)
- Add `ILoveCandy` under the `[options]` block to enable the Pac-Man progress bar

Then sync the package database:

```bash
sudo pacman -Sy
```

---

## Step 3 — Optimize Mirrors with Reflector

Install Reflector:

```bash
sudo pacman -S reflector
```

Back up the existing mirror list:

```bash
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
```

Fetch and apply the 10 fastest HTTPS mirrors:

```bash
sudo reflector --verbose --latest 10 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Sync again:

```bash
sudo pacman -Sy
```

---

## Step 4 — Install Useful Packages

Install a collection of common utilities:

```bash
sudo pacman -S p7zip unrar tar rsync git ark htop exfat-utils fuse-exfat ntfs-3g flac jasper aria2
```

Install a Java environment:

```bash
sudo pacman -S jdk-openjdk
```

---

## Step 5 — Install Microcode Updates

### AMD CPU

```bash
sudo pacman -S amd-ucode
```

Then regenerate the GRUB configuration to apply the microcode:

```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

---

## Step 6 — Install Yay (AUR Helper)

Install build dependencies:

```bash
sudo pacman -S --needed base-devel git
```

Clone the Yay repository:

```bash
git clone https://aur.archlinux.org/yay.git
```

Navigate into the directory and build:

```bash
cd yay
makepkg -si
```

---

## Step 7 — Set Up a Firewall

Install and enable UFW (Uncomplicated Firewall):

```bash
sudo pacman -S ufw
sudo systemctl enable ufw
sudo systemctl start ufw
```

---

## Step 8 — Install Common Applications

Install packages from the official repositories:

```bash
sudo pacman -Syu --needed steam libreoffice-fresh vlc nextcloud-client tailscale ckb-next
```

Install AUR packages via Yay:

```bash
yay -S --needed discord betterbird-bin minecraft-launcher zoom
```

---

## Step 9 — Enable and Start Services

Enable and start the Tailscale daemon:

```bash
sudo systemctl enable --now tailscaled
```

Enable and start the CKB-Next daemon (Corsair peripheral support):

```bash
sudo systemctl enable --now ckb-next-daemon
```

Connect to your Tailnet:

```bash
sudo tailscale up
```

---
