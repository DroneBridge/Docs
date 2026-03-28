---
description: What hardware to use and how to wire it all up
---

# Hardware & Wiring

{% hint style="info" %}
Docs. are WIP - More details to come, stay tuned :)
{% endhint %}

Drone Light Show Edition (DLSE) of DroneBridge for ESP32 supports the following chips:

* ESP32-C3 - Wi-Fi 5 2.4GHz
* ESP32-C5 - Wi-Fi 6 2.4GHz
* ESP32-C6 - Wi-Fi 6 2.4GHz & 5GHz

All chips must have at least 4MB of flash memory.

## ESP32-C3

Recommended GPIOs (NOT PIN NUMBERS) for connecting to the flight controller UART:

| ESP32-C3 GPIO TX | ESP32-C3 GPIO RX | ESP32-C3 GPIO RTS | ESP32-C3 GPIO CTS |
| ---------------- | ---------------- | ----------------- | ----------------- |
| GPIO 5           | GPIO 4           | GPIO 6            | GPIO 7            |

{% hint style="warning" %}
## Pins that cannot be used with the ESP32-C3 and the DLSE

GPIO 2, GPIO 8, GPIO 9, GPIO 12, GPIO 13, GPIO 14, GPIO 15, GPIO 16, GPIO 17
{% endhint %}

## ESP32-C5

Recommended GPIOs (NOT PIN NUMBERS) for connecting to the flight controller UART:

| ESP32-C5 TX GPIO | ESP32-C5 RX GPIO | ESP32-C5 RTS GPIO | ESP32-C5 CTS GPIO |
| ---------------- | ---------------- | ----------------- | ----------------- |
| GPIO 23          | GPIO 24          | GPIO 4 (untested) | GPIO 5 (untested) |

{% hint style="warning" %}
## Pins that cannot be used with the ESP32-C5 and the DLSE

GPIO 2, GPIO 7, GPIO 16, GPIO 17, GPIO 18, GPIO 19, GPIO 20, GPIO 21, GPIO 22, GPIO 25, GPIO 27, GPIO 28
{% endhint %}

## ESP32-C6

Recommended GPIOs (NOT PIN NUMBERS) for connecting to the flight controller UART:

| ESP32-C6 TX GPIO | ESP32-C6 RX GPIO | ESP32-C6 RTS GPIO | ESP32-C6 CTS GPIO |
| ---------------- | ---------------- | ----------------- | ----------------- |
| GPIO 21          | GPIO 2           | GPIO 22           | GPIO 23           |

{% hint style="warning" %}
## Pins that cannot be used with the ESP32-C6 and the DLSE

GPIO 4, GPIO 5, GPIO 8, GPIO 9, GPIO 15, GPIO 24, GPIO 25, GPIO 26, GPIO 27, GPIO 28, GPIO 29, GPIO 30
{% endhint %}
