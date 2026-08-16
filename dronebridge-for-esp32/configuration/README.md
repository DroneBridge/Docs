---
description: Configuration Options for DroneBridge for ESP32
---

# Configuration

## Web Interface

1. Connect to the wifi `DroneBridge for ESP32` with password `dronebridge`
2. In your browser, type: `dronebridge.local` (Chrome: `http://dronebridge.local`) or `192.168.2.1` into the address bar. **You might need to disable the cellular connection to force the browser to use the WiFi connection**
3. Configure as you please and hit `save & reboot`

<figure><img src="../../.gitbook/assets/dbesp32_webinterface (1).png" alt=""><figcaption></figcaption></figure>

## UART Parameters

For the Official DroneBridge for ESP32 Board HW v1.0, HW v1.1 & HW v1.2.\
This configuration is only valid for the official boards! If you did not connect the flow control lines, set RTS and CTS pins to 0 to disable flow control. Check the [Flow Control section](../hardware-and-wiring.md#uart-flow-control) for more details.

<table><thead><tr><th width="247">Board</th><th width="100" data-type="number">TX GPIO</th><th width="100" data-type="number">RX GPIO</th><th width="100" data-type="number">RTS GPIO</th><th width="89" data-type="number">CTS GPIO</th></tr></thead><tbody><tr><td><strong>ESP32C3</strong> Official Hardware</td><td>5</td><td>4</td><td>6</td><td>7</td></tr><tr><td><strong>ESP32C6</strong> Official Hardware</td><td>21</td><td>2</td><td>22</td><td>23</td></tr></tbody></table>

## Resetting the ESP32

If you made a configuration error and want to reset the settings of the ESP32 you can do so using the BOOT Button in the release v2.0 and onwards.

* A short press/click of the boot button will reset the Mode and WiFi settings of the ESP32 to Access Point mode with `dronebridge` as the password. That way you can check the configuration.
* A long press (>1.8s) of the boot button will reset all settings back to defaults. The WiFi Access Point password is `dronebridge`.

## DroneBridge for ESP32 Modes

DroneBridge for ESP32 supports the following modes:

<table data-full-width="true"><thead><tr><th width="124">SYS_ESP32_MODE</th><th>DroneBridge for ESP32 Mode</th><th width="91">Encryption</th><th>Description</th><th>Notes</th></tr></thead><tbody><tr><td>1</td><td>WiFi Access Point Mode</td><td>WPA2 PSK</td><td>ESP32 launches classic WiFi access point using 802.11b rates</td><td>Any WiFi device can connect. Can manage up to 10 WiFi stations/clients.</td></tr><tr><td>2</td><td>WiFi Client Mode</td><td>min. WEP</td><td>ESP32 connects to an existing WiFi access point. LR Mode supported</td><td>Encryption defined by access point. Multiple drones can connect to one AP and GCS. 802.11b rates</td></tr><tr><td>3</td><td>WiFi Access Point Mode LR</td><td>WPA2 PSK</td><td>ESP32 launches WiFi access point mode using espressifs LR mode</td><td>Only ESP32 LR Mode enabled devices can detect and connect to the access point. Data rate is reduced to 0.25Mbit. Range is greatly increased.</td></tr><tr><td>4</td><td>ESP-NOW LR Mode AIR</td><td>AES256-GCM</td><td>ESP32 is able to receive ESP-NOW broadcast packets from any GCS in the area and forwards them to the UART. Broadcasts to all GND stations in the area.</td><td>Connectionless protocol. Data reate is reduced to 0.25Mbit. Range is greatly increased compared to WiFi modes. Custom encryption mode for ESP-NOW broadcasts and protocol.</td></tr><tr><td>5</td><td>ESP-NOW LR Mode GND</td><td>AES256-GCM</td><td>ESP32 is able to receive ESP-NOW broadcast packets from any drone in the area and forwards them to the UART. Broadcasts to all AIR stations in the area.</td><td>Connectionless protocol. Data rate is reduced to 0.25Mbit. Range greatly increased compared to WiFi modes. Custom encrpytion mode for ESP-NOW broadcasts and protocol.</td></tr><tr><td>6</td><td>Bluetooth LE</td><td>TBD</td><td>Bluetooth LE (BLE) connection to your device.<br></td><td><p>The application (GCS) must explicitly support BLE connections.<br>So far this is not the case. They only support Bluetooth Classic SPP which is NOT BLE.</p><p>Use the DroneBridge BLE Bridge application to connect to your GCS.</p></td></tr></tbody></table>

## Configuration Parameters

<table><thead><tr><th width="214">Web Parameter Name</th><th>MAVLink Parameter Name</th><th>Description</th></tr></thead><tbody><tr><td>Mode</td><td>SYS_ESP32_MODE</td><td><a href="./#dronebridge-for-esp32-modes">Check the modes section</a></td></tr><tr><td>SSID</td><td>Cannot be configured via MAVLink</td><td>Specifies the name of the Wi-Fi network in access point and client mode. Up to 31 characters long. WiFi must be at least WEP protected.</td></tr><tr><td>Password</td><td>Cannot be configured via MAVLink</td><td>Wi-Fi access point or ESP-NOW password used for encryption.<br>Min. 8 characters, max 63 characters long. WiFi must be at least WEP encrypted.</td></tr><tr><td>Channel</td><td>WIFI_AP_CHANNEL</td><td>Wi-Fi access point or ESP-NOW channel.</td></tr><tr><td>Gateway IP address</td><td>Cannot be configured via MAVLink</td><td>IP address you want the access point to have</td></tr><tr><td>UART TX PIN</td><td>SERIAL_TX_PIN</td><td>TX GPIO of the ESP32. If the pin matches the RX pin, the UART will not be opened.</td></tr><tr><td>UART RX Pin</td><td>SERIAL_RX_PIN</td><td>RX GPIO of the ESP32. If the pin matches the TX pin, the UART will not be opened.</td></tr><tr><td>UART RTS Pin</td><td>SERIAL_RTS_PIN</td><td>RTS GPIO of the ESP32. If the pin matches the CTS pin, flow control will be disabled.</td></tr><tr><td>UART CTS Pin</td><td>SERIAL_CTS_PIN</td><td>CTS GPIO of the ESP32. If the pin matches the RTS pin, flow control will be disabled.</td></tr><tr><td>UART RTS threshold</td><td>SERIAL_RTS_THRES</td><td>Threshold of hardware RX flow control to prevent FIFO overload. Set any number of bytes between 0-127. Best to leave at 64.</td></tr><tr><td>UART serial protocol</td><td>SERIAL_TEL_PROTO<br><br>MSP/LTM = 1<br>MAVLink = 4<br>TRANSPARENT = 5</td><td>Configures the parser. Set to transparent for no parsing. When not set to transparent it will detect individual messages of the data stream. In MAVLink mode it can inject RADIO_STATUS and heartbeat packets. This allows the ESP32 to register with the GCS. Support for MAVLink parameter protocol.</td></tr><tr><td>UART baud</td><td>SERIAL_BAUD</td><td>UART baud rate. Must be the same as with the autopilot. Try baud rates at the lower end if you see data but your GCS is not showing any of it.</td></tr><tr><td>Maximum packet size</td><td>SERIAL_PACK_SIZE</td><td>Maximum packet size in transparent mode. When not in pransparent mode the parser will try to fill it with the maximum amount of complete messages without plitting the messages. Max. for ESP-NOW mode is &#x3C;250bytes (internally capped when set higher)</td></tr><tr><td>Serial read timeout [ms]</td><td>SERIAL_T_OUT_MS</td><td>Maximum amount of time to wait for a new serial bite. When reached even an uncomplete message will be sent via radio.</td></tr></tbody></table>
