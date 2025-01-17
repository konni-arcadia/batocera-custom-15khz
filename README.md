# Batocera 15kHz Setup Guide

This guide explains how to configure a Batocera setup for a 15kHz CRT display using a reverse-prime configuration. This setup is tailored for an x86_64 system with the following hardware. Note that this guide is specific to my configuration and has not been tested on other setups. This configuration has been tested on Batocera v40.

## Hardware Configuration

- **Motherboard**: Gigabyte Z170X-Gaming 7
- **Graphics Cards**:
  - NVIDIA 2080 (primary, in the first PCIe slot, connected to an LCD display (only used when booting on my Windows setup))
  - ATI HD 4850 (secondary, in the second PCIe slot, connected to a JVC DT-V1700CG CRT via a DVI-VGA adapter and a VGA to BNC cable)

## Overview

The NVIDIA card handles the rendering and passes the framebuffer to the ATI card for display on the CRT (reverse-prime setup). However, I don't know how to verify that the NVIDIA is actively doing the rendering.

---

## Step-by-Step Guide

### 1. Create a Fresh Batocera USB Stick

### 2. Boot from the USB Stick

### 3. Create Configuration Files

You need to create two configuration files as follows:

#### `/etc/X11/xorg.conf.d/99-radeon.conf`:
```plaintext
Section "Device"
    Identifier "Radeon"
    Driver "radeon"
    BusID "PCI:2:0:0"  # Confirmed by `lspci` (see below for command)
    Option "PrimaryGPU" "true"
EndSection

Section "Monitor"
    Identifier "CRTMonitor"
    Option "PreferredMode" "640x480"
    HorizSync 31.5
    VertRefresh 60.0
EndSection

Section "Screen"
    Identifier "Screen0"
    Device "Radeon"
    Monitor "CRTMonitor"
    DefaultDepth 24
    SubSection "Display"
        Depth 24
        Modes "640x480"
    EndSubSection
EndSection

Section "ServerLayout"
    Identifier "Layout0"
    Screen 0 "Screen0" 0 0
EndSection
```

#### `/etc/X11/xorg.conf.d/98-prime.conf`:
```plaintext
Section "ServerLayout"
    Identifier "layout"
    Screen 0 "Radeon"
    Inactive "NVIDIA"
EndSection

Section "Device"
    Identifier "NVIDIA"
    Driver "nvidia"
    BusID "PCI:1:0:0"  # Confirmed by `lspci` (see below for command)
    Option "AllowEmptyInitialConfiguration"
EndSection

Section "Device"
    Identifier "Radeon"
    Driver "radeon"
    BusID "PCI:2:0:0"  # Confirmed by `lspci` (see below for command)
    Option "PrimaryGPU" "true"
EndSection

Section "Screen"
    Identifier "Radeon"
    Device "Radeon"
EndSection
```

### How to Get the BusID

To find the BusID for your GPUs:
1. Run the following command in a terminal:
   ```bash
   lspci | grep -i vga
   ```
2. Locate the output lines corresponding to your GPUs. For example:
   ```plaintext
   01:00.0 VGA compatible controller: NVIDIA Corporation ...
   02:00.0 VGA compatible controller: Advanced Micro Devices ...
   ```
3. Translate the BusID into the format required for the configuration files:
   - Replace the colon (`:`) after the first two digits with a period (`.`).
   - For example:
     - `01:00.0` becomes `PCI:1:0:0`
     - `02:00.0` becomes `PCI:2:0:0`

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
