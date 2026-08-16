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
**Recommendation: the web interface will not be available in ESP-NOW mode.**\
**It is recommended that the serial configuration be first tested using `WiFi Client Mode` or `WiFi Access Point Mode`.**\
**So first make sure you have a working setup when using a standalone ESP32 in Client or AP mode, then add the second ESP32 and configure both in ESP-NOW mode.**

#### ESP-NOW Auto-Binding

Auto-binding configures the channel, device roles, and shared ESP-NOW secret. One GND can bind multiple AIR units.

{% hint style="info" %}
After every reboot, wait at least **2 seconds** before pressing the **BOOT/Fn button** again. On an official ESP32-C6 board, you can continue when the LED starts pulsing.
{% endhint %}

**Bind AIR units**

1. Set the desired ESP-NOW channel on **GND**.
2. **Double-click the BOOT/Fn button on GND.** It reboots and waits for AIR units.
3. For each AIR unit:
   1. Wait at least 2 seconds after boot.
   2. **Triple-click the BOOT/Fn button on AIR.**
   3. Wait for the success indication and automatic reboot.
4. After adding the final AIR, **single-click the BOOT/Fn button on GND**. GND reboots into normal ESP-NOW operation.

GND reuses its existing group secret. If no secret exists, it creates one automatically. AIR scans channels 1–13 for up to 90 seconds and receives the GND channel and secret automatically.

**Start a new group**

**Four-click the BOOT/Fn button on GND** instead of double-clicking it. This creates a new secret and disconnects all previously bound AIR units. Bind every required AIR again, then single-click the BOOT/Fn button on GND to finish.

The **Rotate secret** button in the web interface also creates a new secret, but does not start binding automatically.

**LED feedback on official ESP32-C6 boards**

| LED           | Meaning                     |
| ------------- | --------------------------- |
| Slow pulse    | Waiting or scanning         |
| Fast pulse    | Binding in progress         |
| Solid briefly | AIR bound successfully      |
| Three flashes | Binding failed or timed out |

Other supported boards use the same BOOT/Fn button sequence but may not provide LED feedback.

{% hint style="warning" %}
Bind only in a trusted radio environment. The secret is protected from passive listeners, but button-only binding does not protect against an active nearby attacker.
{% endhint %}

#### Manual GND Configuration

Use the web interface if you prefer to configure the ESP-NOW link manually.

1. Select `ESP-NOW LR Mode GND`.
2. Select the ESP-NOW channel.
3. Enter a 43-character ESP-NOW link secret, or click **Rotate secret** to generate one.
4. Copy the complete secret. Every AIR unit must use exactly the same value.
5. Configure the serial protocol, baud rate, and required pins.
6. Click **Save Settings & Reboot**.

With the standard firmware, connect the GND through a UART-to-USB adapter using the configured TX and RX pins. The `USBSerial` firmware uses the board’s USB connection instead.

#### Manual AIR Configuration

Configure every AIR unit with the same channel and secret as GND.

1. Select `ESP-NOW LR Mode AIR`.
2. Select the same ESP-NOW channel as GND.
3. Paste the exact ESP-NOW link secret copied from GND.
4. Configure the serial protocol, baud rate, and required pins.
5. Click **Save Settings & Reboot**.

The secret is saved to NVM and applied after reboot. Repeat these steps for every additional AIR unit.

{% hint style="info" %}
If the ESP-NOW link secret is empty, the firmware uses the normal Wi-Fi password as a legacy fallback. All manually configured devices must still use the same channel and encryption value.
{% endhint %}

After every reboot, wait at least 2 seconds—or wait for the LED to indicate the next state—before using the BOOT/Fn button.
