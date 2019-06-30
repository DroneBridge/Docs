---
description: >-
  The protocol & messages used by DroneBridge USBBridge module to communicate
  with an android device connected via USB to the ground station.
---

# USB message

## Introduction

The USBBridge module with the USB protocol allows for an alternative to WiFi based connections between the ground station and the DroneBridge for android app. Compared to USB tethering, the USB protocol allows for a more direct control of the data streams. It is fast & reliable. The user experience is flawless since the app automatically starts up when the phone is connected to the ground station. Most importantly the connection does not require the phone/tablet to have a SIM card. The SIM card is a requirement for android to support USB tethering. In past releases cable based connections were only possible with devices carrying a SIM card. The DroneBridge USB protocol helps to overcome that issue. With it all android based devices can communicate via a wired connection with the ground station.

The DroneBridge USB protocol is used to transfer data between an android device and the ground station. Both devices must be connected via USB. The android open accessory \(AOA\) protocol is used to establish a connection between the phone and the ground station \(USBBridge\). On top of the AOA protocol sits the DroneBridge USB protocol. It manages multiple IP streams over a single USB connection.

## Message format

| Identifier | Port | Payload length | Payload |
| :--- | :--- | :--- | :--- |
| Sequence of bytes to identify the start of a new message and its protocol version. | Virtual destination port of the data. Same values as for DroneBridge raw protocol | Length of the payload | Payload/User data |
| `0x44, 0x42, 0x01` \(DB1\) | `0x01, 0x02, 0x03, 0x04...` | unsigned 16bit  little endian | sequence of bytes |
| 3 bytes | 1 byte | 2 bytes | &lt;3072 bytes |

