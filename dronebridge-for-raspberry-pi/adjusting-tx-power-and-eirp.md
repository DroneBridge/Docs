---
description: How to change the TX power of the DroneBridge system
---

# Adjusting TX Power & EIRP

## Why change it?

This is a very important step if you want to operate the system legally. By default the DroneBridge system might have a too high EIRP depending on your setup and location.

_The output power might be too low \(not enough range\) or too high \(lost packets and max. EIRP\)_

_If you are using an un-patched Kernel/drivers and don't go crazy on the antennas you should be on the save side EIRP wise, but your hardware might not operate at the "range sweet spot"._

From the hardware section you can get an idea about the max possible power output of some WiFi adapters. If you do not know what EIRP is go read up on it! It depends on the TX power of your card and dB rating of your antenna. There is a max rating for it depending on the band \(2.4 GHz/5.x GHz\) you want to use. Because DroneBridge is a digital system the [max EIRP for DroneBridge in the EU is ](https://wlan1nde.wordpress.com/2014/11/26/wlan-maximum-transmission-power-etsi/)

* **100 mW \(20 dBm\) -** 1, 2, 5.5 and 11 Mbps, CCK modulation, **2.4 GHz** band
* **63 mW** **\(18 dBm\)** - g/n-Rates, OFDM modulation, **2.4 GHz**
* **100 mW - 1000 mW \(30 dBm\)** - **5.x Ghz**

As you can see the case is a little more complicated with the 5 GHz rage. Some cards in monitor mode might support DFS and TPC while others don't.

General rules are:

* **Stick to the ISM bands**
* **High power cards might need lower settings. Low power cards might need higher settings**
* **Stay away \(~1km+\) from others that might be affected by the extensive use of your set frequency**
* **Data rates of nearby WiFi networks are heavily affected by DroneBridge and vise versa**
* **Good antennas have a much higher effect on range than output power**
* **Too high power settings can damage your hardware, result in packet-loss and can lead to less range because the hardware is operated outside its sweet spot.**

## How to change?

You can measure the TX power of your card using a power meter. As for now there is now there is no general way of adjusting the power output. Depending on your chipset you need to change the following parameter:

### Atheros \(AR9271\)

_In general there is no need to further increase the TX power as the hardware already operates at its max by default._  
The the Ubiquiti Wifistation EXT you may need to lower the power to stop packetloss.

To change \(set via driver parameter\): 

* Go to the console and type  `txpower_atheros xx` where `xx` is a value from: `1-63` with `50` being the default value
* Reboot

### Ralink

To change \(set via driver parameter\): 

* Go to the console and type `txpower_ralink xx` where `xx` is a value from `-5 to 5` with `0` being the default value
* Reboot

### Realtek

Set via `iw tx-power 30` inside the start script. Change value in line  `pyw.txset(wifi_card, 'fixed', 3000)` inside `startup/init_wifi.py`

## Finding the right setting

* Use an rf power meter to be sure about the actual power output & EIRP
* Measure packet-loss inside shielded environment \(basement or inside of micro oven might be just fine\)
* Start with low parameters and then increase

