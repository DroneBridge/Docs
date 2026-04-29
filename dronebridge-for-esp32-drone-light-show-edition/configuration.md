# Configuration

{% hint style="info" %}
This page applies only to the **Drone Light Show Edition (DLSE)** of DroneBridge for ESP32. The sections below mirror the current DLSE web interface and focus on the settings that are actually exposed to operators.
{% endhint %}

## Access and Configuration Methods

DLSE can currently be configured in three ways:

* **Web interface** - the main and recommended way for setup, inspection, import/export, and day-to-day changes
* **REST API / JSON** - useful for automation and fleet tooling, for example through the [Commercial Support Suite](https://github.com/DroneBridge/DLSECommercialSupportSuite)
* **MAVLink parameter protocol** - useful from a GCS, but limited to non-string parameters

The web interface also supports **Export Settings** and **Import Settings**, which is helpful when you want to replicate one working setup across multiple ESP32 modules.

### Web Interface

In normal setup, connect to the ESP32 over Wi-Fi and open the configuration page in a browser at `http://192.168.2.1`.

The exact Wi-Fi network name depends on the active mode and your saved settings:

* In **Wi-Fi Access Point Mode**, the ESP32 uses its configured AP SSID
* In **Wi-Fi Client Mode**, the ESP32 joins your existing show network
* If client-mode connection fails long enough, DLSE can open a **failsafe AP** using the configured hostname as SSID and the AP password

{% hint style="info" %}
MAVLink parameter access does not cover string parameters such as SSIDs, passwords, IP strings, or hostname. Use the web interface or REST API for those.
{% endhint %}

## Wi-Fi

DLSE only exposes two radio modes in the current UI:

* `1` = Wi-Fi Access Point Mode
* `2` = Wi-Fi Client Mode

For real drone-show deployments, **Wi-Fi Client Mode** is usually the production mode because the ESP32 joins the show access point directly. **Wi-Fi Access Point Mode** is mainly useful for bench setup, troubleshooting, and fallback access.

### Wi-Fi Parameters

<table><thead><tr><th width="170">Parameter</th><th width="220">Purpose</th><th width="130">Default</th><th>Description / Notes</th></tr></thead><tbody><tr><td><code>esp32_mode</code></td><td>ESP32 mode</td><td><code>1</code></td><td><code>1</code> = Wi-Fi Access Point Mode, <code>2</code> = Wi-Fi Client Mode. In DLSE, mode changes are stored and become active after reboot.</td></tr><tr><td><code>ssid_ap</code></td><td>SSID ESP32 Access Point</td><td><code>DroneBridge for ESP32</code></td><td>Name of the AP created by the ESP32 in AP mode.</td></tr><tr><td><code>wifi_pass_ap</code></td><td>Password ESP32 Access Point</td><td><code>dronebridge</code></td><td>Password for AP mode and also for the failsafe AP. String parameter, so not available through MAVLink parameter protocol.</td></tr><tr><td><code>wifi_chan</code></td><td>AP Wi-Fi channel</td><td><code>6</code></td><td>Used when the ESP32 hosts its own access point. Valid range in firmware: <code>1-165</code>. In practice, the current UI offers channels <code>1-13</code>.</td></tr><tr><td><code>ap_ip</code></td><td>Gateway IP address</td><td><code>192.168.2.1</code></td><td>IPv4 address used by the ESP32 in AP mode.</td></tr><tr><td><code>ssid</code></td><td>Client-mode SSID</td><td><code>DroneBridge for ESP32</code></td><td>SSID of the show network or other external AP the ESP32 shall join in Wi-Fi Client Mode.</td></tr><tr><td><code>wifi_pass</code></td><td>Client-mode password</td><td><code>dronebridge</code></td><td>Password of the external AP in Wi-Fi Client Mode.</td></tr><tr><td><code>wifi_en_gn</code></td><td>802.11b/g/n/ac/ax support</td><td><code>0</code></td><td>When disabled, DLSE stays on 802.11b-only client behavior, which is usually preferable for range-focused telemetry links. Enable only when your infrastructure requires broader PHY support.</td></tr><tr><td><code>ip_sta</code></td><td>Static IP address</td><td>empty</td><td>Optional static IP for Wi-Fi Client Mode. Leave empty for DHCP.</td></tr><tr><td><code>ip_sta_gw</code></td><td>Static IP gateway</td><td>empty</td><td>Required together with <code>ip_sta</code> when using a static IP.</td></tr><tr><td><code>ip_sta_netmsk</code></td><td>Static IP netmask</td><td>empty</td><td>Required together with <code>ip_sta</code> when using a static IP.</td></tr><tr><td><code>wifi_hostname</code></td><td>Hostname</td><td>firmware-build dependent</td><td>Default comes from the firmware build configuration. In the current DLSE defaults it is typically <code>DroneBridgeDLSE</code>. This hostname is also used as the SSID for the failsafe AP.</td></tr></tbody></table>

{% hint style="info" %}
If you enable SYS ID based on IP later on this page, it is highly recommended to also use a static IP so the MAVLink SYS ID does not change unexpectedly between boots.
{% endhint %}

## Antenna Configuration

The current DLSE UI supports choosing the default antenna path or switching to a secondary external antenna through an RF switch. Treat these settings as hardware-integration parameters: only change them if your PCB and RF front end are designed for it.

<table><thead><tr><th width="170">Parameter</th><th width="220">Purpose</th><th width="130">Default</th><th>Description / Notes</th></tr></thead><tbody><tr><td><code>radio_ant_selec</code></td><td>Antenna selection</td><td><code>0</code></td><td><code>0</code> = default antenna, <code>1</code> = secondary antenna. The firmware also defines <code>2</code> for diversity, but that is not exposed as a normal operator option in the current DLSE UI.</td></tr><tr><td><code>radio_ant_en</code></td><td>GPIO to enable external RF switch</td><td><code>0</code></td><td>Optional GPIO used to enable the external RF switch circuit. Keep at <code>0</code> if your hardware does not require it.</td></tr><tr><td><code>radio_ant_sw</code></td><td>GPIO used to control external RF switch</td><td><code>0</code></td><td>Optional GPIO that selects the secondary antenna path on supported hardware.</td></tr></tbody></table>

{% hint style="warning" %}
Do not change antenna-switch GPIOs unless your board really includes the matching RF switch circuitry. Wrong settings here can degrade or completely break RF performance.
{% endhint %}

## Drone Light Show Settings

These settings control how DLSE integrates into a show network, how it exposes itself to the GCS, and how it behaves when no explicit UDP client is known yet.

### DLSE Network and GCS Behavior

<table><thead><tr><th width="170">Parameter</th><th width="220">Purpose</th><th width="130">Default</th><th>Description / Notes</th></tr></thead><tbody><tr><td><code>wifi_dis_udpdet</code></td><td>Disable UDP client auto detection</td><td><code>0</code></td><td>When enabled, DLSE stops auto-registering UDP clients that send traffic to the ESP32. Only manually managed targets are then served.</td></tr><tr><td><code>wifi_brcst_nudp</code></td><td>Broadcast if no client is known</td><td><code>1</code></td><td>If no UDP client is known, DLSE broadcasts MAVLink traffic to help the GCS auto-detect the device. This is useful for Skybrush-style workflows.</td></tr><tr><td><code>udp_local_port</code></td><td>Local UDP port</td><td><code>14555</code></td><td>Local UDP port used by the ESP32 for UDP traffic. This differs from some older internal documentation; the current firmware default is <code>14555</code>.</td></tr><tr><td><code>wifi_brcst_port</code></td><td>Remote broadcast UDP port</td><td><code>14550</code></td><td>Remote UDP port on the GCS side used for broadcast discovery traffic when no UDP client is known. The current firmware default is <code>14550</code>.</td></tr><tr><td><code>show_ap_fail_to</code></td><td>Failsafe AP spawn timeout [ms]</td><td><code>90000</code></td><td>How long DLSE waits in Wi-Fi Client Mode before opening its own failsafe AP. Set to <code>0</code> to wait indefinitely and never fall back to a failsafe AP.</td></tr><tr><td><code>rep_rssi_dbm</code></td><td>Report RSSI in dBm</td><td><code>1</code></td><td>When enabled, DLSE reports RSSI in dBm. Disable if your GCS expects a 0-100 style RSSI scale instead.</td></tr><tr><td><code>show_pm_en_hb</code></td><td>ESP32 emits heartbeats</td><td><code>0</code></td><td>When enabled, DLSE emits heartbeats so the GCS can detect the ESP32 more easily as a MAVLink component. The current firmware default is <code>0</code>, even though older spreadsheets listed it as enabled by default.</td></tr><tr><td><code>show_en_syid_ip</code></td><td>MAVLink SYS ID based on local IP</td><td><code>1</code></td><td>When enabled, DLSE derives its MAVLink system ID from the last octet of its local IP. This is useful for fleets that already assign drone IDs through static IP planning.</td></tr><tr><td><code>show_man_sysid</code></td><td>Manual ESP32 MAVLink SYS ID</td><td><code>1</code></td><td>Manual system ID used only when <code>show_en_syid_ip</code> is disabled.</td></tr></tbody></table>

{% hint style="info" %}
The UI lets you add or clear UDP clients at runtime.
{% endhint %}

## Power Management

DLSE can control an external power-management circuit connected to the ESP32. This is one of the most safety-sensitive parts of the configuration, because it directly affects whether the flight controller can be powered on or off remotely.

The exact default values for some power-management parameters depend on the firmware build and target hardware configuration. That is why you may see different defaults in internal spreadsheets or preconfigured builds.

<table><thead><tr><th width="170">Parameter</th><th width="220">Purpose</th><th width="160">Default</th><th>Description / Notes</th></tr></thead><tbody><tr><td><code>show_pm_en</code></td><td>Enable ESP32 based Power Management</td><td>build dependent</td><td>Enables remote power-management behavior. In the current default DLSE build this is enabled, but treat it as hardware-profile dependent.</td></tr><tr><td><code>show_pm_gpio</code></td><td>Power Control GPIO</td><td>build dependent</td><td>GPIO used to drive the external power-control circuit.</td></tr><tr><td><code>show_pm_logic</code></td><td>Power Control Logic</td><td><code>0</code></td><td><code>0</code> = active low, <code>1</code> = active high.</td></tr><tr><td><code>show_pm_pulse_l</code></td><td>Power Control Pulse Length [ms]</td><td>build dependent</td><td>Set to <code>0</code> for a static level. Use a non-zero value if your hardware expects a pulse that toggles power.</td></tr><tr><td><code>show_pm_qu_gpio</code></td><td>Power State Query GPIO</td><td>build dependent</td><td>Optional GPIO used to detect whether the flight controller is powered. If set to <code>0</code>, DLSE can derive state from MAVLink traffic instead.</td></tr><tr><td><code>show_pm_en_emcy</code></td><td>Allow emergency power off</td><td><code>0</code></td><td>Disables the normal "is-armed" safety check before power-off. Only enable this if you fully understand the consequences.</td></tr></tbody></table>

{% hint style="warning" %}
**Emergency power-off can shut down a drone in flight.** If <code>show_pm_en_emcy</code> is enabled, DLSE may cut power regardless of the aircraft state. This is only appropriate when your overall system design explicitly accounts for that behavior.
{% endhint %}

{% hint style="info" %}
If you use telemetry-based power-state detection instead of a dedicated query GPIO, DLSE needs MAVLink parsing. In transparent serial mode, the armed state is otherwise unknown.
{% endhint %}

## Battery Monitor

The current DLSE UI can expose battery voltage and current measured by ESP32 ADC inputs. These are optional hardware-integration features and require a correct analog frontend on your board.

<table><thead><tr><th width="170">Parameter</th><th width="220">Purpose</th><th width="130">Default</th><th>Description / Notes</th></tr></thead><tbody><tr><td><code>adc_v_en</code></td><td>Monitor Battery Voltage</td><td><code>0</code></td><td>Enables voltage monitoring through an ADC-capable ESP32 pin.</td></tr><tr><td><code>adc_v_gpio</code></td><td>Battery Voltage Monitor GPIO</td><td><code>0</code></td><td>ADC-capable GPIO connected to the stepped-down battery voltage.</td></tr><tr><td><code>adc_v_multi</code></td><td>ADC Voltage Multiplier</td><td><code>110</code></td><td>Voltage calibration factor. The internal documentation describes this as a value scaled by 10, so <code>110</code> corresponds to a divider factor of 11. Match it to your hardware and to the flight-controller-side voltage calibration.</td></tr><tr><td><code>adc_a_en</code></td><td>Monitor Battery Current</td><td><code>0</code></td><td>Enables current monitoring through an ADC-capable ESP32 pin.</td></tr><tr><td><code>adc_a_gpio</code></td><td>Battery Current Monitor GPIO</td><td><code>0</code></td><td>ADC-capable GPIO used for current sensing.</td></tr><tr><td><code>adc_a_multi</code></td><td>ADC Current Multiplier [mA/V]</td><td><code>36364</code></td><td>Current calibration factor in mA per volt measured at the ESP32 ADC input. Align it with your current-sense circuit and the corresponding ArduPilot current calibration.</td></tr></tbody></table>

{% hint style="info" %}
Use the same calibration basis as your flight controller. For ArduPilot-based systems, the internal parameter notes explicitly point to <code>BATT_VOLT_MULT</code> and <code>BATT_AMP_PERVLT</code> as the matching reference values.
{% endhint %}

## Serial

These settings control the UART connection between the ESP32 and the flight controller.

<table><thead><tr><th width="170">Parameter</th><th width="220">Purpose</th><th width="160">Default</th><th>Description / Notes</th></tr></thead><tbody><tr><td><code>gpio_tx</code></td><td>UART TX GPIO</td><td>build dependent</td><td>TX pin used by the ESP32 UART.</td></tr><tr><td><code>gpio_rx</code></td><td>UART RX GPIO</td><td>build dependent</td><td>RX pin used by the ESP32 UART.</td></tr><tr><td><code>gpio_rts</code></td><td>UART RTS GPIO</td><td>build dependent</td><td>Set equal to CTS to effectively disable hardware flow control.</td></tr><tr><td><code>gpio_cts</code></td><td>UART CTS GPIO</td><td>build dependent</td><td>Set equal to RTS to effectively disable hardware flow control.</td></tr><tr><td><code>rts_thresh</code></td><td>UART RTS threshold</td><td><code>127</code></td><td>Hardware flow-control threshold. Valid range in firmware: <code>1-127</code>. Usually best left at the default.</td></tr><tr><td><code>proto</code></td><td>UART serial protocol</td><td><code>4</code></td><td><code>4</code> = MAVLink, <code>5</code> = Transparent. MAVLink mode is required for DLSE-specific component behavior such as parameter access and telemetry-aware logic.</td></tr><tr><td><code>baud</code></td><td>UART baud</td><td>build dependent</td><td>Valid firmware range: <code>1200-5000000</code>. Must match the flight controller UART configuration.</td></tr><tr><td><code>trans_pack_size</code></td><td>Maximum packet size</td><td><code>576</code></td><td>Maximum over-the-air packet size used by the serial transport. Valid firmware range: <code>16-1056</code>.</td></tr><tr><td><code>serial_timeout</code></td><td>Serial read timeout [ms]</td><td><code>50</code></td><td>Maximum wait time before sending a partially filled packet. The current firmware default is <code>50</code>, even though the current UI HTML still shows a placeholder value of <code>20</code>.</td></tr></tbody></table>

{% hint style="warning" %}
Transparent mode removes MAVLink awareness inside DLSE. That affects features that depend on MAVLink parsing, including parts of component detection, armed-state handling, and telemetry-derived power-state logic.
{% endhint %}

## Practical Recommendations

* For actual show operation, prefer **Wi-Fi Client Mode** with a planned static IP scheme.
* If you use **SYS ID based on IP**, keep the IP static and aligned with the vehicle ID strategy in Skybrush or your flight-controller configuration.
* Leave **UDP auto-detection** and **broadcast if no client is known** enabled unless your network design requires explicit manual targets only.
* Treat **power-management** and **battery-monitor** settings as hardware-integration parameters, not generic software toggles.
* If something stops working after a network change, a failsafe AP may be your easiest recovery path.
