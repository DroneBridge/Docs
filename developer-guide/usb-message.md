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

<table>
  <thead>
    <tr>
      <th style="text-align:left">Identifier</th>
      <th style="text-align:left">Port</th>
      <th style="text-align:left">Payload length</th>
      <th style="text-align:left">Payload</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Sequence of bytes to identify the start of a new message and its protocol
        version.</td>
      <td style="text-align:left">Virtual destination port of the data. Same values as for DroneBridge raw
        protocol</td>
      <td style="text-align:left">Length of the payload</td>
      <td style="text-align:left">Payload/User data</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>0x44, 0x42, 0x01</code> (DB1)</td>
      <td style="text-align:left"><code>0x01, 0x02, 0x03, 0x04...</code>
      </td>
      <td style="text-align:left">unsigned 16bit
        <br />little endian</td>
      <td style="text-align:left">sequence of bytes</td>
    </tr>
    <tr>
      <td style="text-align:left">3 bytes</td>
      <td style="text-align:left">1 byte</td>
      <td style="text-align:left">2 bytes</td>
      <td style="text-align:left">
        <p>&lt;=2048 bytes</p>
        <p>&lt;1500 recommended if packets go over raw link</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">bit 0-23</td>
      <td style="text-align:left">bit 24-31</td>
      <td style="text-align:left">bit 32-47</td>
      <td style="text-align:left">bit 48+</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
For payloads bigger than the max size allowed by the USB controller the payload can be split into multiple transmissions. In this case the header is only written once in the first transmission. The following transmissions for the payload must not have a DB-USB-MSG header.

Multiple payloads/headers per USB transmission are not allowed!
{% endhint %}

