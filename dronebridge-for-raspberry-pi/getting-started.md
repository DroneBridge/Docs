---
description: Guide on setting up the DroneBridge system
---

# Getting started

## Content

* [Basic hardware & setup](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#basic-hardware--setup)
* [MultiWii based flight controller \(iNAV\)](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#setup-multiwii-based-flight-controller)
* [MAVLink based flight controller](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#setup-mavlink-based-flight-controller)
* [Connecting an RC transmitter](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#connecting-a-rc-transmitter)

## Basic hardware & Installation

Hardware to GND & AIR side:

* **Two Raspberry Pis \(Pi Zero W should work but Pi 2 or 3 are recommended\)**
* **Two SD cards with min. 4GB storage**
* **A Pi camera module \(v1 or v2\)**
* **Two WiFi sticks \(**[**Supported WiFi adapters**](https://github.com/seeul8er/DroneBridge/wiki/Supported-Hardware#wifi-adapters)**\)**
* &gt;2A 5V power supply/BEC

Write the image onto the SD cards using e.g. [Win32 Disk Imager \(Windows\)](https://sourceforge.net/projects/win32diskimager/) or your favourite Linux tool \(e.g. dd or [some GUI tools](https://www.fossmint.com/3-best-gui-enabled-usb-image-writer-tools-on-linux/)\). Connect the Pi camera module to the raspberry pi that will be on the UAV. Make sure your power supply for both Pis provides min. 2A current and stable 5V!

## Wiring 

First the FCs UART must be wired to the AirPis UART. **The UART of the Raspberry Pi is 3.3V only!** In case your FC uses 5V on the UART you will need a logic level converter.

The Raspberry Pis UART is on GPIO 14 \(TX\) & 15 \(RX\). Connect the TX to RX and vise versa. There are many tutorials on how to connect a UART.

{% hint style="info" %}
If you want to use RC over DroneBridge it is recommended to connect the Pi to the FC with an additional UART connection. This connection will transmit the SUMD RC messages to the FC. To get a second UART on the Pi you will need a FTDI adapter connected via USB on the AirPi.
{% endhint %}

## Configuration

DroneBridge is pre-configured to work out of the box with MAVLink based systems like the Pixhawk or Ardupilot. Then configuration options must be matched. This can be done via DroneBridge for Android or by manually editing the configuration files.

In this guide we will focus on the configuration files since not all functions are accessible via the DroneBridge for Android app. 

The configuration files are located on the SD card at `/DroneBridge/DroneBridgeConfig.ini` All settings in the `COMMON` section must match on AIR & GND!

{% hint style="info" %}
Configuration files for the OSD are located at `/DroneBridge/osdconfig.txt`
{% endhint %}

### MAVLink based flight controller

MAVLink messages are read by the control module on the AirPi and sent to the proxy module on the GndPi. Following settings must be set inside the configuration files:

On the GndPi inside the OSD configuration file:

```c
#define MAVLINK
```

On the AirPi inside the`[AIR]` section:

```bash
[AIR]
en_control=Y
serial_int_cont=/dev/serial1  # serial port connected to FC
baud_control=115200 # baud rate of UART/serial port
serial_prot=5  # for MAVLink transparent
pass_through_packet_size=64
```

### iNAV

{% hint style="danger" %}
Support for MultiWii based flight controllers is discontinued by the base project. DroneBridge will continue to support bi-directional MSPv1 & MSPv2 as part of a legacy support.
{% endhint %}

On the GndPi inside the OSD configuration file:

```c
#define LTM
```

On the AirPi inside the`[AIR]` section:

```bash
[AIR]
en_control=Y
serial_int_cont=/dev/serial1  # serial port connected to FC
baud_control=115200 # baud rate of UART/serial port
serial_prot=5  # for MSP & LTM transparent at the same time
# serial_prot=2 for MSP only (parsed, no OSD telemetry)
pass_through_packet_size=32
```

### Connecting an RC transmitter

You can use DroneBridge to control your UAV. Currently the FrSky i6S and all OpenTX based RCs are supported. Custom hardware can easily be integrated with just a few lines of code. Please take the OpenTX implementation as a reference.

1. Connect the RC via USB to the ground station
2. If possible: Disable the built-in transmitter of the RC to reduce RF noise

RC commands are sent to the UAV using lower bit rates then the video stream. This means that your RC range should be significantly higher than your video range. That way you are/should be able to control the UAV even when your video feed is already down. As of release v0.5 the RC implementation is not much tested. Please use with caution!

{% hint style="warning" %}
If you decide to switch the current RC hardware \(hot swap\) you need to reboot the ground station. Connecting and then disconnecting hardware \#1 followed by connecting hardware \#2 is not possible without reboot. If both RCs run on the same firmware a hot swap is possible without reboot.
{% endhint %}

Advantages of RC via DroneBridge over the transmitter built into the RC:

* DroneBridge set-ups generally offer a lot more range

## Notes to SUMD and UART

If you need more than the one UART that the Raspberry Pi offers you can also connect a FTDI adapter to your AirPi. This is recommended if you want to use SUMD as RC link to FC. SUMD output with DroneBridge Beta v0.3 is untested!

## Using multiple wifi cards

{% hint style="info" %}
As of release v0.6 DroneBridge supports diversity transmission on all built in modules
{% endhint %}

By default all connected WiFi adapters will be used for transmission. They are set to the same frequency.

In the DroneBridge configuration files you can specify `interface_selection`. Set it to `auto` and one wifi adapter is automatically selected for all modules.

Set `interface_selection` to `manual` and you can specify an interface for every module. So you can e.g. use one wifi stick for video and telemetry and the other one for RC. You can even set them to different frequencies to avoid packet loss and bad video. To change the interface for the control module e.g. set `interface_control=18a6f716a511` where `18a6f716a511` is the MAC address of the interface to be used. To change the frequency you need to edit the settings.

