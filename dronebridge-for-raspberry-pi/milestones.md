---
description: Milestones for past and upcoming releases
---

# Milestones

## Beta v0.7 and later...

* Rewrite the WBC video module using faster FEC libs
* Upload and verify missions using the android app
* Allow the creation of missions for MAVLink in the Android app
* Allow DroneBridge to start in "Desktop-Mode" on ground pi to use with mwptools \(iNAV\) or QGroundControl, AMP Planner 2 UgCS \(MAVLInk\)

## Beta v0.6 \(WIP\)

* **Added USBBridge: Send data directly via USB using Android Open Accessory \(AOA\) protocol**
* **Move to TCP connection on GND to GCS except for video \(removed IP checker\)**
* **Professional Log & Trace support of all modules using syslog and TCP log msg. forwarder**
* **Plugin system so users can easily add their own scripts and modules to DroneBridge. Plugins can be easily installed on every new release so users do not need to modify the newly released image to get their personal code running again.**
* **Compete rewrite of all startup code. New startup system based on service**
* **All modules including video use DroneBridge libraries and support diversity**
* **Support for AES encryption with the Python library**
* **Refractoring of Common libraries \(Python & C\)**
* **Support for 12 channel MAVLink output via DroneBridge RC.** Or 14ch MAVLink when not using DroneBridge RC \(not recommended\)
* **Control module on UAV side write received RC channel values to shared memory so that plugins can access the channel values**
* **Control module reports CPU usage and temp of AirPi to ground station** in the same message as the RC RSSI.
* Log telemetry to file on GND station
* New folder structure
* Support for new DroneBridge for Android release
* Upgrade to latest Kernel and publish source on [RPiKernel repository](https://github.com/DroneBridge/RPiKernel)
* Allow for RC overrides on DroneBridge control module. Override messages are received by status module and written to corresponding shared memory. Can be used for head tracking etc.
* \(**WIP**\) Add support for RTL8812au cards
* Support for Raspberry Pi 3B+
* Support for Raspberry Pi 4 \(untested\)
* Remove almost all WBC legacy code

## Beta v0.5 \(released\)

* ✔ Easier/more convenient way to calibrate the RC. Use of jscal-store & jscal-restore. Init calibration from DroneBridge app possible.
* ✔ Support for OpenTX based radios \(eg. all Taranis models, tested with FrSky X-Lite and OpenTX 2.2\)
* ✔ Support for multiple cameras using a [multi camera adapter](http://www.arducam.com/multi-camera-adapter-module-raspberry-pi/) - switch can be issued via app
* ✔ Fixed telemetry stream when connected via USB tethering
* ✔ Added new communication messages: ping, ask, camswitch, rcadjust, firmware info
* ✔ Added version file
* ✔ Massive stability & usability improvements in combination with DroneBridge app \(70% less cpu usage, 4ms less latency, new user dialogs to see whats going on, bugfixes etc.\)

## Beta v0.4 \(released\)

* ✔ Verified SUMD RC output in UAV
* ✔ App sends user data for debugging purposes \(only on crash\)
* ✔ Implement telemetry module \(air and ground\) in C for less CPU usage
* ✔ Merge telemetry module code & proxy module code into one executable
* ✔ Release DroneBridge for ESP32 \(ultra cheap medium-range UAV link\)
* ✔ Added working example on how to use DroneBridge python lib
* ✔ New and more robust hotspot checker \(move away form EZ-Wifibroadcasts way of doing things\)
* ✔ Fixed OSD via proxy telemetry

## Beta v0.3.1 \(released\)

* ✔ Provide a new MAVLink config option that is a simple pass through

## Beta v0.3 \(released\)

* ✔ Based on Wifibroadcast 1.6RC6
* ✔ support LTM and MAVLink telemetry over telemetry module
* ✔ support LTM and MAVLink telemetry in DroneBridge Android app over same port
* ✔ Auto detection on telemetry type in telemetry module \(MAVLink stream and LTM packet wise\)
* ✔ Implementation of DroneBridge RC protocol
* ✔ Status module sends status-frames and rc-channel-frames to Android app
* ✔ Android app can display RC values from MAVLink telemetry and rc-channel-frames from status module
* ✔ Full move to DroneBridge raw protocol v2
* ✔ Control module on drone sends received packet count \(RC related\) to status module
* ✔ Control module has two open raw ports: One for MAVLink/MSP up/downlink and for DroneBridge RC messages
* ✔ Creating libdb\_common: A library doing all the raw protocol related stuff for faster development
* ✔ Full move to libdb\_common with all modules implemented in C
* ✔ Allow to request individual settings \(DB, wbc\) using the DroneBridge communication protocol --&gt; smaller packets, less traffic and better control
* ✔ Implement DroneBridge proxy module to manage MSP/MAVLink messages between raw and UDP interface
* ✔ MSP/MAVLink transparent up and downlink using control module and proxy module
* ✔ Support MAVLink over only one serial port \(telemetry, RC, commands over one module and serial port\) using control module
* ✔ Make all UDP ports configurable
* ✔ Add RC via SUMD option to control module for use with MAVLink FCs: --&gt; \[DB\_RC\] --&gt; control module --&gt; \[SUMD\] --&gt; FC
* ✔ Allow to easily configure serial port setup and module setup via the Android app and the communication module

