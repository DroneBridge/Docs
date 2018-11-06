---
description: >-
  Current releases can be found at:
  https://github.com/seeul8er/DroneBridge/releases
---

# Release Notes

## v0.5 Beta

**Warning: This is a beta release! Despite extensive tests use with caution!**  
**DroneBridge for Android v1.2.2+ required!**

### Added/Changed

* Added OpenTX support \(e.g all FrSky radios - tested with FrSky X-Lite\) - just connect via USB and use DroneBridge for transmission of RC signals
* Auto calibration for OpenTX radios on Linux side via app - Calibrate your RC first! Only use if values still not correct
* Added communication messages: error, ping, ACK, camselect, adjustrc
* [Support for multi-camera modules.](http://www.arducam.com/multi-camera-adapter-module-raspberry-pi/) Change camera via JSON or using the app \(Not tested due to no available hardware\)
* Support for new app features: ping, cam select & adjustrc
* Faster response to connection errors due to reduced timeouts
* Better error messages forwarded to app
* Removed calls for WBC MSP bidirectional link - Use DroneBridge modules
* More robust Android app - bugfixes and usability improvements

### Fixes

* No data except video when connected via USB tethering
* DroneBridge modules would start too early when USB Stick is connected on startup

## v0.4 Beta

**Warning: This is a beta release! Despite extensive tests use with caution!**

### Added/Changed

* Verified SUMD RC output in UAV
* Fixed mwptools support
* Implement telemetry module \(air and ground\) in C for less CPU usage
* Merge telemetry module code & proxy module code into one executable
* Release DroneBridge for ESP32 \(ultra cheap medium-range UAV link\)
* Added working example on how to use DroneBridge python lib
* New and more robust hotspot checker \(move away from EZ-WifiBroadcasts way of doing things\)
* Fixed OSD via proxy telemetry

## v0.3.1 Beta

**Warning: This is a beta release! Despite extensive tests use with caution!**

### Added/Changed

* added transparent pass through option for a telemetry stream \(recommended for MAVLink telemetry\)
* fixed 5% CPU usage by control module on groundstation. Now it is &lt; 0.1%

## v0.3 Beta

 **Warning: This is a beta release! Despite extensive tests use with caution!**

### **Added/Changed**

Much of the code and protocols has been rewritten. DroneBridge now features a custom C-lib that handles all the protocol and transmission stuff. That way all modules are based on the same source, except python implementations. All fancy new features of WifiBroadcast 1.6RC6 like OSD and kernel modules are available. DroneBridge comes with updated packages.

* **Full support for MAVLink telemetry \(DroneBridge on Raspberry Pi & app\)**
* MAVLink upload \(bidirectional link\). Untested as I do not have a MAVLink FC :\(
* **MSP upload/download fully supported and tested \(might be slow due to packetloss or some bug\)**
* MWP Tools sucessfuly tested with DroneBridge. Upload iNAV missions!
* **All new DroneBridge RC protocol \(12 channels & 50% smaller packets, more secure\)**
* **All new DroneBridge raw protocol v2: way smaller packets while keeping all the functionality**
* Android app can display RC channels \(via MAVLink telemetry or DroneBridge RC\)
* Based on WBC 1.6 RC6
* Ability to set/request individual settings - smaller packets, future proof
* Telemetry module baud rate can be changed
* Display WBC video stats in app
* **SUMD RC output** \(Untested\)
* Lots of code cleanup and new modules \(C only\)
* Custom splash screen at boot

## **v0.2 Beta**

### **Fixes**

* ip checker module caused telemetry lag

### **Known Issues**

* WifiBroadcast needs to be running before modules do start
* MSP upload is not verified by control module. It is not recommended to use control module to upload missions yet
* No wifi mode support
* No usb accessory mode \(use usb-tethering or wifi\)
* RC signal quality not forwarded to WBC OSD
* Reception on drone side measured by telemetry module and not by control module
* currently two versions of the DroneBridge LTM frame get transmitted \(Y \(telemetry module\) & Z \(status module\)\)

## **v0.1 Beta**

**Warning: This is a beta release! Despite extensive tests use with caution!**  
**This image is based on the WifiBroadcast 1.6 RC2 release**

### **Added**

* status module
* control module \(RC i6S\)
* telemetry module
* communication module
* support for Atheros and Ralink chips

### **Known Issues**

* WifiBroadcast needs to be running before modules do start
* MSP upload is not verified by control module. It is not recommended to use control module to upload missions yet
* No wifi mode support
* No usb accessory mode \(use usb-tethering or wifi\)
* RC signal quality not forwarded to WBC OSD
* Reception on drone side measured by telemetry module and not by control module
* currently two versions of the DroneBridge LTM frame get transmitted \(Y \(telemetry module\) & Z \(status module\)\)



