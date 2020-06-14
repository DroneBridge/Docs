---
description: A list for all the supported hardware options you have with DroneBridge
---

# Supported hardware

## SOCs

{% hint style="danger" %}
With the v0.6 build there is no OSD support on the Pi4

The Raspberry Pi Zero suffers from startup stability issues. Its resources are very limited. Only use the Pi Zero if you do not have enough space or money. The Pi3A+ recommended instead.
{% endhint %}

{% hint style="warning" %}
**Recommended hardware:**   
Ground-Station: Raspberry Pi3B\(+\)  
Air-Unit: Raspberry Pi3A+
{% endhint %}

With a Pi3\(+\) you get more USB ports \(diversity, serial ports via FTDI etc.\) and more compute power \(more config options - FEC rate, less latency - ARMv7 instructions for packet encoding/decoding in the future\). 

* PiA+
* Pi1B+
* Pi2B
* Pi3B
* Pi3B+
* Pi4B          \(no OSD support in GND station mode\)
* Pi Zero      \(not recommended anymore\)
* Pi Zero W \(not recommended anymore\)

**Other untestet hardware that should work just fine**

* Odriod-W \(\*\)
* NVIDIA Jetson \(Nano\) \(works with RPi Camera v2 out of the box\) \(\*\)
* [Lychee](https://dronee.aero/pages/lychee)

{% hint style="warning" %}
\(\*\) For this hardware no ready made DroneBridge images are available. You must compile & setup the system by yourself. To get more outout power and to access all features it is necessary to patch the Kernel. See image build chapter.
{% endhint %}

## Cameras

A wide angle lens is recommended. There are some V1 models with that.

* Pi camera module V1
* Pi camera module V2+

Any other camera supported by linux should work as well. However you need to change the start scripts manually to feed the video to the video air module stdin. The latency might be higher than with Pi camera modules.

## Wifi Adapters

Following **chipsets** work:

* Atheros AR9271
* Ralink RT2070, RT2770, RT2870, RT3070, RT3071, RT3072, RT3370, RT3572, RT5370, RT5372, RT3573, RT5572
* MediaTek MT7601

**Experimental support added in v0.6**

* Realtek 8812au \(RTL8812au\)

### Examples for supported WiFi adapters

* CSL 300Mbit Stick \(2.4/5Ghz, Diversity, RT5572 chipset\)
* Alfa AWUS036NHA \(2.3/2.4Ghz, high power, Atheros AR9271 chipset\)
* Ubiquiti Wifistation USB \(2.3/2.4Ghz, high power, Atheros AR9271 chipset\)
* TPLink TL-WN722N V1 \(2.3/2.4Ghz, Atheros AR9271 chipset\)
* ALFA AWUS051NH v2 \(2.4Ghz/5Ghz, high power, Ralink RT3572 chipset\)
* ALFA AWUS052NH v2 \(2.4Ghz/5Ghz, Diversity, high power, Ralink chipset\)
* TP-Link-TL-WDN3200 \(2.4/5Ghz, Diversity, RT5572 chipset\)
* Rosewill RNX-N600UBE \(2.4/5Ghz, Diversity, RT5572 chipset, txpower unknown currently, RT5572 chipset\)
* AW-NU138 \(2.3/2.4Ghz, Atheros AR9271 chipset\)

### Recommended combinations

It is recommended to try and match chipsets on the UAV & GND station.

A single high power card may be on the UAS. On the ground you can decide for yourself. A high power card is not required since the set data rate is much lower. More than one card/antenna is recommended on the ground station.

**For 2.3/2.4/2.6 GHz**

* TP-Link TL-WN722N V1.x
* ALFA AWUS036NHA
* Ubiquiti Wifistation \(EXT\)

**For dual band \(2.4/5 GHz\)**

* ALFA AWUS036ACH
* other cards with external antenna and RTL8812au chipset.

For higher output power & range a high quality low noise amplifier \(LNA\) may be added to the antenna output of the cards. This may or may not improve the systems performance. Make sure you stay within the local regulations \(EIRP\).

{% hint style="danger" %}
RTL8814au chipsets are not supported due to unstable drivers. This means that cards like the ALFA AWUS1900 will not work!
{% endhint %}

### Notes

{% hint style="info" %}
This sections needs some work
{% endhint %}

TX-Power as reported by some users & the internet.

**IMPORTANT:** Make sure that you stay within the local EIRP limit. In many countries an EIRP &gt; 100mW is illegal! A range of 500m-1000m might not look like much at first glance, but it is enough to get 99% of all shots you might want to take when using DroneBridge for aerial photography and many other applications.

Some users demonstrated that ranges of 10km or even 34km are possible. However, because of legal concerns and safety precautions it is not recommended to push that far.

## Authors

* befinitiv - wifibroadcast
* Rodizio1 - EZ-Wifibroadcast project
* seeul8er - DroneBridge
* People of the internet and the rc-groups forum

