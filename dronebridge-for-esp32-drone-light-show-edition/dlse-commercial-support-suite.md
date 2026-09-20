---
description: >-
  Open-source Python tooling for bulk flashing, configuration, licensing, and
  OTA updates across your entire drone fleet.
---

# Support Suite

<figure><img src="../.gitbook/assets/DLSE_UI1.png" alt="DLSE Fleet Manager showing DLSE ESP32 devices in a table that can be configured"><figcaption></figcaption></figure>

The **DLSE Commercial Support Suite** is an open-source Python library and set of ready-to-use scripts that automate the most time-consuming parts of managing a large fleet of ESP32s. Instead of configuring, flashing, and licensing each drone one by one, you set up one reference device, export its configuration, and let the suite handle the rest.

<a href="https://github.com/DroneBridge/DLSECommercialSupportSuite" class="button primary" data-icon="github">View on GitHub</a>

***

## What It Can Do

<table data-full-width="false"><thead><tr><th>Capability</th><th>How</th><th>Use When</th></tr></thead><tbody><tr><td><strong>Batch serial flash + configure + activate</strong></td><td><code>batch_install_dlse_allinone.py</code></td><td>Initial setup of a new fleet — plug in ESP32s one by one</td></tr><tr><td><strong>Batch OTA firmware update</strong></td><td><code>batch_ota_update_allinone.py</code></td><td>Upgrading all drones in the field over Wi-Fi</td></tr><tr><td><strong>Batch OTA license activation</strong></td><td><code>batch_ota_license_activation.py</code></td><td>Licensing a fleet of ESP32s over Wi-Fi in one pass</td></tr><tr><td>Scan network for DLSE devices</td><td>Library function</td><td>Discovery, health checks</td></tr><tr><td>Upload / download licenses</td><td>Library function</td><td>Re-licensing, backup</td></tr><tr><td>Get activation key from device</td><td>Library function</td><td>Programmatic licensing workflows</td></tr><tr><td>Remote reset of ESP32</td><td>Library function</td><td>Automated test pipelines</td></tr><tr><td>Change settings over Wi-Fi</td><td>Library function</td><td>Dynamic reconfiguration without USB</td></tr><tr><td>Download flight logs via the ESP32 bridge</td><td>Library function</td><td>Post-show data collection</td></tr></tbody></table>

***

## Prerequisites

