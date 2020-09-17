---
description: Lists tests to be conducted before every new release
---

# System Integration Test Specification

## Prerequisites

* All Pis need to be operated using a power supply rated at least 2.1A 5V
* All connected adapters shall be connected via an active USB hub or have a separate USB power supply. Do not power via RPi USB ports!
* All tests are conducted in a noisy environment with a distance of 1-20 meters between GND & AIR

## Startup

* [ ] Sucessful startup of all modules with a **Pi3B** in **GND** configuration
  * [ ] Using one AR9271 driver based adapter 
  * [ ] Using two AR9271 driver based adapters 
  * [ ] Using one RTL8814AU driver based adapter
  * [ ] Using one RT2800 USB driver based adapter
* [ ] Sucessful startup of all modules with a **Pi3B** in **AIR** configuration
  * [ ] Using one AR9271 driver based adapter 
  * [ ] Using one RTL8814AU driver based adapter
  * [ ] Using one RT2800 USB driver based adapter
* [ ] Sucessful startup of all modules with a **Pi0W** in **AIR** configuration
  * [ ] Using one AR9271 driver based adapter 
  * [ ] Using one RTL8814AU driver based adapter
  * [ ] Using one RT2800 USB driver based adapter

### Video Transmission

* [ ] 15min stable video transmission using system default configuration between:

  * [ ] Pi3B \(AIR\) - Pi3B \(GND\) using a single AR9271 based adapter each
  * [ ] Pi0W \(AIR\) - Pi3B \(GND\) using a single AR9271 based adapter each

  Connection via HDMI out. Connection via WiFi hotspot and DroneBridge for Android.

* [ ] Data stream connection loss test: Auto reconnection after a lost signal event simulated by putting the AIR unit into a microwave oven using AR9271 cards.

### Telemetry Transmission

* [ ] 5min stable MAVLink telemetry transmission using system default configuration:
  * [ ] Pi3B \(AIR\) - Pi3B \(GND\) using a single AR9271 based adapter each

    Waypoint mission upload using QGroundControl

    Start/Stop mission using QGroundControl

### Communication Module

* [ ] Successful settings change on GND and AIR using DroneBridge for Android and a WiFi connection to the GND station
* [ ] Successful settings request from GND and AIR unit using DroneBridge for Android and a WiFi connection to the GND station

### RC Link

* [ ] 15min stable RC link using a Taranis based RC with OpenTX 2.2 firmware
  * [ ] Connection loss/recovery test by simulating a connection loss inside a microwave oven
  * [ ] Min of 30 packets/s at AIR unit at all times
  * [ ] Video and RC simultanious
  * [ ] RC USB connection loss test: Unplugging RC from GND station -&gt; failsafe event
  * [ ] RC USB auto reconnection on re-plugging RC to GND station -&gt; full recovery

### USBBridge Transmission

* [ ] Autostartup of DroneBridge for Android on connection
* [ ] 15min stable video & MAVLink telemetry transmission to app
* [ ] Communication module settings change on AIR and GND
* [ ] Auto-recovery of data streams on USB cable reconnection



