---
description: How to choose the right ESP32 hardware for your show fleet and how to wire it to your flight controller and power management board.
---

# Hardware & Wiring

## Choosing Your ESP32 Chip

The DLSE supports three ESP32 chip families. All three will work for drone light shows — the choice mainly comes down to your Wi-Fi frequency requirements and the size of your fleet.

{% hint style="info" %}
Regardless of which chip you choose, the ESP32 module must have **at least 4 MB of flash memory**. Almost all modules sold today meet this requirement, but double-check the product listing before ordering.
{% endhint %}

<table data-full-width="false"><thead><tr><th>Chip</th><th>Wi-Fi Standard</th><th>Frequency Bands</th><th>Best For</th></tr></thead><tbody>
<tr><td><strong>ESP32-C5</strong></td><td>Wi-Fi 6 (802.11ax)</td><td>2.4 GHz &amp; 5 GHz</td><td>New designs — dual-band gives full frequency flexibility for current and future deployments</td></tr>
<tr><td><strong>ESP32-C6</strong></td><td>Wi-Fi 6 (802.11ax)</td><td>2.4 GHz only</td><td>Existing designs or where C5 modules are unavailable</td></tr>
<tr><td><strong>ESP32-C3</strong></td><td>Wi-Fi 5 (802.11n)</td><td>2.4 GHz only</td><td>Lowest cost, but 2.4 GHz only and older Wi-Fi standard. Not recommended for large fleets of 200+ drones</td></tr>
</tbody></table>

{% hint style="success" %}
**Not sure which to pick?** Use the **ESP32-C5** for any new design. It is the only supported chip with dual-band Wi-Fi (2.4 GHz and 5 GHz), which gives you the option to move to the less congested 5 GHz band as your fleet grows or when operating at busy venues.
{% endhint %}

## Choosing a Physical Module

ESP32 chips come on many different ready-made modules. For drone use, you want something **small, lightweight, and with a reliable power supply input**. Below are recommended options, beware that only the XIAO ESP32 models are currently tested during development. Contact the manufacturer of the board before you order to check the compatibility of DLSE features with their hardware.

### Module Options

{% tab title="Propel Labs ESP32C5" %}
{% endtab %}

{% tab title="Propel Labs ESP32C5" %}
* Small footprint
* ESP32-C5 chip: Wi-Fi 6, 2.4 GHz & 5GHz support
* Onboard USB-C for flashing (connected to the USB-JTAG interface - to be confirmed)
* U.FL connector for an external antenna (included) — for better signal quality & range
* 8 MB flash (future proof)

