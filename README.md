# Batocera 15kHz Setup Guide

This guide explains how to configure a Batocera setup for a 15kHz CRT display using a reverse-prime configuration. This setup is tailored for an x86_64 system with the following hardware:

## Hardware Configuration

- **Motherboard**: Gigabyte Z170X-Gaming 7
- **Graphics Cards**:
  - NVIDIA 2080 (primary, in the first PCIe slot, connected to an LCD display (only used when booting on my windows setup))
  - ATI HD 4850 (secondary, in the second PCIe slot, connected to a JVC DT-V1700CG CRT via a DVI-VGA adapter and a VGA to BNC cable)

## Overview

The NVIDIA card handles the rendering and passes the framebuffer to the ATI card for display on the CRT (reverse-prime setup). However, I don't know how to verify that the NVIDIA is actively doing the rendering.

---

## Step-by-Step Guide

### 1. Create a Fresh Batocera USB Stick

### 2. Boot from the USB Stick

### 3. Create Configuration Files

You need to create two configuration files. (Details on the file names and contents will be provided later.)

### 4. Save the Overlay

After creating the configuration files:

1. Run the command to save the overlay:
   ```
   batocera-save-overlay
   ```

2. Reboot the system.

### 5. Use the CRT Script

Run the CRT configuration script from https://github.com/ZFEbHVUE/Batocera-CRT-Script:

### 6. Reboot the System

---
