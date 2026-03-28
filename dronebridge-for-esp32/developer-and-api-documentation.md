---
description: >-
  Documentation regarding DroneBridge for ESP32 aimed at developers and everyone
  how wants to use the REST:API.
---

# Developer & API Documentation

## Compile

You will need the Espressif SDK: esp-idf + toolchain. Check out their website for more info and on how to set it up. The code is written in pure C using the esp-idf (no Arduino libs).\
Compile using esp-idf v5.1 or esp-idf v5.2 or esp-idf v5.3

* ESP32 `idf.py set-target esp32 build`
* ESP32S2 `idf.py set-target esp32s2 build`
* ESP32S3 `idf.py set-target esp32s3 build`
* ESP32C3 `idf.py set-target esp32c3 build`
* ESP32C3 `idf.py set-target esp32c6 build`

Or compile all at once with release configuration running the release scripts for bash or PowerShell `create_release_zip.sh` or `create_release_zip.ps1`

**This project supports the v5.3 of ESP-IDF**\
Compile and flash by running: `idf.py build`, `idf.py flash`

The web interface is built using the command `idf.py frontend`. This is done automatically when compiling the entire project using `idf.py build`. The frontend is built to `build/frontend`.\
Alternatively, the frontend can be built using `npm install && npm i -D shx && npm run build` within `/frontend/`, then manually copy the content of `/frontend/build` to `/build/frontend`

## API

The web interface communicates with a REST: API on the ESP32. You can use that API to set configurations that are not selectable via the web interface (e.g., baud rate). It also allows you to integrate DroneBridge for ESP32 easily.

### Get Settings

<mark style="color:green;">`GET`</mark> `/api/settings`

Get all available settings that are currently stored on the ESP32.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `none`             |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
  "esp32_mode": 2,
  "wifi_ssid": "DroneBridge",
  "wifi_pass": "dronebridge",
  "ap_channel": 6,
  "ap_ip": "192.168.2.1",
  "dis_wifi_arm": 0,
  "trans_pack_size": 128,
  "tx_pin": 4,
  "rx_pin": 5,
  "cts_pin": 0,
  "rts_pin": 0,
  "rts_thresh":	64,
  "baud": 115200,
  "telem_proto": 4,
  "ltm_pp": 2,
  "static_client_ip": "",
  "static_netmask": "",
  "static_gw_ip": ""
}
```
{% endtab %}
{% endtabs %}

### Get Statistics

<mark style="color:green;">`GET`</mark> `/api/system/stats`

Receive real-time statistics about connected clients and received UART bytes.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `none`             |

**Response**

{% tabs %}
{% tab title="200" %}
<pre class="language-json"><code class="lang-json">{
<strong>    "read_bytes": 4672,
</strong>    "tcp_connected": 0,
    "udp_connected": 2,
    "udp_clients": ["192.168.10.55:14550", "192.168.10.6:14550"],
    "current_client_ip": "192.168.10.17",
    "esp_rssi":	-45
}
</code></pre>
{% endtab %}
{% endtabs %}

### Get System Info

<mark style="color:green;">`GET`</mark> `/api/system/info`

General information about the installed DroneBridge for ESP32 firmware

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `none`             |

**Response**

{% tabs %}
{% tab title="200" %}
```json
{
 "idf_version":	"v5.3",
 "db_build_version": 12,
 "major_version": 2,
 "minor_version": 0,
 "esp_chip_model": 5,
 "esp_mac": "EC:DA:3B:BC:XX:XX",
 "serial_via_JTAG": 1
}
```
{% endtab %}
{% endtabs %}

The esp\_chip\_model is an index [according to the enumeration of espressif](https://github.com/espressif/esp-idf/blob/67c1de1eebe095d554d281952fde63c16ee2dca0/components/esp_hw_support/include/esp_chip_info.h#L22):

```c
CHIP_ESP32  = 1, //!< ESP32
CHIP_ESP32S2 = 2, //!< ESP32-S2
CHIP_ESP32S3 = 9, //!< ESP32-S3
CHIP_ESP32C3 = 5, //!< ESP32-C3
CHIP_ESP32C2 = 12, //!< ESP32-C2
CHIP_ESP32C6 = 13, //!< ESP32-C6
CHIP_ESP32H2 = 16, //!< ESP32-H2
CHIP_ESP32P4 = 18, //!< ESP32-P4
CHIP_ESP32C61= 20, //!< ESP32-C61
CHIP_ESP32C5 = 23, //!< ESP32-C5
CHIP_POSIX_LINUX = 999, //!< The code is running on POSIX/Linux simulator
```

### Get connected clients

<mark style="color:green;">`GET`</mark> `/api/system/clients`

List of all connected UDP and TCP clients with their ports.

**Headers**

| Name          | Value              |
| ------------- | ------------------ |
| Content-Type  | `application/json` |
| Authorization | `none`             |

**Response**

```json
{
    "udp_clients": ["192.168.10.55:14550", "192.168.10.6:14550"]
}
```

### Manually add UDP target

Add a UDP target with IP and port that the ESP32 shall send serial data to. Set "save" to true in case you want the UDP client be saved to NVM. That way it will be automatically added on startup. Otherwise (false) it will only be added for this session and deleted on poweroff/reset of the ESP32.\
Only one UDP client can be added to NVM. The currently stored client will be overwritten if called multiple times with "save" = true.

To clear saved and manually added UDP clients call API to remove/clear UDP clients.

<mark style="color:green;">`POST`</mark> `/api/settings/clients/udp`

```json
{
  "ip": "XXX.XXX.XXX.XXX",
  "port": 452,
  "save": true
}
```

### Remove all UDP targets

Removes all UDP targets/clients for this session and from the NVM.

<mark style="color:green;">`DELETE`</mark> `/api/settings/clients/clear_udp`

No need to send any content.

### Assign static IP when in WiFi Client Mode

<mark style="color:green;">`POST`</mark> `/api/settings/static-ip`

**Request**

```json
 {
  "client_ip": "XXX.XXX.XXX.XXX",
  "netmask": "XXX.XXX.XXX.XXX",
  "gw_ip": "XXX.XXX.XXX.XXX"
 }
```

## Testing Locally

Check `/test` for scripts triggering the API.

To test the frontend without the ESP32 run

```sh
npm install -g json-server@v0.17.4
json-server db.json --routes routes.json
```

Set  `const ROOT_URL = "http://localhost:3000/"` inside `index.html` and the `<script>` block
