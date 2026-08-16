# QGroundControl Setup

## QGroundControl Configuration

When the serial protocol of the ESP32 is configured to MAVLink, QGroundControl can detect the ESP32. That is because all ESP32s will register as a MAVLink device with the GCS.

{% hint style="warning" %}
**Known Issues with QGroundControl**

Regarding the use of QGroundControl with the `USBSerial` firmware:\
The GND-Unit ESP32 must be reset after every disconnect of QGroundControl. Press the reset button on the board once, then reconnect.

Regarding the use of QGroundControl with the `noUARTConsole` firmware:\
The GND-Unit ESP32 must be **reconfigured** after every disconnect of QGroundControl. QGroundControl is currently triggering a reset of the settings on reconnect.
{% endhint %}

The ESP32 will appear as Component 68.

<figure><img src="../../.gitbook/assets/grafik (5).png" alt=""><figcaption><p>ESP32s in MAVLink mode will appear in the MAVLink Inspector.</p></figcaption></figure>

At the moment, QGroundControl will only show the settings of the ESP32 AIR-Unit even when there is a GND-Unit connected as well.

<figure><img src="../../.gitbook/assets/grafik (2).png" alt=""><figcaption></figcaption></figure>
