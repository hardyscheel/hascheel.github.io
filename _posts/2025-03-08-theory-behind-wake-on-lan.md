---
layout: post
title: Theory Behind Wake-on-LAN
description: Wake on lan briefly explained.
date: 2025-03-08
author: Hardy Scheel
tags: [WoL, Wake-on-LAN, wakeonlan, Magic-Packet, Python]
published: true
---

### Table of content
* [What is a Wake-on-LAN magic packet](#What-is-a-Wake-on-LAN-magic-packet)
    * [Theory Behind Wake-on-LAN](#Theory-Behind-Wake-on-LAN)
    * [Structure of the Magic Packet](#Structure-of-the-Magic-Packet)
    * [Transmission of the Magic Packet](#Transmission-of-the-Magic-Packet)
    * [Why 16 Repetitions of the MAC Address?](#Why-16-Repetitions-of-the-MAC-Address)

---

## What is a Wake-on-LAN magic packet

A Wake-on-LAN (WOL) Magic Packet is a special network packet that is used to activate computers or other network devices from a power-saving state (e.g. sleep or standby mode). The concept is based on the ability of the network card to recognize and react to certain packets even when it is switched off.

- The Magic Packet is a simple yet effective protocol to wake up devices over the network.
- It consists of a 6-byte header (`0xFF`) and 16 repetitions of the target device's MAC address.
- The packet is sent via UDP to the broadcast address and is detected by the target device's network card.

### **Theory Behind Wake-on-LAN**
1. **Requirements**:
   - The target device's network card must support Wake-on-LAN.
   - Wake-on-LAN must be enabled in the BIOS/UEFI and network card settings.
   - The device must be connected to a network (either via cable or Wi-Fi, though WOL over Wi-Fi is not always supported).

2. **How It Works**:
   - The target device remains in a standby mode where the network card stays active and listens for specific packets.
   - A special packet (the "Magic Packet") is sent to the target device to wake it up.

---

### **Structure of the Magic Packet**
The Magic Packet is a UDP packet with a specific structure. It consists of two main parts:

1. **Header**:
   - 6 bytes with the value `0xFF` (decimal 255).
   - This header serves as a marker to identify the Magic Packet.

2. **MAC Address**:
   - The MAC address of the target device is repeated 16 times.
   - The MAC address is 6 bytes long, so this part of the packet comprises 96 bytes (16 repetitions × 6 bytes).

---

#### ***Magic Packet Structure***
| Part             | Size (Bytes) | Content                                   |
|------------------|--------------|------------------------------------------|
| Header           | 6            | `0xFF 0xFF 0xFF 0xFF 0xFF 0xFF`         |
| MAC Address (x16)| 96           | 16 repetitions of the MAC address        |
| **Total**        | **102**      |                                          |

---

### **Example**
Assume the MAC address of the target device is `00:11:22:33:44:55`. The Magic Packet would look like this:

1. **Header**:  
   `FF FF FF FF FF FF`

2. **MAC Address (16 repetitions)**:  
   `00 11 22 33 44 55 00 11 22 33 44 55 00 11 22 33 44 55 ...` (16 times in total).

The complete Magic Packet in hexadecimal representation:  
`FF FF FF FF FF FF 00 11 22 33 44 55 00 11 22 33 44 55 ...` (102 bytes).

---

### **Transmission of the Magic Packet**
- **Protocol**: UDP (User Datagram Protocol).
- **Destination Address**: The broadcast address of the network (e.g., `255.255.255.255`).
- **Port**: By default, port 7 (Echo) or 9 (Discard), but any other port can be used.

---

### **Why 16 Repetitions of the MAC Address?**
The 16 repetitions of the MAC address ensure that the packet is recognized even if there are packet losses or errors in the network. This increases the reliability of the Wake-on-LAN mechanism.

---
