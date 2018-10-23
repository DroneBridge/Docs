# Getting started

## Content

* [Basic hardware & setup](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#basic-hardware--setup)
* [MultiWii based flight controller \(iNAV\)](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#setup-multiwii-based-flight-controller)
* [MAVLink based flight controller](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#setup-mavlink-based-flight-controller)
* [Connecting an RC transmitter](https://github.com/seeul8er/DroneBridge/wiki/Setup-Guide/#connecting-a-rc-transmitter)

## Basic hardware & setup

The basic setup is exactly the same as with EZ-WifiBroadcast. [**Read the Wiki to find out more**](https://github.com/seeul8er/DroneBridge/wiki/Supported-Hardware) You need:

* **Two Raspberry Pis \(Pi Zero W should work but Pi 2 or 3 are recommended\)**
* **Two SD cards with min. 4GB storage**
* **A Pi camera \(v1 or v2\)**
* **Two WiFi sticks \(**[**Supported WiFi adapters**](https://github.com/seeul8er/DroneBridge/wiki/Supported-Hardware#wifi-adapters)**\)**

Just write the image onto the SD cards using e.g. [Win32 Disk Imager \(Windows\)](https://sourceforge.net/projects/win32diskimager/) or your favourite Linux tool \(e.g. dd or [some GUI tools](https://www.fossmint.com/3-best-gui-enabled-usb-image-writer-tools-on-linux/)\) Connect the Pi camera module to the raspberry pi that will be on the drone. Make sure your power supply for both Pis provides min. 2A current and stable 5V!

**For further details please read the instructions given here:** [**WifiBroadcast setup and installation guide**](https://github.com/bortek/EZ-WifiBroadcast/wiki#installation--setup)

### Setup: MultiWii based flight controller

**Supported flight controllers run iNAV \(recommended\) or MAVLink based systems** **The default setup is for iNAV which uses MSPv2**

With MultiWii based flight controllers you should use the multi port configuration of DroneBridge. This means that you have two serial connections between flight controller and AirPi. One is for LTM telemetry & the other one for MSP messages \(missions, RC etc.\). You can also use only one serial connection with LTM if you only need telemetry.

#### Hardware setup

* The Raspberry Pi has only one **3.3V UART** port. You connect this one to an UART on your FC **which also runs on 3.3V**. Otherwise use a regulator. _\(serial connection \#1 for telemetry\)_
* Most F3/F4/F7 flight controllers come with a micro USB connector which is by default configured as a MSP port to change settings via the configurator. Connect the FC to your Pi via a micro USB cable. _\(serial connection \#2 for MSP\)_
* **The FC usually gets powered by USB so do not connect any other power source to the FC.**

**Example setup using a Raspberry Pi 3 on the drone side**

Connect AirPi UART, which is _Pin 06_ \(Ground\), _Pin08_ \(GPIO14\), _Pin 10_ \(GPIO15\)\) to FC UART **\(has to be 3.3V or use regulator\)** Connect TX to RX, RX to TX and Ground to Ground. Setup telemetry over this UART on FC.

![pi 3 header](https://images.computerfrage.net/media/fragen/bilder/raspberry-pi-model-b-vergleichbare-pins/0_original.jpg?v=1408441560000)

#### DroneBridge configuration

By default the configuration of DroneBridge should work with iNAV.

**DroneBridgeGround.ini**

```text
en_control=Y  
en_comm=Y
```

**DroneBridgeAir.ini**

```text
en_tel=Y  
en_comm=Y  
en_control=Y
# serial interface for LTM & telemetry module
serial_int_tel=/dev/serial0
# auto|ltm|mavlink
tel_proto=auto
# serial interface for MSP & control module
serial_int_cont=/dev/serial1
# baud rate set on MSP interface
baud_control=115200
# 2 = MSPv2
serial_prot=2
# optional SUMD RC output
enable_sumd_rc=N  
serial_int_sumd=/dev/ttyUSB0
```

**osdconfig.txt**

_Remove the line \(if it exists - somewhere near the top\)_

```text
#define MAVLINK
```

_Add the line_

```text
#define LTM
```

### Setup: MAVLink based flight controller

Since DroneBridge Beta v0.3 the MAVLink protocol is supported by all modules \(control, telemetry, app\)

#### Hardware setup

* Connect your Raspberry Pi 3,3V UART to the `TELEM1` or `TELEM2` port of your flight controller.
* **You do not need to connect the red wire \(5V\) if you power your Pixhawk through some other device!**

![connection pi pixhawk](https://discuss.ardupilot.org/uploads/default/original/2X/f/f837b6b1116ec02c3490e34035c2f09da5a62936.jpg)

#### DroneBridge configuration

With MAVLink based flight controllers it is recommended that you use the single port config of DroneBridge with a transparent pass through. This means that only one serial connection exists between FC and AirPi. All data over this serial port is handled by the control module on the AirPi. The control module will send packets of 128 bytes each to the telemetry module on the GroundPi. Do not use this option if you do not have a constant stream of data!

**DroneBridgeGround.ini**

```text
en_control=Y
en_comm=Y
```

**DronebridgeAir.ini**

```text
# disable telemetry module (single port config)
en_tel=N
en_comm=Y
en_control=Y
# Serial interface of AirPi-UART connected to FC
serial_int_cont=/dev/serial0
# The baud rate the FC set on TELEM1
baud_control=115200
# Serial protocol 5 = transparent pass through
serial_prot=5
# Set to Y if you have a FTDI adapter connected and want to use RC over DroneBridge
enable_sumd_rc=N  
serial_int_sumd=/dev/ttyUSB0
```

**osdconfig.txt**

_Remove the line \(if it exists - somewhere near the top\)_

```text
#define LTM
```

_Add the line_

```text
#define MAVLINK
```

## Connecting an RC transmitter

You can use DroneBridge to control your UAV. Currently the FrSky i6S and all OpenTX based RCs are supported.

Steps: 1. Connect the RC via USB to the ground station 2. If possible: Disable the built-in transmitter of the RC to reduce rf noise \(better video\)

RC commands are sent to the UAV using lower bit rates then the video stream. This means that your RC range should be significantly higher than your video range. That way you are/should be able to control the UAV even when your video feed is already down. As of release v0.5 the RC implementation is not much tested. Please use with caution!

Known bugs:

* If you decide to switch the current RC hardware \(hot swap\) you need to reboot the ground station. Connecting and then disconnecting hardware \#1 followed by connecting hardware \#2 is not possible without reboot. If both RCs run on the same firmware a hot swap is possible without reboot.

Advantages of RC via DroneBridge over the transmitter built into the RC:

* DroneBridge set-ups generally offer a lot more range

## Notes to SUMD and UART

If you need more than the one UART that the Raspberry Pi offers you can also connect a FTDI adapter to your AirPi. This is recommended if you want to use SUMD as RC link to FC. SUMD output with DroneBridge Beta v0.3 is untested!

## Using multiple wifi cards

DroneBridge just as WifiBroadcast allows the use of more than one wifi adapter. Check out the EZ-WifiBroadcast Project for further information.

**Beware: This feature is untested.**

In the DroneBridge configuration files you can specify `interface_selection`. Set it to `auto` and one wifi adapter is automatically selected for all modules.

Set `interface_selection` to `manual` and you can specify an interface for every module. So you can e.g. use one wifi stick for video and telemetry and the other one for RC. You can even set them to different frequencies to avoid packet loss and bad video. To change the interface for the control module e.g. set `interface_control=18a6f716a511` where `18a6f716a511` is the MAC address of the interface to be used. To change the frequency you need to edit the wifibroadcast settings. Visit [WifiBroadcast config options](https://github.com/bortek/EZ-WifiBroadcast/wiki/Configuration-options)

PLEASE NOTE: As of release v0.5 DroneBridge modules do not support diversity reception!

