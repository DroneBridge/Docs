---
description: Drone Light Show Edition (DLSE) of DroneBridge for ESP32 — what it is, how it fits into your show system, and how to get started.
---

# Overview — Drone Light Show Edition

{% hint style="info" %}
**This section is exclusively for the Drone Light Show Edition (DLSE) of DroneBridge for ESP32.**\
For the general-purpose release, see the DroneBridge for ESP32 section.
{% endhint %}

## What is the DLSE?

The **Drone Light Show Edition (DLSE)** is a firmware for the ESP32 microcontroller. It turns a small, low-cost ESP32 module into a **dedicated Wi-Fi telemetry bridge** between your drone's flight controller and your ground control software.

In a drone light show, every drone in the swarm needs a reliable wireless data link. The flight controller (running [Skybrush](https://skybrush.io/) / Lightbrush firmware) receives mission commands and sends telemetry back — all over Wi-Fi. The ESP32 running DLSE **is that Wi-Fi bridge**, sitting on each drone between the flight controller's UART port and your Wi-Fi network.

## System Architecture

A typical drone light show system using DLSE looks like this:

```
[ Skybrush Live (GCS) ]
         │
         │  Ethernet
         ▼
[ Wi-Fi Access Point ]
         │
         │  Wi-Fi
         ▼
[ ESP32 running DLSE ]  ← on every drone
         │
         │  UART
         ▼
[ Flight Controller (Skybrush / Lightbrush firmware) ]
```

The ESP32 is wired directly to one of the flight controller's UART telemetry ports. Depending on your drone design, additional hardware such as a power management board may also be connected to the ESP32 to enable remote power control of the drone.

## Why DLSE?

DroneBridge for ESP32 also has a free general-purpose release. That version is suitable for hobby use and single-drone setups, but it is **not designed for, tested for, or supported in drone light show operations**. Drone light shows have requirements that simply do not exist in single-drone use: fleet-scale OTA updates, remote power management, tight Skybrush Live integration, and the kind of robustness and reliability that commercial operations demand.

The DLSE was built specifically to meet those requirements:

* **Skybrush Live integration** — auto-detected as a MAVLink component, no manual IP configuration needed in the GCS
* **Remote sleep & wakeup** — the GCS can power the flight controller on or off via the ESP32's power management GPIO, enabling fully automated pre-show and post-show workflows
* **OTA firmware updates** — update the firmware on every drone in the fleet over Wi-Fi, without touching a single USB cable
* **Improved reliability and robustness** — significantly tightened code quality, extensive additional safety checks throughout the firmware, and reduced complexity compared to the general-purpose release — all with drone show operations specifically in mind
* **Reduced network load** — unnecessary services are disabled, so each ESP32 puts less traffic on the shared show network as the fleet grows
* **ESP32-C5 support** — first to support the C5 chip with dual-band Wi-Fi capability
* **Commercial Support Suite** — open-source Python tooling for bulk flashing, configuration, and license activation across the whole fleet
* **Show-specific documentation and parameters** — every setting is documented in the context of a Skybrush/Lightbrush setup

{% hint style="info" %}
If you are setting up a drone light show fleet with Skybrush Live, the DLSE is the correct choice. The general-purpose release is not a supported path for this use case.
{% endhint %}

## Licensing

The DLSE uses a per-device license model. By default, every ESP32 starts in **TRIAL** mode, which limits operation to 10 minutes before the telemetry link is cut — this is not suitable for actual shows. See the [License](license.md) page for the full overview of license types and how to obtain one.

## Getting Started

Follow the pages in this section in order:

{% stepper %}
{% step %}
### Choose and source your hardware

Learn which ESP32 chip is right for your fleet and what physical module to buy.\
→ [Hardware & Wiring](hardware-and-wiring.md)
{% endstep %}

{% step %}
### Install the DLSE firmware

Flash the DLSE firmware to each ESP32 using esptool or the online flasher.\
→ [Installation](installation.md)
{% endstep %}

{% step %}
### Read the Safety & Integration Guideline

Before wiring or flying anything, understand the safety requirements and constraints.\
→ [Safety & Integration Guideline](safety-and-integration-guideline.md)
{% endstep %}

{% step %}
### Wire the ESP32 to your flight controller

Connect the ESP32 to the flight controller UART port and to the power management board.\
→ [Hardware & Wiring — Wiring](hardware-and-wiring.md#wiring)
{% endstep %}

{% step %}
### Configure the firmware

Set up Wi-Fi credentials, UART pins, MAVLink parameters, and power management via the web interface or REST API.\
→ [Configuration](configuration.md)
{% endstep %}

{% step %}
### Obtain and activate a license

Request an EVALUATION or ACTIVATED license for each ESP32 in your fleet.\
→ [License](license.md)
{% endstep %}
{% endstepper %}

## Feature Overview

<table data-full-width="false"><thead><tr><th>Feature</th><th>Description</th></tr></thead><tbody>
<tr><td>Wi-Fi Telemetry Bridge</td><td>Forwards MAVLink traffic between the flight controller UART and the Wi-Fi network</td></tr>
<tr><td>Skybrush Live Integration</td><td>Auto-detected as a drone component — no manual IP configuration needed in the GCS</td></tr>
<tr><td>Remote Sleep &amp; Wakeup</td><td>The GCS can power-cycle the flight controller via the ESP32's power management GPIO</td></tr>
<tr><td>OTA Firmware Updates</td><td>Update DLSE firmware over Wi-Fi — no USB cable needed per drone</td></tr>
<tr><td>Improved Reliability &amp; Safety</td><td>Significantly tightened code quality with many additional checks throughout the firmware, designed with drone show operations in mind</td></tr>
<tr><td>Reduced Network Load</td><td>Unnecessary services disabled — scales cleanly to large swarms</td></tr>
<tr><td>ESP32-C5 Support</td><td>The only supported chip with dual-band Wi-Fi (2.4 GHz &amp; 5 GHz) — recommended for all new designs</td></tr>
<tr><td>Commercial Support Suite</td><td>Open-source Python tooling for bulk flashing, configuration, and fleet management</td></tr>
</tbody></table>
