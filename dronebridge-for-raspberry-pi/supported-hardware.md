---
description: A  list for all the supported hardware options you have with DroneBridge
---

# Supported hardware

## SOCs

NOTE: **The ground station must be at least a Pi2. A Pi3 is recommended for the Air- and Ground-Side.** With a Pi3 you get more USB ports \(diversity, serial ports via FTDI etc.\) and more compute power \(more config options - FEC rate, less latency - ARMv7 instructions for packet encoding/decoding in the future\). Only use the Pi Zero if you do not have enough space or money.

* PiA+
* Pi1B+
* Pi2B
* Pi3B
* Pi3B+ \(experimental with Beta v0.6\)
* Pi4B \(experimental with Beta v0.6\)
* Pi Zero
* Pi Zero W

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

Any other camera supported by linux should work as well. However you need to change the start scripts manually to feed the video to the video air module stdin.

## Wifi Adapters

Following chipsets should work:

* Atheros AR9271
* Ralink RT2070, RT2770, RT2870, RT3070, RT3071, RT3072, RT3370, RT3572, RT5370, RT5372, RT3573, RT5572
* MediaTek MT7601

**Experimental support added in v0.6+. Not much tested, no power patches, might experience a lot of packet loss**

* Realtek 8812au \(RTL8812au\)

### Examples

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

It is a good idea to try and match chipsets on the UAV & GND station.

A single high power card may be on the UAS. On the ground you can decide for yourself. A high power card is not required since the set data rate is much lower. More than one card/antenna is recommended on the ground station.

**For 2.3/2.4/2.6 GHz**

* TP-Link TL-WN722N V1.x
* ALFA AWUS036NHA
* Ubiquiti Wifistation \(EXT\)

**For dual band \(2.4/5 GHz\)**

* ALFA AWUS1900 - good ground station
* ALFA AWUS036ACH

For higher output power & range a high quality low noise amplifier \(LNA\) may be added to the antenna output of the cards. This may or may not improve the systems performance. Make sure you stay within the local regulations \(EIRP\).

### Notes

{% hint style="info" %}
### This sections needs some work
{% endhint %}

TX-Power as reported by some users & the internet.

**IMPORTANT:** Make sure that you stay within the local EIRP limit. In many countries an EIRP &gt; 100mW is illegal! A range of 500m-1000m might not look like much at first glance, but it is enough to get 99% of all shots you might want to take when using DroneBridge for aerial photography and many other applications.

Some users demonstrated that ranges of 10km or even 34km are possible. However, because of legal concerns and safety precautions it is not recommended to push that far.

#### TPLink TL-WN722N V1

* TX-Power: ~60mW 
* Range: ~800-1000m with 2.1dBi stock antennas

**IMPORTANT:** Some sellers ship the newer version \(V2\) instead of the supported V1 & V1.1 hardware revision. The V2 is using a different chipset and is not supported by DroneBridge.

**Note:** The PCB/chip has got a second antenna port. Soldering a second antenna to it might increase the robustness of the link.

#### AWUS036NHA

* TX-Power: ~280mW 
* Range \(2.4GHz\): &gt;1km with directional antennas

#### Ubiquit Wifistation USB

* TX-Power: Value unknown but one of the highest out there

#### AW-NU138

* TX-Power: ~50mW

Internal antenna can be replaced by external antenna. Needs good cooling.

#### CSL 300Mbit stick

* TX-Power: ~30mW
* Range \(5GHz\): ~200-300m

#### AWUS051NH & AWUS052NH

* TX-Power: ~330mW 
* Range \(5GHz\): ~800-1000m

## Authors

* befinitiv - wifibroadcast
* Rodizio1 - EZ-Wifibroadcast project
* seeul8er - DroneBridge
* People of the internet and the rc-groups forum

