---
description: Fastest way to get started with DroneBridge for ESP32, plus links to the detailed guides.
icon: rocket
---

# Quick Start

This page is the fastest way to get started with the general-purpose **DroneBridge for ESP32** release.

{% hint style="info" %}
This page is for **DroneBridge for ESP32**.

It is **not** the [Drone Light Show Edition](../dronebridge-for-esp32-drone-light-show-edition/overview-drone-light-show-edition.md) and it is **not** the legacy [DroneBridge for Raspberry Pi](../dronebridge-for-raspberry-pi/getting-started.md) project.
{% endhint %}

DroneBridge for ESP32 is a lightweight telemetry and data bridge for supported ESP32 boards. It is easy to set up and works well for single-drone telemetry links, but it currently does **not** provide video transmission or radio control.

## Before You Start

You will typically need:

* One supported ESP32 board
* A USB cable for flashing and initial setup
* Access to a flight controller UART
* A Ground Control Station such as MissionPlanner or QGroundControl

{% hint style="warning" %}
DroneBridge is not designed for use as a safety-critical link and must not be relied on as the only communication link.

Before wiring or flying anything, read [Safety & Integration](safety-and-integration.md).
{% endhint %}

If you still need to choose hardware, start with [Hardware & Wiring](hardware-and-wiring.md).

## Recommended First Setup

For a first test, use a **single ESP32 in WiFi Access Point mode**. This is the simplest setup because it only needs one board and lets you connect directly to the ESP32 from your phone, tablet, or laptop.

{% stepper %}
{% step %}
### 1. Choose a supported board

Pick a supported ESP32 board and check whether it is an official board, a XIAO board, or a more generic development board.

-> [Hardware & Wiring](hardware-and-wiring.md)
{% endstep %}

{% step %}
### 2. Flash the firmware

Use the online flashing tool if possible. It is the easiest option for a first installation and helps you pick the correct firmware flavour.

-> [Installation](installation.md)
{% endstep %}

{% step %}
### 3. Wire the ESP32 to the flight controller

Connect `5V`, `GND`, `TX`, and `RX`. If you are not using UART flow control, leave `RTS` and `CTS` disconnected and disabled.

-> [Hardware & Wiring - Wiring to the Flight Controller](hardware-and-wiring.md#wiring-to-the-flight-controller)
{% endstep %}

{% step %}
### 4. Open the web interface

Power the ESP32, connect to the Wi-Fi network `DroneBridge for ESP32` using the password `dronebridge`, and open `http://dronebridge.local` or `http://192.168.2.1`.

-> [Configuration - Web Interface](configuration.md#web-interface)
{% endstep %}

{% step %}
### 5. Configure the UART and mode

Set the correct UART pins and baud rate for your hardware, then keep the device in **WiFi Access Point Mode** for the first test.

-> [Configuration](configuration.md)
{% endstep %}

{% step %}
### 6. Connect your Ground Control Station

Connect your GCS to the ESP32 link and verify that telemetry is arriving before trying more advanced modes.

-> [Configuration - MissionPlanner Configuration](configuration.md#missionplanner-configuration)

-> [Configuration - QGroundControl Configuration](configuration.md#qgroundcontrol-configuration)
{% endstep %}
{% endstepper %}

## Choose the Right Mode

Use the mode that matches your setup:

* **WiFi Access Point Mode**: Best first setup for a single drone and a direct connection to the ESP32
* **WiFi Client Mode**: Best when the ESP32 should join an existing Wi-Fi network
* **WiFi LR or ESP-NOW LR**: Best when you want more range and can also use ESP32 hardware on the ground side
* **Bluetooth LE**: Best for BLE-specific workflows and mobile-style setups, but it requires the BLE bridge application

Read the full mode details on [Configuration](configuration.md). If you are comparing WiFi-based multi-drone setups with ESP-NOW, also see [Drone Swarm Control](drone-swarm-control.md).

## Detailed Guides

<table data-view="cards">
    <thead>
        <tr>
            <th>Guide</th>
            <th data-card-target data-type="content-ref">Open</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Choose hardware and wiring</td>
            <td><a href="hardware-and-wiring.md">Hardware &amp; Wiring</a></td>
        </tr>
        <tr>
            <td>Flash the firmware</td>
            <td><a href="installation.md">Installation</a></td>
        </tr>
        <tr>
            <td>Configure modes and UART settings</td>
            <td><a href="configuration.md">Configuration</a></td>
        </tr>
        <tr>
            <td>See complete setup examples</td>
            <td><a href="hardware-setup-examples.md">Hardware Setup Examples</a></td>
        </tr>
        <tr>
            <td>Fix common problems</td>
            <td><a href="troubleshooting-help.md">Troubleshooting/Help</a></td>
        </tr>
        <tr>
            <td>Capture logs and debug startup issues</td>
            <td><a href="logging-and-debugging.md">Logging &amp; Debugging</a></td>
        </tr>
        <tr>
            <td>Build against the API or firmware</td>
            <td><a href="developer-and-api-documentation.md">Developer &amp; API Documentation</a></td>
        </tr>
    </tbody>
</table>

## Common First-Run Problems

If your first setup does not work right away, these pages will usually get you unstuck quickly:

* The access point is not visible: [Troubleshooting/Help](troubleshooting-help.md#i-flashed-and-the-access-point-is-not-showing-up)
* The web interface does not load: [Troubleshooting/Help](troubleshooting-help.md#the-web-interface-is-not-loading)
* No telemetry shows up in the GCS: [Troubleshooting/Help](troubleshooting-help.md#i-see-no-data-in-my-ground-station)
* You are locked out after changing settings: [Troubleshooting/Help](troubleshooting-help.md#i-am-locked-out-how-do-i-reset-my-esp32-running-dronebridge)
