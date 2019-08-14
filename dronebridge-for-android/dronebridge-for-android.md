# DroneBridge for Android

## Features

* Connection via Wifi-AP or DroneBridge USBBridge using Android Accessory Protocol
* Display low latency video stream of DroneBridge
* Implementation of DroneBridge communication protocol
* Display RC link quality from DroneBridge control module
* User Interface to change settings of DroneBridge modules
* Full support for LTM-Protocol
* Support for most important MAVLink messages
* Show home point and drone on map
* Calculate battery percent from ampere or voltage using a battery model
* Show video link status: bad blocks/lost packets - bitrate and link quality
* Display distance between pilot \(app\) and UAS
* Multi-Window support. Use DroneBridge for Android & QGroundcontrol side by side on a tablet
* Tablet & Phone layouts
* Deprecated: Support for MSPv1 \(Betaflight/Cleanflight\) and MSPv2 \(iNAV\)

## Installation and Requirements

While still in beta phase you will need to download the latest DroneBridge App release from links provided in this repository.

* Inside your phones settings you must activate "install apps from unknown sources"
* Phone with min. Android API 21 \(Lollipop\)

Tested Hardware:

* OnePlus X
* Sony Xperia Z4 Tablet

Do I need the provided DroneBridge image for my Raspberry Pi?

## How to connect to Raspberry Pi?

You can connect your phone to the raspberry pi via USB-Tethering or WiFi. In urban areas or if you are using the 2.4 GHz band for the long range radio link it is recommended to use USB-Tethering.

### USB \(recommended\)

Power up the ground station running the latest DroneBridge release \(0.6+\). Wait about eight seconds or till you see your WiFi adapter blinking. Then connect your phone to the Pi using a USB cable. DroneBridge for android should launch automatically.

### WiFi

{% hint style="info" %}
The hotspot can be activated setting the following parameters inside `DroneBridgeConfig.ini`

```typescript
wifi_ap=Y
wifi_ap_if=internal
```
{% endhint %}

To connect via WiFi you need to activate the wifi hotspot on the Raspberry Pi. If your Pi does not have a built in wifi module you will need a extra wifi adapter. By default the system uses the built in wifi chip. You can change the WiFi channel & settings by editing`/DroneBridge/apconfig.txt`

* SSID: `DroneBridge`
* Password: `dronebridge`

## The UI

![App UI with description](https://raw.githubusercontent.com/seeul8er/DroneBridge/master/wiki/ui_explain.png)

![](https://raw.githubusercontent.com/seeul8er/DroneBridge/master/wiki/dronebridge_settings.png)

## Do more!

Replace your EZ-GUI/QGroundcontrol with the DroneBridge app to monitor your flights. Integrate it into your own projects. Connect Wifi SOCs like ESP32 to your phone and get rid of the Raspberry Pis. All the key protocols and interfaces are open and documented. Make it yours!

The DroneBridge app listens for following data:

| Data  | Port  | Protocol | Format |
| :--- | :--- | :--- | :--- |
| Video  | 5000 | UDP | raw H.264 stream |
| LTM Telemetry MAVLink | 14556 | TCP | Connects to specified port |

