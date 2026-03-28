---
description: How to flash the firmware to the hardware.
---

# Installation

## Firmware Options

DroneBridge for ESP32 offers various firmware flavours for every release. Depending on your needs and hardware, you can choose from the following options:

<table data-full-width="true"><thead><tr><th>Firmware flavour</th><th>Indended Use-Case</th><th>Description</th></tr></thead><tbody><tr><td>ESP32(-XX)</td><td>ESP32s connected to the flight controller via UART.<br>ESP32s connected to a GCS via an external Serial-to-USB adapter</td><td>Generic standard firmware. <br>If in doubt use this one!</td></tr><tr><td>ESP32(-XX) Official HW</td><td>Like the standard firmware above.<br>For official hardware boards operating as AIR-Units.</td><td>Generic standard firmware. Pre-configured UART pins for the official hardware.</td></tr><tr><td>ESP32(-XX) <code>(USBSerial)</code></td><td>For official hardware boards operating as GND-Unit using the onboard USB connector.<br>ESP32s connected to a GCS via the onboard USB connector via the USB-JTAG interface of the ESP32.</td><td>Only some boards support this firmware. They must have the USB-JTAG interface available.<br>All telemetry is forwarded to the USB-JTAG interface instead of a UART.</td></tr><tr><td>ESP32(-XX) <code>(noUARTConsole)</code></td><td>For generic development boards with an onboard Serial-to-USB chip operating as GND-Unit.<br>E.g. ESP32-S2-DevKitM-1, ESP32-C3-DevKitM-1, ESP32-DevKitM-1<br>This firmware is not available for the ESP32 (Classic).</td><td>Debugging UART of the ESP32 is disabled. UART can now be configured by DroneBridge for telemetry output to a GCS via USB. This removes the need for an additional external Serial-to-USB adapter. Check the manufacturer's data sheet for the TX &#x26; RX PINs.<br>This firmware is not available for the ESP32 (Classic).</td></tr></tbody></table>

{% hint style="warning" %}
**Known Issues with QGroundControl**

Regarding the use of QGroundControl with the `USBSerial` firmware:\
The GND-Unit ESP32 must be reset after every disconnect of QGroundControl. Press the reset button on the board once, then reconnect.

Regarding the use of QGroundControl with the `noUARTConsole` firmware:\
The GND-Unit ESP32 must be **re-configured** after every disconnect of QGroundControl. QGroundControl is currently triggering a reset of the settings on reconnect.
{% endhint %}

### Some Background Information:

In general, the older ESP32s just have plain UARTs. So the development boards usually come with an onboard Serial-to-USB chip that just connects to the ESP32s debugging UART so it can be accessed via the PC conveniently (no additional hardware required).&#x20;

The newer ESP32 C3 and C6 boards have a USB-JTAG interface as well. Using that interface, you do not need a dedicated Serial-to-USB chip anymore. So that is why some development boards for the C3 & C6 chips come without one. For these boards, DroneBridge offers the `USBSerial` firmware. It deactivates the debugging console on the USB-JTAG interface and uses it for transporting the telemetry stream instead. This means you can just connect your PC to the USB port of the ESP32 and receive telemetry. No additional Serial-to-USB adapter is required.

For the older ESP32 boards with no USB-JTAG interface, DroneBridge offers the `noUARTConsole` firmware flavour. This deactivates the debugging output on the UART/USB port. You can now configure these pins in the web interface.

## Installing DroneBridge for ESP32

You have three options to flash/install DroneBridge for ESP32 to your device.

1. Official Online Flashing Tool for DroneBridge for ESP32 (recommended)
2. Use the official espressif python-based tool esptool.py

In any case, go and conduct the following steps first:

1. Connect the ESP32 via USB to your computer
2. [Determine the serial port number](https://www.pololu.com/docs/0J67/4.5) e.g. via the Windows Device Manager - in this example it will be COM4

{% hint style="info" %}
If your board is running the `USBSerial` flavour of the firmware, and if you want to flash a different firmware, you must press and hold the boot button (on the official board labelled with "Reset Settings" or "R" on the official case) of the ESP32 while powering up. \
After that, you can release the button. \
Only then will the flashing tool be able to detect the board and flash a new firmware.&#x20;
{% endhint %}

## Official Flashing Tool

This is the most convenient and fool-proof way to install DroneBridge for ESP32. It does not require any download and it works on all platforms. \
You will love it (at least I hope so).

<figure><img src="../.gitbook/assets/grafik (6).png" alt="" width="401"><figcaption><p>UI showing the official flashing tool</p></figcaption></figure>

1. [Go to the installation page](https://dronebridge.github.io/ESP32/install.html)
2. Connect the ESP32 to your computer
3. Follow the steps presented by the tool
   1. Click Connect and wait for the tool to detect the ESP32\`s chip
   2. Select the desired release of DroneBridge for ESP32
   3. The UI will automatically present you all with your board-compatible "flavours" of the release. If you are not sure, just choose the standard one. You can read more about the `USBSerial` flavour in this Wiki. It is a special version that an ESP32, configured as a ground station, can use.
   4. Click "Flash" to install the firmware to your device
   5. Unplug and re-plug the ESP32 to properly restart the device

## ESP-Tool (console)

1. [Install esptool.py via pip](https://docs.espressif.com/projects/esptool/en/latest/esp32)\
   In Windows use `python -m esptool ...` instead of `esptool.py ...` with the following commands
2. Erase the flash first with `esptool.py -p COM4 erase_flash` (not necessary with all releases, but prevents issues)
3. Change to the extracted esp32c3 folder (the pre-compiled release files of DroneBridge for ESP32 you downloaded from GitHub)
4. Run the flashing command that came with the release binaries for the esp32c3. \
   **It may** look something like:\
   `esptool.py -p (PORT) -b 115200 --before default_reset --after hard_reset --chip esp32c3 write_flash --flash_mode dio --flash_size 2MB --flash_freq 80m 0x0 bootloader.bin 0x8000 partition-table.bin 0x10000 db_esp32.bin 0x110000 www.bin`

[Look here for more detailed information](https://github.com/espressif/esptool)

## Generic Boards

Run the command specified with the release binary and for your board, using `esptool.py` erase the flash first by running `esptool.py -p <port> erase_flash`. Alternatively, you can follow the guide for the web-based serial flasher.
