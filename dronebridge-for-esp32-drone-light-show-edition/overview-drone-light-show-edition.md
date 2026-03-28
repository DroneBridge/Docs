---
description: Drone Light Show Edition (DLSE) of DroneBridge for ESP32
---

# Overview Drone Light Show Edition

{% hint style="info" %}
**This section of the documentation is for the Drone LightShow Edition (DLSE) of DroneBridge for ESP32 only!**&#x20;
{% endhint %}

See the section above for documentation regarding the public general-purpose release of DroneBridge for ESP32.

The Drone Light Show Edition (DLSE) has the following improvements over the general-purpose release of DroneBridge for ESP32:

* Support for Lightbrush.io and its flight controller firmware
  * Auto-detection of the UAV by Skybrush Live
  * Support for remote sleep & wakeup modes
  * Battery Monitoring when in sleep mode
  * Various MAVLink- and Drone Show-related configuration parameters in the firmware
  * Open-Source Commercial Support Suite to help with flashing, the setup and configuration of a drone fleet
  * Documentation specifically targeted for skybrush & drone show setup
* Over-The-Air (OTA) Updates of the ESP32 firmware
* Improved safety: In code safety checks with drone shows in mind & reduced code size/complexity
* Reduced the load on the network by each ESP32 by disabling unnecessary services
* First to support the ESP32-C5 with dual-band Wi-Fi capability
