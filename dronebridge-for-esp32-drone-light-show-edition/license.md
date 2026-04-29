---
description: Info about available licenses and the way they work
---

# License

{% hint style="info" %}
This page applies only to the **Drone Light Show Edition (DLSE)** of DroneBridge for ESP32. The general open-source DroneBridge for ESP32 release does not use this commercial license flow.
{% endhint %}

## How to Obtain a License

To use DLSE in a real drone show deployment, you must register an account and activate each ESP32 individually.

{% stepper %}
{% step %}
#### Register at drone-bridge.com

Create your DLSE user account at [https://drone-bridge.com/dlse/](https://drone-bridge.com/dlse/).
{% endstep %}

{% step %}
#### Buy license credits

After registration, open the DLSE user dashboard and purchase the number of license credits you need. DLSE licenses are sold on a prepaid basis.
{% endstep %}

{% step %}
#### Activate each ESP32

Use the dashboard or a supported activation workflow to activate your DLSE firmware for a specific ESP32. One license credit activates one ESP32.
{% endstep %}

{% step %}
#### License becomes bound to that hardware

After activation, the license is locked to that specific ESP32 hardware. The activated device can then run DLSE without the 10-minute TRIAL limitation.
{% endstep %}
{% endstepper %}

## License Types

The Drone Light Show Edition of DroneBridge for ESP32 (DLSE) knows the following license types:

* TRIAL
* EVALUATION
* ACTIVATED

All installed DLSE licenses work offline. An internet connection is only required when you purchase a license, activate a device, or re-generate a license file for the same hardware.

<table><thead><tr><th width="141">License Type</th><th>Description and Terms</th></tr></thead><tbody><tr><td>TRIAL</td><td>Free default state after flashing. The telemetry stream is stopped after 10 minutes of ESP32 operation. <strong>You will lose connection via the ESP32.</strong><br>For setup, testing, and evaluation only.</td></tr><tr><td>EVALUATION</td><td>Free license for testing on a specific ESP32. It removes the firmware restrictions until its defined end date.<br>After expiry, it behaves like a TRIAL license.<br>For testing and evaluation only.</td></tr><tr><td>ACTIVATED</td><td>Paid license for one ESP32. It has no expiration date after activation.<br>Locked to a specific ESP32 at activation time.<br>Intended for commercial use.</td></tr></tbody></table>

## Important Notes

* License credits are stored in your account until you redeem them by activating an ESP32.
* Unredeemed license credits do not expire as long as your account remains active and in good standing.
* One activation consumes one license credit and binds that activation to one specific ESP32.
* The DLSE firmware on the ESP32 does not need an internet connection for normal operation after activation.
* A full erase of the ESP32 flash removes the locally stored license. You can re-generate a replacement license file for the same hardware without buying another credit.

{% hint style="warning" %}
Update or install licenses only while the drone is disarmed. This matches the DLSE safety guidance and helps avoid unexpected behavior.
{% endhint %}

## Terms & Conditions

For the legally binding commercial terms, always refer to the official [Terms & Conditions for DLSE](https://drone-bridge.com/termsconditions/).
