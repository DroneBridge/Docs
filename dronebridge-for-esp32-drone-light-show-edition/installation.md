---
description: How to flash the DLSE firmware to your ESP32 module.
---

# Installation

{% hint style="warning" %}
**Before you flash:** Make sure you have read the [Safety & Integration Guideline](safety-and-integration-guideline.md) and completed the [Hardware & Wiring](hardware-and-wiring.md) steps first.
{% endhint %}

## What You Will Flash

The DLSE firmware is distributed as a set of binary files - one set per supported chip (C3, C5, C6). Each set contains all the firmware partitions needed to fully program the ESP32.

By default, a freshly flashed ESP32 starts in **TRIAL** license mode, which cuts the MAVLink connection after 10 minutes. This is sufficient for initial setup and bench testing, but you will need to obtain a proper license before using the device in a show. Register at [drone-bridge.com/dlse](https://drone-bridge.com/dlse/), buy license credits, and then activate each ESP32 individually. See [License](license.md).

***

## Flashing Methods

{% tabs %}
{% tab title="Online Flasher (Easiest)" %}
The online flasher runs in your browser and requires no software installation. It works on Windows, macOS, and Linux.

{% stepper %}
{% step %}
#### Open the flasher

Go to [https://drone-bridge.com/flasher/](https://drone-bridge.com/flasher/) in a **Chrome or Edge** browser (Web Serial API required - Firefox is not supported).
{% endstep %}

{% step %}
#### Connect your ESP32

Plug the ESP32 into your computer via USB. If this is the first time connecting, your OS may need to install a USB-to-Serial driver (usually happens automatically on Windows; may require `cp210x` or `ch340` driver depending on your module).
{% endstep %}

{% step %}
#### Select the firmware

Click **Connect**, allow the browser to access the serial port, and select your device from the list.\
The flasher will auto-detect the chip and present the available DLSE firmware releases. Select the desired DLSE release.
{% endstep %}

{% step %}
#### Flash

Click **Flash** and wait for the process to complete. Do not disconnect the ESP32 during flashing.\
Once done, unplug and re-plug the ESP32 to reboot into the new firmware.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Offline (esptool)" %}
Use this method if the online flasher is unavailable or if you need to automate flashing across a large fleet.

{% stepper %}
{% step %}
#### Install Python and esptool

You need Python 3.10 or newer.

```bash
pip install esptool
pip install setuptools
```

Verify the installation:

```bash
esptool.py version
```
{% endstep %}

{% step %}
#### Download the firmware

Download the latest DLSE beta release from [https://drone-bridge.com/dlse/](https://drone-bridge.com/dlse/).

Extract the archive. Inside you will find:

* Firmware binary files (one set per chip)
* A `flashing_instructions.txt` file with the exact `esptool` command for your chip
{% endstep %}

{% step %}
#### Connect your ESP32

Plug the ESP32 into your computer via USB. Identify the serial port:

* **Linux/macOS:** Usually `/dev/ttyUSB0` or `/dev/ttyACM0`. Run `ls /dev/tty*` before and after connecting to find it.
* **Windows:** Check Device Manager for the COM port number (e.g., `COM3`).
{% endstep %}

{% step %}
#### Flash the firmware

Open `flashing_instructions.txt` and copy the command for your chip. It will look similar to this (your command may differ - always use the one from the instructions file):

```bash
esptool.py --chip esp32c6 --port /dev/ttyUSB0 --baud 460800 \
  write_flash --flash_mode dio --flash_size 4MB \
  0x0 bootloader.bin \
  0x8000 partition-table.bin \
  0x10000 dronebridge_dlse.bin
```

Replace `/dev/ttyUSB0` with your actual serial port. Run the command and wait for it to complete.
{% endstep %}

{% step %}
#### Reboot

Once flashing is complete, unplug and re-plug the ESP32. It will boot into the DLSE firmware.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Flashing a Fleet" %}
For deploying DLSE across many drones, use the open-source **Commercial Support Suite**:

[DroneBridge DLSE Commercial Support Suite on GitHub](https://github.com/DroneBridge/DLSECommercialSupportSuite)

The suite provides Python scripts for:

* Bulk flashing, activation & configuration over USB
* Bulk  Over-The-Air Updates
* Automated configuration via the REST API
* License activation for multiple ESP32 devices
{% endtab %}
{% endtabs %}

***

## Verifying the Installation

After rebooting, the DLSE firmware starts a Wi-Fi Access Point (AP) by default. You can verify the firmware is running correctly:

{% stepper %}
{% step %}
#### Connect to the ESP32 Wi-Fi AP

On your laptop or phone, scan for Wi-Fi networks. You should see a new network named **`DroneBridgeESP32`** (or similar, depending on the configured SSID). Connect to it using the default password `dronebridge`.
{% endstep %}

{% step %}
#### Open the web interface

Open a browser and navigate to **`http://192.168.2.1`**.\
You should see the DLSE configuration web interface. If you see it, the firmware is running correctly.
{% endstep %}

{% step %}
#### Check the license status

In the web interface, find the **License** section. It should show **TRIAL** as the current license type. This confirms the firmware is active.
{% endstep %}
{% endstepper %}

After confirming the device boots correctly in TRIAL mode, the next step is usually to register your DLSE account, purchase license credits, and activate the module before flight use.

***

## Next Steps

<a href="safety-and-integration-guideline.md" class="button secondary">Read Safety &#x26; Integration Guideline</a> <a href="hardware-and-wiring.md" class="button secondary">Wire to Flight Controller</a> <a href="configuration.md" class="button primary">Configure the Firmware -></a>
