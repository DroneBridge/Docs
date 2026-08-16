---
description: Configuration Instructions for DroneBridge for ESP32 Long Range ESP-NOW Modes
---

# ESP-NOW Mode

### ESP-NOW LR Mode GND & ESP-NOW LR Mode AIR

![DroneBridge for ESP32 ESP-NOW Mode that can be used for drone swarms. Connectionless protocol with custom AES encryption. Requires ESP32s no UAV and ground.](https://raw.githubusercontent.com/DroneBridge/ESP32/master/wiki/modes/DB_ESP32_Mode_WiFi_ESPNOW.png)

DroneBridge for ESP32s\` custom ESP-NOW implementation using ESP-NOW broadcast packets with an AES256-GCM encrypted payload.\
Like all Long-Range (LR) modes, it requires you to have ESP32 devices as AIR- and GND-Unit and a Serial-to-USB adapter to connect a GCS.\
This is a more robust mode than the WiFi LR Mode because the ESP-NOW protocol is connectionless. The specified WiFi password is used for encryption.

{% hint style="info" %}
If you are planning a multi-drone deployment, also see [Drone Swarm Control](../drone-swarm-control.md) for a concise comparison between WiFi-based swarm setups and ESP-NOW.
{% endhint %}

You will not be able to change the config once ESP-NOW mode is enabled since the web interface will be unavailable! Short-press the boot button on the ESP32 to enable WiFi access point mode to be able to change settings.

{% hint style="info" %}
**Notes on security:**\
The AES-GCM encryption uses random IVs. If an attacker can listen to all of the traffic (encrypted using the same password), he has a 50% chance of decrypting/cracking your password after 2^48 packets.\
For you, this means you should change your password from time to time to be on the secure side. Generally, changing the password every 2^32 packets is advised to reduce the probability of a successful decryption attack to 1 in 4 billion.\
**Since telemetry is not generating a massive amount of packets/second you should be fine :)**
{% endhint %}

#### **Configuration**

Configure the ESP32 devices the following way depending on their role.\
**Recommendation: the web interface will not be available in ESP-NOW mode.** \
**It is recommended that the serial configuration be first tested using `WiFi Client Mode` or `WiFi Access Point Mode`.** \
**So first make sure you have a working setup when using a standalone ESP32 in Client or AP mode, then add the second ESP32 and configure both in ESP-NOW mode.**

#### ESP-NOW Auto-Binding

Starting with DroneBridge for ESP32 v2.3.x you can use the boot button on the ESP32 boards to init an auto-binding of two ESP32 devices running DroneBridge. No manual configuration necessary.

**User Steps**

1. Decide which device is the ground station and which device is the air unit.
2. Configure the preferred ESP-NOW channel on the device intended to become GND. The AIR unit will automatically receive this channel during binding.
3. **Double-click** the boot button on **GND**.
4. Within 90 seconds, **triple-click** the boot button on **AIR**.
5. Wait for both devices to restart. No password or channel needs to be entered on AIR.

**LED Feedback on Official ESP32-C6 Boards**

<table><thead><tr><th width="209">LED pattern</th><th>Meaning</th></tr></thead><tbody><tr><td>Slow pulse</td><td>Waiting for the other device or scanning channels</td></tr><tr><td>Fast pulse</td><td>Pairing and saving configuration</td></tr><tr><td>Solid briefly</td><td>Binding succeeded and the device will restart</td></tr><tr><td>Repeated flashes</td><td>Binding failed or timed out</td></tr></tbody></table>

**What binding configures**

Binding automatically creates and stores:

* A new random ESP-NOW link secret
* The selected GND channel on both devices
* ESP-NOW GND mode on the double-clicked device
* ESP-NOW AIR mode on the triple-clicked device

The normal Wi-Fi password is not changed. Existing manual ESP-NOW pairs continue using the old Wi-Fi password until they are successfully rebound.

**Important Note**

Perform binding in a trusted radio environment. The exchanged link secret is encrypted against passive listeners, but button-only binding does not protect against an active nearby attacker interfering during the 90-second setup window.

#### **Manual AIR Configuration**

In case you do not want to use auto-bind, you can manually configure the devices using the web interface.

* Mode: `ESP-NOW LR Mode AIR`
* Define a secure password - SSID is ignored (all ESP32s must share the same password). For releases v2.3.x or newer, you simply rotate the security key and copy the same key to all ESP32s you want to connect (AIR and GND).
* Set the channel to a number between 1-11 (all ESP32s must share the same channel)
* Set the TX & RX pins according to your configuration (official ESP32C3 board: TX=5, RX=4) and optionally the RTS & CTS pins if flow control shall be used
* Set the serial protocol according to your needs. (For MAVLink, the max. packet size shall be >=64 bytes)

Save the settings and trigger a reboot!

#### **Manual GND Configuration**

* Mode: `ESP-NOW LR Mode GND`
* Define a secure password - SSID is ignored (all ESP32s must share the same password). For releases v2.3.x or newer, you simply rotate the security key and copy the same key to all ESP32s you want to connect (AIR and GND).
* Set the channel to a number between 1-11 (all ESP32s must share the same channel)
* Set the TX & RX pins according to your configuration. With the default firmware, you need to connect a UART-to-USB adapter to the ESP32. Define the pins used for the connection here. \
  If you are running the `USBSerial` firmware, you will not be able to see these options since all serial data is sent via the USB port.
* Set the serial protocol according to your needs. (For MAVLink, the max. packet size shall be >=64 bytes)

Save the settings and trigger a reboot!
