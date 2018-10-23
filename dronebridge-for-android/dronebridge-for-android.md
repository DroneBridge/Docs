# DroneBridge for Android

## Features

* Display low latency video stream of wifibroadcast \(USB Tethering or Wifi-AP\)
* Implementation of DroneBridge communication protocol
* Display RC link quality from DroneBridge control module
* User Interface to change settings of DroneBridge modules and Wifibroadcast
* Full support for LTM-Protocol
* Full support for MAVLink Telemetry
* Show home point and drone on map
* Calculate battery percent from ampere or voltage using a battery model
* Show Wifibroadcast status: bad blocks/lost packets - bitrate and link quality
* Display distance between pilot \(app\) and drone
* Support for MSPv1 \(Betaflight/Cleanflight\) and MSPv2 \(iNAV\)

## Installation and Requirements

While still in beta phase you will need to download the latest DroneBridge App release from links provided in this repository.

* Inside your phones settings you must activate "install apps from unknown sources"
* Phone with min. Android API 21 \(Lollipop\)
* Despite massive optimizations this app still is quiet demanding. Low cost phones may display artifacts

Tested Hardware:

* OnePlus X
* Sony Xperia Z4 Tablet

Do I need the provided DroneBridge image for my Raspberry Pi?

|  |  | supported by project image file |  |
| :--- | :--- | :--- | :--- |
|  |  | DroneBridge image | EZ-WifiBroadcast image |
| In app function | Video | ✓ | ✓ |
| LTM Telemetry | ✓ |  |  |
| Change Settings \(communication module\) | ✓ |  |  |

## How to connect to Raspberry Pi?

You can connect your phone to the raspberry pi via USB-Tethering or Wifi. In urban areas or if you are using the 2.4Ghz band for the long range radio link it is recommended to use USB-Tethering.

### Set up USB-Tethering

Power up the groundstation running the latest DroneBridge release. Wait about eight seconds or till you see your wifi adapter blinking. Then connect your phone to the Pi using a USB cable. **Do not connect before or right after power up. This might cause WifiBroadcast to detect it as a USB Stick. The boot sequence may be aborted!** Go to your android wifi settings and find the setting to activate USB-Tethering. Launch the DroneBridge App.

### Set up Wifi

To connect via wifi you need to activate the wifi hotspot on the Raspberry Pi. If your Pi does not have a built in wifi module you will need a extra wifi adapter. Only Raspberry Pi 3 and Pi Zero W have built in wifi. By default the underlying WifiBroadcast will use the built in wifi chip.

SSID: `DroneBridge`

Password: `dronebridge`

#### Using the DroneBridge App

To activate the wifi hotspot use the DroneBridge app over USB-Tethering and enable `Wifi Hotspot` under `Settings` --&gt; `Wifibroadcast`. **Do not forget to request the current configuration first and then click the save button.**

#### Manual

Insert the SD card into your computer and edit the `wifibroadcast-1.txt` file. Set `WIFI_HOTSPOT=Y`

#### Changing the adapter for wifi hotspot

Insert the SD card into your computer and edit the `wifibroadcast-1.txt` file. Set `WIFI_HOTSPOT_NIC=internal` or the MAC address of the USB wifi card you want to use

## The UI

![ui elements with explanation](https://github.com/seeul8er/DroneBridge/blob/master/wiki/ui_explain.png) 

![dronebridge settings](https://github.com/seeul8er/DroneBridge/blob/master/wiki/dronebridge_settings.png)

## Displaying video with the standard EZ-WifiBroadcast image

Changing settings with the app and display telemetry data currently only works with the DroneBridge images. If you are just interested in the video feed you can also go with the images provided directly on the [EZ-WifiBroadcast Github page](https://github.com/bortek/EZ-WifiBroadcast/wiki). Set `FORWARD_STREAM=raw` and `VIDEO_UDP_PORT=5000` in your `wifibroadcast-1.txt` settings file.

## Do more!

Replace your EZ-GUI with the DroneBridge app to monitor your flights. Integrate it into your own projects. Connect Wifi SOCs like ESP32 to your phone and get rid of the Raspberry Pis. All the key protocols and interfaces are open and documented. Make it yours!

The DroneBridge app listens for following data:

| Data  | Port  | Protocol | Format |
| :--- | :--- | :--- | :--- |
| Video  | 5000 | UDP | raw H264 stream  |
| LTM Telemetry MAVLink telemetry | 1604 | UDP | one LTM frame per packet constant MAVLink stream |

