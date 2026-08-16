---
description: >-
  How does DroneBridge for Raspberry Pi compare to traditional WiFi connections
  & other similar projects
---

# Comparison

## Compared to traditional WiFi

1. Patched WiFi drivers inside the Kernel force the WiFi adapters to send at their power limit (configurable)
2. Forward Error Correction (FEC) of sensitive data (video) to account for packet loss
3. Broadcasting of data. Not connection oriented like with a WiFi AP. If your card can pick up a packet you will be able to read its data. No re-sending of packets re-authorisation on connection loss. This means no guaranteed delivery.
4. Typically lower data rates than with traditional WiFi (18Mbit compared to <150Mbit) give more range
5. [More details on the original website](https://befinitiv.wordpress.com/wifibroadcast-analog-like-transmission-of-live-video-data/). DroneBridge and all forks still use the same principle

## **Why DroneBridge?**&#x20;

Compared to the other projects it was developed with a system design approach in mind from the very first moment on. There is a common software library that implements all core functions like sending & receiving data. This makes is very convenient and easy to develop the system.&#x20;

DroneBridge also enables users to write their own plugins. Plugins are standalone programs that get started right after boot. They can be just as powerful and performant as the core modules. You can write your own plugin in less than 10 minutes making full use of the DroneBridge common library. DroneBridge modules and the code base are very well documented. It is free of experimental scripts and follows the directive to be a general communication link. Implementations for custom hardware options (camera switcher, GPIO in/output etc.) can be implemented via the plugins. This means the base image is easy to understand and does not confuse new users with lots of badly documented extras that only a few people really use.&#x20;

Besides that DroneBridge also offers professional logging support, support for wired connections to all android devices (No USB tethering, no SIM required) and many more things. In fact it was the first project to introduce a custom app that integrated convenient setting changes, telemetry and low latency video display.

