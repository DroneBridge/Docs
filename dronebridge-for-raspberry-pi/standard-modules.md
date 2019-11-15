---
description: Information on all standard modules on what they do and their inputs & outputs
---

# Standard Modules

## General

All modules use the common library to send & receive data via the long range link. Some modules exist in different implementations depending where they are running \(UAV, GND station\).

## Video Module

Capable of transmitting a continuous stream of data that is protected by forward error correction \(FEC\). **Any kind of data** can be fed to the UAV module and is stream to the ground.

**UAV**

Input of data stream via STDIN. FEC packets are computed and sent to GND interleaved with data packets. FEC parameters and packet size can be set via config/command line parameters.

`video_air -n <networkInterface> -f <frametype>`

**GND**

Data from the UAV module is received and FEC decoded. Depending on the configuration the decoded data is sent to specified outputs. UDP endpoints can register with the module by sending a &gt;0 length UDP packet. This will change the UDP receiver to the senders IP. Destination port is `5000`.

## Control Module

TODO

## Proxy Module

TODO

## Status Module

TODO

## USB Bridge

TODO

## OSD

TODO

## Syslog Server

TODO



