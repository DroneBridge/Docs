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

