---
description: >-
  DroneBridge is a system based on standard WiFi hardware and single board
  computers (Raspberry Pi). It is designed to send & receive data from and to a
  UAV running iNAV or MAVLink.
---

# Introduction

![DroneBridge logo](.gitbook/assets/dronebridgelogo_text.png)

DroneBridge is a system based on the WifiBroadcast approach. A bidirectional digital radio link between two endpoints is established using standard WiFi hardware and a custom protocol. DroneBridge is optimized for use in UAV/UAS applications and is a complete system. It is intended be a real alternative to other similar systems, such as DJI Lightbridge or OcuSync.

While it is optimized and designed for use in UAV applications it can also be used in any other scenario where

* **High data rate** \(1-54 Mbit\)
* **High range** \(0.5-35 km\)
* **Low latency** \(80 - 120 ms\)

is important. It features software modules supporting Forward Error Correction \(FEC\) and a common library for C/C++ and Python supporting software diversity with multiple WiFi adapters. All of this helps increasing range and reliability compared to a traditional WiFi connection. The underlying protocol is connectionless and can carry any payload.

**This place currently is under construction**