* Python 3.10 or higher
* A DroneBridge account and **secret token** (obtained from [drone-bridge.com/dlse](https://drone-bridge.com/dlse) dashboard) for licensing features
* The [latest DLSE release binaries](https://drone-bridge.com/dlse/) downloaded and extracted locally

***

## Installation

Open a command line window like Powershell on Windows and run:

```bash
python -m pip install pipx
python -m pipx ensurepath
```

Open a new terminal and install using:

```shellscript
pipx install https://github.com/DroneBridge/DLSECommercialSupportSuite/releases/download/v1.1.0/dlsecommercialsupportsuite-1.1.0-py3-none-any.whl
```

For upgrades of an existing installation, you might need to run:

```bash
pipx install --force https://github.com/DroneBridge/DLSECommercialSupportSuite/releases/download/v1.1.0/dlsecommercialsupportsuite-1.1.0-py3-none-any.whl
```

For newer releases, replace `v1.1.0` and the wheel filename with the version shown on the GitHub Releases page.

Open a new terminal and verify the commands are available:

```
dlse-activate --help
dlse-reboot --help
dlse-update --help
dlse-install --help
dlse-ui
```

***

## DLSE Fleet Manager

The DroneBridge DLSE Fleet Manager allows you to manage the configuration of all your DLSE ESP32s. It offers a convenient user interface designed for fleet-level adjustments.

<figure><img src="../.gitbook/assets/DLSE_UI2.png" alt="DLSE Fleet Manager table view with detected drones"><figcaption></figcaption></figure>

### Quick Start

Click `Scan for Devices` to start detecting your DLSE ESP32s on the network. Once all ESP32s are listed, click the button again to stop the network scan.

You can connect to your UniFi Gateway + Access Points by providing the UniFi Gateway IP and access token in the settings.

The buttons in the header (Reboot Devices, Manage Static IPs, Apply Settings, etc.) apply to all selected (via the checkbox in the table) devices.

`Clear Fleet` **clears the table only**. All discovered ESP32s will be removed from the table and will have to be re-discovered by a scan. This does not change any setting or configuration.

#### Fleet Manager Settings

<figure><img src="../.gitbook/assets/grafik (7).png" alt="DLSE Commercial Support Suite Fleet Manager Settings for ESP32 discovery scans on the network"><figcaption></figcaption></figure>

### Fleet Manager settings

These settings control how Fleet Manager discovers and monitors DLSE ESP32 devices on the local network. The default values work with the standard DLSE configuration.

| **Setting**            | Description                                                                                                                                                                                                                                                                                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Discovery methods      | <p><strong>MAVLink broadcast</strong> is recommended and provides fast discovery. The DLSE UART/MAVLink interface must be configured. <br><strong>HTTP IP-range scan</strong> checks each address through <code>/api/system/info</code>; it is slower but can be used as a robust fallback.</p>                                                                                                  |
| IPv4 subnet            | <p>Network range to scan, written in CIDR notation. Example: <code>192.168.1.0/24</code>. The subnet must include the computer and all DLSE devices. <br>This covers <code>192.168.1.0</code>–<code>192.168.1.255</code>; Fleet Manager scans usable host addresses <code>192.168.1.1</code>–<code>192.168.1.254</code> and uses <code>192.168.1.255</code> for MAVLink broadcast discovery.</p> |
| ESP32 broadcast port   | UDP port on which the DLSE listens for discovery messages. Match this with `udp_local_port` in the DLSE web interface. Default: `14555`.                                                                                                                                                                                                                                                         |
| Local receive port     | UDP port used by Fleet Manager to receive MAVLink responses. Match this with `wifi_brcst_port` in the DLSE web interface. Default: `14550`. The port must not be used by another application.                                                                                                                                                                                                    |
| Discovery interval (s) | Time between automatic discovery scans. Default: `5` seconds.                                                                                                                                                                                                                                                                                                                                    |
| HTTP timeout (s)       | Maximum wait time for each HTTP scan request. Default: `1` second.                                                                                                                                                                                                                                                                                                                               |
| HTTP concurrency       | Number of IP addresses checked simultaneously during an HTTP scan. Default: `20`.                                                                                                                                                                                                                                                                                                                |

#### UniFi AP observations

Enable this section to display wireless client and access-point information from a UniFi Network gateway.

* **Gateway URL**: UniFi gateway address, for example `https://192.168.1.1`.
* **API token**: UniFi Network API token. It is stored in the local Fleet Manager settings.
* **Site**: UniFi site name. Default: `default`.
* **Verify certificate**: Enable this for a valid TLS certificate. Disable it only when using a trusted gateway with a self-signed certificate.

If MAVLink discovery does not find devices, verify the subnet, confirm that both UDP ports match the DLSE web-interface settings, and ensure that no other application (Skybrush Server) is occupying the local receive port.

#### System stats polling

Fleet Manager can periodically query `/api/system/stats` for devices that have already been discovered.

* **Background polling**: Enables or disables live DLSE statistics polling.
* **Target interval (s)**: Time between polling rounds. Default: `2` seconds. Increase on large fleets.
* **Request timeout (s)**: Maximum wait time per device. Default: `1` second. Increase on large fleets.
* **HTTP concurrency**: Number of devices queried simultaneously. Default: `20`.
* **Failures before offline**: Number of consecutive failed requests before a device is shown as offline. Default: `3`.

### Change individual settings on one device

Click on a table row (no selection via the checkbox required) to be able to change settings via the web interface of the dedicated settings tab on the right of the user interface.\
You can download or upload all the settings as well.

### Change settings on multiple devices

1. Select the desired devices via the checkbox in the table view
2. Click `Apply Settings` in the header bar
3. Select a `.csv` DLSE settings file. You can export them from a pre-configured DLSE device via the `Settings` tab in the Fleet Manager or via the web interface
4. Follow the instructions in the user interface. Not all settings will be applied. IP, hostname and MAV SYS ID are incremented for every device.

### OTA Firmware Upgrade

You should activate/make visible the `Operation`  and `Progress` columns inside the table view to see the current state of the firmware upgrade.

<figure><img src="../.gitbook/assets/grafik (8).png" alt="DLSE Commercial Support Suite Over-The-Air firmware upgrade dialog where the user has to enter his secret token to get his releases. Settings to upgrade only specific devices."><figcaption></figcaption></figure>

Use this dialogue to update the DLSE firmware of selected or visible ESP32 devices over HTTP. Device settings and licenses are preserved.

For each device, Fleet Manager:

1. Checks the device and identifies its chip type.
2. Uploads `www.bin` to update the web interface.
3. Waits two seconds.
4. Uploads `db_esp32.bin` and reboots the device.

#### Options

* **DroneBridge account release**: Enter a license-server token, load available releases, and select a release. Cached releases can also be used.
* **Validated release folder**: Select a local DLSE release folder. Fleet Manager automatically uses the binaries matching each device’s chip.
* **Explicit WWW and application binaries**: Select both `www.bin` and `db_esp32.bin` manually.
* **Target firmware version**: Optional exact version filter. Only devices running this version are updated. Leave empty to update all targets.
* **Parallel updates**: Number of devices updated simultaneously. The default is `20`.
* **Targets**: Choose either the selected devices or all devices currently visible in the Fleet Manager table.

Cancelling stops queued updates; uploads already in progress are allowed to finish.

### Align SYS IDs

Align SYS IDs synchronises the MAVLink system ID of the DLSE and its connected flight controller.

Only explicitly selected devices with **Evaluation** or **Activated** licenses are processed.

#### Options

* **Based on DLSE IP address**\
  Sets the flight-controller SYS ID to the last octet of the DLSE IP address. For example, `192.168.1.42` results in SYS ID `42`. It also enables the DLSE web-interface option `show_en_syid_ip`.
* **Based on FC SYS ID**\
  Reads the current flight-controller SYS ID and writes it as the DLSE manual SYS ID (`show_man_sysid`). The flight-controller SYS ID is not changed.
* **Based on manual DLSE SYS ID**\
  Uses the current DLSE manual SYS ID and writes the same value to the flight controller. IP-based SYS ID assignment is disabled.

The modes that change the flight-controller SYS ID write a MAVLink parameter and reboot the flight controller. The DLSE settings are then updated.

### Manage Static IPs

This dialogue assigns sequential static IP addresses to visible, selected devices with **Evaluation** or **Activated** licenses. Selected devices hidden by the current table filter are excluded.

#### Assign Static IPs

* **Starting static IP**: First address to assign. Further addresses are allocated in the current table order. For example, starting at `192.168.20.1` assigns `.1`, `.2`, `.3`, and so on.
* **Subnet mask**: Network mask used by the devices, for example `255.255.255.0`.
* **Gateway IP**: Network gateway. It must be a usable address within the selected subnet and must not conflict with an assigned device address.

The values correspond to the DLSE web-interface settings `ip_sta`, `ip_sta_netmsk`, and `ip_sta_gw`. Each device reboots and stops responding at its old IP address after the change.

#### Clear All Static IPs

Clears the static IP, subnet mask, and gateway settings. Each device reboots and uses DHCP after reconnecting.

***

## CLI: Batch Serial Flash, Configure & Activate

This function is not available through the graphical user interface yet.&#x20;

### Overview

`batch_install_dlse_allinone.py` is the main script for setting up a fresh fleet. You run it once, then plug in your ESP32 modules one by one via USB. For each device it automatically:

1. Flashes the DLSE firmware over the serial connection
2. Writes your exported configuration (SSID, IPs, GPIO pins, MAVLink settings, etc.)
3. Assigns a unique hostname, AP SSID, and static IP based on a sequential index (e.g., drone 55, 56, 57…)
4. Requests a license from the DroneBridge license server and activates the device
5. Logs everything to a `/logs` folder

{% hint style="info" %}
**Re-flashing an already-licensed device?** The script detects that the ESP32 already has a valid license, pulls it from the device before flashing, and re-applies it after. You will not lose license credits by re-running the script on the same hardware.
{% endhint %}

### Workflow

{% stepper %}
{% step %}
**Configure one reference ESP32**

Set up a single ESP32 exactly as you want all drones in your fleet to be configured.

1. Flash DLSE using the [online flasher](https://drone-bridge.com/flasher/)
2. Connect to the ESP32's Wi-Fi AP (`DroneBridge for ESP32`, password `dronebridge`)
3. Open the web interface at `http://192.168.2.1` and configure all settings (UART pins, baud rate, Wi-Fi credentials, power management, etc.)
4. Test the configuration with your show drone and flight controller
5. Activate this reference device manually using your token from [drone-bridge.com](https://drone-bridge.com) and the online [license generator](https://drone-bridge.com/licensegenerator/)
6. **Export the settings** using the web interface — this produces a `.csv` file you will pass to the batch script
{% endstep %}

{% step %}
**Prepare the suite**

Download the latest DLSE release binaries from [drone-bridge.com/dlse](https://drone-bridge.com/dlse/) and extract them **into the `DLSECommercialSupportSuite` folder**. The folder name (e.g., `DroneBridge_ESP32DLSE_BETA3`) is used as the `--release-folder` argument.
{% endstep %}

{% step %}
**Run the batch script**

```bash
python batch_install_dlse_allinone.py \
  --token <YOUR_SECRET_TOKEN> \
  --release-folder "DroneBridge_ESP32DLSE_BETA3" \
  --settings-file my_parameters/dlse_my_params.csv \
  --start-index 55
```

**Arguments:**

| Argument           | Description                                                                             |
| ------------------ | --------------------------------------------------------------------------------------- |
| `--token`          | Your secret token from the drone-bridge.com dashboard                                   |
| `--release-folder` | Folder containing the DLSE firmware binaries                                            |
| `--settings-file`  | The `.csv` settings file exported from your reference ESP32                             |
| `--start-index`    | Starting index for per-drone unique values (AP SSID suffix, hostname, static IP suffix) |

The `--start-index` parameter allows automatic per-device differentiation. With `--start-index 55`, the first ESP32 you plug in will get a hostname of `<your_hostname>55`, an AP SSID of `<your_ssid>55`, and a static IP suffix of `55`. The index increments automatically for each subsequent device.
{% endstep %}

{% step %}
**Plug in ESP32s one by one**

With the script running, simply connect your ESP32 modules to the computer via USB one at a time. The script detects each new device, flashes it, configures it, and activates it — then waits for the next one. The whole process per device takes under a minute.

All actions are logged to the `/logs` folder for your records.
{% endstep %}
{% endstepper %}

{% hint style="success" %}
**Offline activation is also supported.** If the license server is unavailable, the script checks the `/received_licenses` folder for a previously downloaded license file matching the device's activation key, and applies it locally.
{% endhint %}

***

## CLI: Batch OTA Firmware Update

### Overview

`batch_ota_update_allinone.py` scans your Wi-Fi subnet, discovers all DLSE devices, and pushes a new firmware version to each of them over Wi-Fi. No USB cable or physical access needed. **Existing settings and licenses are preserved** across an OTA update.

{% hint style="warning" %}
**Skybrush Live must be shut down** before running this script. The script needs to bind to the broadcast port that Skybrush Live also uses for device discovery.
{% endhint %}

### Usage

Update all detected devices:

```bash
python batch_ota_update_allinone.py \
  --release-folder "DroneBridge_ESP32DLSE_BETA4" \
  --subnetmask "192.168.1.0/24"
```

Update only devices running a specific version (useful when your fleet has mixed versions):

```bash
python batch_ota_update_allinone.py \
  --release-folder "DroneBridge_ESP32DLSE_BETA4" \
  --subnetmask "192.168.1.0/24" \
  --target-version "1.0.0-beta.3"
```

**Arguments:**

| Argument           | Description                                                                                           |
| ------------------ | ----------------------------------------------------------------------------------------------------- |
| `--release-folder` | Path to the folder with DLSE firmware binaries                                                        |
| `--subnetmask`     | IP range to scan, e.g. `192.168.1.0/24`                                                               |
| `--target-version` | _(Optional)_ Only update ESP32s running this specific version. Devices on other versions are skipped. |

### What Happens

The script scans the subnet, lists all discovered DLSE devices with their current firmware versions, and asks for confirmation before proceeding. It then pushes the firmware binary matching each device's chip type (C3, C5, or C6) and reports success or failure per device.

<details>

<summary>Example console output</summary>

```
[2026-03-04 23:15:34] Found binaries for ESP32C3, ESP32C5, ESP32C6
[2026-03-04 23:15:37] Found 2 ESP32 (DLSE) device(s).
    [ ] IP 192.168.1.174  SYS_ID: 174  firmware: 1.0.0-beta.4
    [X] IP 192.168.1.206  SYS_ID: 206  firmware: 0.0.0-dev.1

Do you want to proceed with the OTA update for the selected [x] devices? (y/n): y

Skipping 192.168.1.174 — not running target version
Updating 192.168.1.206 ...
Uploading: 100%|██████████| 1.11M/1.11M [00:14<00:00]
✅ OTA update successful — rebooting device

OTA update finished. 1 updated successfully. 0 failed.
```

</details>

***

## CLI: Batch OTA License Activation

### Overview

`batch_ota_license_activation.py` continuously scans the subnet and activates any unlicensed DLSE device it finds — over Wi-Fi, without needing a USB connection. Run it while your drones are powered on and connected to the show network, and it will work through the fleet automatically.

{% hint style="warning" %}
**Skybrush Live must be shut down** before running this script.
{% endhint %}

### Usage

```bash
python batch_ota_license_activation.py \
  --token <YOUR_SECRET_TOKEN> \
  --subnetmask "192.168.1.0/24" \
  --esp32localbrcstport 14635 \
  --esp32remotebrcstport 14630
```

**Arguments:**

| Argument                 | Description                                                |
| ------------------------ | ---------------------------------------------------------- |
| `--token`                | Your secret token from the drone-bridge.com dashboard      |
| `--subnetmask`           | IP range to scan                                           |
| `--esp32localbrcstport`  | Must match `udp_local_port` in each ESP32's configuration  |
| `--esp32remotebrcstport` | Must match `wifi_brcst_port` in each ESP32's configuration |

The script scans in a loop. As drones are powered on and appear on the network, they are discovered, their activation key is read, a license is requested from the DroneBridge license server, and the license is installed on the device. All licenses are also saved locally to `/received_licenses` as a backup.

<details>

<summary>Example console output</summary>

```
[23:34:08] Scanning 192.168.1.0/24 for ESP32 devices...
[23:34:10]   Found 0 devices.
[23:34:28] Scanning 192.168.1.0/24 for ESP32 devices...
[23:34:30]   Found 1 device.
[23:34:31] Requesting license from drone-bridge.com...
[23:34:31] ✅ License generated and saved.
[23:34:31] ✅ License signature valid.
[23:34:32] 🔑 Activated 192.168.1.149
[23:34:42] Session total: 1 device activated.
```

</details>

***

## Library Functions & Individual Examples

Beyond the batch scripts, the suite exposes a Python library (`DroneBridgeCommercialSupportSuite.py`) for building your own automation tools. Individual example scripts are provided for each function:

| Script                                 | What It Demonstrates                                      |
| -------------------------------------- | --------------------------------------------------------- |
| `example_esp32_get_license.py`         | Request a license file using an activation key            |
| `example_params_update_flash.py`       | Update configuration parameters in the CSV and flash them |
| `example_esp32_ota_update.py`          | OTA firmware update for all detected devices              |
| `example_esp32_download_log.py`        | Download flight controller logs via the ESP32 bridge      |
| `example_esp32_download_log_MAVSDK.py` | Same, using MAVSDK                                        |

***

## Installation for Developers

```bash
git clone --recursive https://github.com/DroneBridge/DLSECommercialSupportSuite.git
cd DLSECommercialSupportSuite
pip install .
```

Each script contains configuration variables near the top (such as `MY_SECRET_TOKEN`, `ESP_SERIAL_PORT`, or subnet addresses). Open the script you intend to use and update these before running.

## OpenAPI Definition

The suite also ships with an **OpenAPI definition** (`api_definition/openapi_definition.yaml`) describing the full DLSE REST API. Use it to generate client code in any language, explore the API in tools like Swagger UI, or build your own tooling on top of the DLSE configuration endpoint.

See the [Developers — DLSE API Definition](developers-dlse-api-definition/) section for more.
