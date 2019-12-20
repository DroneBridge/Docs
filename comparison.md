---
description: >-
  How does DroneBridge compare to traditional WiFi connections & other similar
  projects
---

# Comparison

## Compared to traditional WiFi

1. Patched WiFi drivers inside the Kernel force the WiFi adapters to send at their power limit \(configurable\)
2. Forward Error Correction \(FEC\) of sensitive data \(video\) to account for packet loss
3. Broadcasting of data. Not connection oriented like with a WiFi AP. If your card can pick up a packet you will be able to read its data. No re-sending of packets re-authorisation on connection loss. This means no guaranteed delivery.
4. Typically lower data rates than with traditional WiFi \(18Mbit compared to &lt;150Mbit\) give more range
5. [More details on the original website](https://befinitiv.wordpress.com/wifibroadcast-analog-like-transmission-of-live-video-data/). DroneBridge and all forks still use the same principle

## Compared to forks of befinitivs WifiBroadcast

Relevant forks are:

* svpcoms WifiBroadcast \(basic TX-RX\)
* DroneBridge \(system design with library\)
* OpenHD \(follow-up on EZ-WifiBroadcast\)
* EZ-Wifibroadcast \(not under development\)

The base for all of the current projects is befinitivs WifiBroadcast. Based on that, EZ-WifiBroadcast was built with many improvements like Kernel patches for higher power output, OSD, telemetry, RC etc.. While the original WifiBroadcast is just a data link, all future forks focus on the use case in UAV applications. Custom data links are still possible with all variants.

Because the EZ-WifiBroadcast was missing a clear project structure, was more or less a bunch of scripts and did not have any kind of proper documentation, DroneBridge was built. The first releases were based on EZ-WifiBroadcast with extra programs onboard to replace some of the functionality. In parallel an app was developed to work with the system. As of release v0.6 DroneBridge does not use any WifiBroadcast code base except for the OSD, video player and FEC encoding/decoding.

Because of some disagreements regarding the future and technical development of the project, parts of the EZ-WifiBroadcast community forked OpenHD. OpenHD is built around an automated image builder.

## What to choose?

{% hint style="warning" %}
In short: The answer will be DroneBridge for almost all cases \(You are inside the DroneBridge Wiki. What did you expect!? :D\). In the special case that you only need a basic \(continuous\) data stream from one device to another and that requires FEC, it is best to choose svpcom WifiBroadcast. It is fast to setup and will get the job done just fine.
{% endhint %}

**Why DroneBridge then?** 

Compared to the other projects it was developed with a system design approach in mind from the very first moment on. There is a common software library that implements all core functions like sending & receiving data. This makes is very convenient and easy to develop the system. 

DroneBridge also enables users to write their own plugins. Plugins are standalone programs that get started right after boot. They can be just as powerful and performant as the core modules. You can write your own plugin in less than 10 minutes making full use of the DroneBridge common library. DroneBridge modules and the code base are very well documented. It is free of experimental scripts and follows the directive to be a general communication link. Implementations for custom hardware options \(camera switcher, GPIO in/output etc.\) can be implemented via the plugins. This means the base image is easy to understand and does not confuse new users with lots of badly documented extras that only a few people really use. 

Besides that DroneBridge also offers professional logging support, support for wired connections to all android devices \(No USB tethering, no SIM required\) and many more things. In fact it was the first project to introduce a custom app that integrated convenient setting changes, telemetry and low latency video display.