Contact User Propel Labs: [1470527829903933654](https://discord.com/users/1470527829903933654) via Discord for more details.    
[Manufacturers Website](https://www.propellabs.co.uk/)
{% endtab %}
{% endtabs %}

**Do you have hardware you want to offer for DLSE? Contact DroneBridge to get added to the list of recommended boards!**

### Other Compatible Modules

Any ESP32 development board featuring the C3, C5, or C6 chip will work, provided it has at least 4 MB of flash. Common alternatives include:

* Any custom or third-party module based on these chips
* XIAO ESP32 C6
* XIAO ESP32 C5

{% hint style="info" %}
The ESP32-C5 is a newer chip and dedicated modules in small drone-friendly form factors are still becoming available. Check Espressif's official devkits (ESP32-C5-DevKitC-1) and module catalogues from distributors such as Mouser or Digi-Key for current options.
{% endhint %}

{% hint style="warning" %}
**Antenna matters.** For reliable telemetry across a show site, always use a module with a U.FL or SMA connector for an **external antenna**. Onboard PCB antennas have significantly shorter range and are sensitive to placement near metal and carbon fiber structures on the drone.
{% endhint %}

---

## Wiring

This is a generic wiring guide. If you have a board from a third-party, follow their wiring instructions instead.

### Overview

Two electrical connections are needed between the ESP32 and the rest of the drone:

1. **UART connection to the flight controller** — carries MAVLink telemetry (mandatory)
2. **Power management connection** — allows the ESP32 to power-cycle the flight controller (recommended, but optional for remote sleep/wakeup)
3. **LED Control** - allows the ESP32 to control the LED of the Light Show Drone based on Skybrush commands. Use the ESP32 instead of the FC. (optional, check on what LED types are supported)

The ESP32 itself is powered from the drone's **5V supply rail** (e.g., from the flight controller's 5V output or a BEC).

### Voltage Levels

{% hint style="warning" %}
**The ESP32 UART operates at 3.3V logic levels.**\
Skybrush-compatible flight controllers (e.g., those running Lightbrush firmware) use 3.3V UART logic by default. Do **not** connect the ESP32 UART directly to a 5V UART without a level shifter — this will damage the ESP32.

You can still **power** the ESP32 from the 5V supply line.
{% endhint %}

---

### UART Wiring (Flight Controller ↔ ESP32)

The UART connection carries all MAVLink traffic between the flight controller and the DLSE. Four wires are needed for the mandatory connection, with two optional wires for hardware flow control.

| Flight Controller Pin | ESP32 Pin | Notes |
|---|---|---|
| TELEM TX | ESP32 **RX** GPIO | Cross-connection: FC TX → ESP RX |
| TELEM RX | ESP32 **TX** GPIO | Cross-connection: FC RX → ESP TX |
| TELEM GND | ESP32 GND | Common ground — always required |
| TELEM 5V | ESP32 5V/VIN | Powers the ESP32 |
| TELEM CTS _(optional)_ | ESP32 **RTS** GPIO | Flow control — see note below |
| TELEM RTS _(optional)_ | ESP32 **CTS** GPIO | Flow control — see note below |

{% hint style="info" %}
**Flow control (RTS/CTS)** is optional. If your flight controller supports it and you are running high baud rates, enabling flow control reduces the chance of dropped bytes. If you do not wire RTS/CTS, set both `gpio_rts` and `gpio_cts` to `0` in the configuration to disable flow control. In general, it is recommended to have flow control. It helps speed up firmware updates of the flight controller.
{% endhint %}

#### Which GPIO Pins to Use

Refer to the tables below for the **recommended GPIO numbers** for each chip. These are GPIO numbers — not pin numbers printed on the physical board. Check the pinout diagram of your specific module to map GPIO numbers to physical pads.

{% tabs %}
{% tab title="ESP32-C3" %}
| Function | Recommended GPIO |
|---|---|
| TX (to FC RX) | GPIO 5 |
| RX (from FC TX) | GPIO 4 |
| RTS (optional) | GPIO 6 |
| CTS (optional) | GPIO 7 |

**Pins that cannot be used with DLSE on ESP32-C3:**

{% hint style="danger" %}
GPIO 2, GPIO 8, GPIO 9, GPIO 12, GPIO 13, GPIO 14, GPIO 15, GPIO 16, GPIO 17
{% endhint %}
{% endtab %}

{% tab title="ESP32-C6" %}
| Function | Recommended GPIO |
|---|---|
| TX (to FC RX) | GPIO 21 |
| RX (from FC TX) | GPIO 2 |
| RTS (optional) | GPIO 22 |
| CTS (optional) | GPIO 23 |

**Pins that cannot be used with DLSE on ESP32-C6:**

{% hint style="danger" %}
GPIO 4, GPIO 5, GPIO 8, GPIO 9, GPIO 15, GPIO 24, GPIO 25, GPIO 26, GPIO 27, GPIO 28, GPIO 29, GPIO 30
{% endhint %}
{% endtab %}

{% tab title="ESP32-C5" %}
| Function | Recommended GPIO |
|---|---|
| TX (to FC RX) | GPIO 23 |
| RX (from FC TX) | GPIO 24 |
| RTS (optional) | GPIO 4 _(untested)_ |
| CTS (optional) | GPIO 5 _(untested)_ |

**Pins that cannot be used with DLSE on ESP32-C5:**

{% hint style="danger" %}
GPIO 2, GPIO 7, GPIO 16, GPIO 17, GPIO 18, GPIO 19, GPIO 20, GPIO 21, GPIO 22, GPIO 25, GPIO 27, GPIO 28
{% endhint %}
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Board pin labels ≠ GPIO numbers.**\
Module manufacturers often label physical pins with their own numbering (e.g., D0, D1, A0…). Always look up the GPIO number in the manufacturer's pinout diagram. The configuration web interface requires GPIO numbers, not physical pin labels.
{% endhint %}

---

### Power Management Wiring

The DLSE power management feature lets Skybrush Live **remotely power the flight controller on or off** — for example, to put drones to sleep between shows or for automated pre-show checks. The ESP32 drives a GPIO pin that triggers an external power control circuit on your power distribution board.

{% hint style="info" %}
Power management requires an external circuit on your power board that can switch the flight controller's power supply based on a signal from the ESP32. The ESP32 itself does **not** directly switch the power — it only sends a control signal.
{% endhint %}

#### Recommended Logic: Active-Low

The recommended approach is **active-low logic** (GPIO low = power on; GPIO high = power off). This is safer because a failing ESP32 will likely pull all its pins low, which means the drone stays powered — this reduces the risk that it will accidentally cut power in flight.

| ESP32 Pin | Power Board Pin | Description |
|---|---|---|
| `show_pm_gpio` (configurable) | Power control input | ESP32 sends on/off signal |
| GND | GND | Common ground |

#### Pulse vs. Static Signal

You can configure the ESP32 to send either a **static** high/low signal or a **timed pulse** (`show_pm_pulse_l` parameter) to trigger the power switch. A timed pulse is more robust — a faulty ESP32 is unlikely to generate a pulse of exactly the right duration, so a spurious power-off command is far less likely.

#### Power State Feedback (Optional)

If your power board can report back whether the flight controller is powered, connect that signal to the `show_pm_qu_gpio` pin. The ESP32 will use this feedback to confirm the power state rather than inferring it from MAVLink traffic. If not provided, the ESP32 will derive the flight controller's power state by looking at the MAVLink telemetry stream. Successful MAVLink packets received from the flight controller indicate a powered flight controller.    
Based on the flight controllers' derived powered state, the ESP32 will adjust its power control signal. It will only send a power-on or off command if it makes sense from a powered state perspective. This is one of many safety features implemented with DLSE.

| Power Board Pin | ESP32 GPIO | Description |
|---|---|---|
| FC power-state output (high = FC powered) | `show_pm_qu_gpio` | ESP32 reads FC power state |

If you do not have this feedback signal, set `show_pm_qu_gpio` to `0`. The ESP32 will then use the presence of MAVLink heartbeats to infer the power state.

#### Safety Requirement

The DLSE version is very much focused on safety. Follow the recommendations when designing or choosing a power management circuit.

{% hint style="danger" %}
**The power management circuit must never cut power to the drone while it is airborne.**\
Implement a hardware interlock on your power board — for example, inhibit the power-off command if the motor current draw exceeds idle levels. Do not rely solely on the ESP32's software armed-state checks. See the [Safety & Integration Guideline](safety-and-integration-guideline.md) for full details.
{% endhint %}

---

### Antenna Placement

For best performance:

* Mount the ESP32 and its antenna **away from carbon fibre** structures, as carbon fibre is conductive and absorbs RF energy.
* The antenna should have a **clear line of sight** toward the ground station direction.
* If using a U.FL-connected external antenna, secure the U.FL connector with a small drop of cyanoacrylate glue (applied after the antenna is connected) to prevent it from loosening due to vibration.
* Avoid routing the antenna cable near ESCs or power wires.

---

## Next Step

Once your hardware is selected and wired, proceed to install the DLSE firmware:

<a href="installation.md" class="button primary">Install DLSE Firmware →</a>
