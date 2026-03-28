# Installation

{% hint style="info" %}
For now, there is only an offline flashing method available since the online flasher still causes issues. Check the flashing instructions within the release file.

Instructions on how to flash using offline tools and how to activate & configure your firmware on the fly will follow here on this page.
{% endhint %}

## Beta Release v1.0.0

{% hint style="danger" %}
Use at your own risk!

By default, an unactivated Drone Light Show Edition runs in trial license mode. In this mode, the MAVLink connection is stopped after 10 minutes of operation.

**You will lose connection to your drone via the ESP32! Make sure you have a backup link.**&#x20;

It is not recommended to perform extended flight testing using this mode.

Licenses can be obtained directly by contacting seeul8er via Discord Support. The online shop will open soon, allowing you to simply buy as many licenses as you need.
{% endhint %}

[**Download BETA v1.0.0 here!**](https://drone-bridge.com/dlse/)

### Installation Instructions

1. [Get the esptool from espressif](https://docs.espressif.com/projects/esptool/en/latest/esp32c6/index.html) ([more details here](https://docs.espressif.com/projects/esptool/en/latest/esp32c6/installation.html#global-installation))
   1. Get Python 3.10 or newer
   2. Run `pip install esptool`
   3. Run `pip install setuptools`
2. Connect your ESP32 and flash the firmware with the command supplied within the `flashing_instructions.txt` for your ESP32 chip
3. [Read and implement the Safety & Integration Guideline](safety-and-integration-guideline.md)

**Alternative:**

[Use the online flashing tool!](https://drone-bridge.com/flasher/)
