# Notes About Injection

## Atheros AR9271

A great chip with open source drivers & **firmware**. However it got some very strange behaviour when it comes to injection bit rates. Path of packets is as follows:

* Data rate specified in Radiotap header. Visible in Wireshark on sending interface
* Data rate gets overwritten by driver to 1Mbit?! Visible as a second packet in Wireshark in sending interface 
* Sent packet has data rate configured by `iw dev "$interface" set bitrates legacy-2.4 11`
* Data rate must be set while interface is in managed mode. Command will fail if device is on monitor mode

_The final data rate used by the system for sending is not visible in Wireshark on the sending interface_. This means that the firmware decides about the final data rate. To verify the transmission rate a second adapter is required. Rates can be set using following script:

{% code title="enable\_monitor" %}
```bash
#!/bin/bash
declare -a wifi_adps=("wlx00c0ca973410" "wlxfgdfgd513d1fg51")

frequency="2472"
bitrate="36"

for adapter in "${wifi_adps[@]}"
do
   echo "$adapter"
   interface="$adapter"
   ip link set "$interface" down
   iw dev "$interface" set type managed
   ip link set "$interface" up
   iw dev "$interface" set bitrates legacy-2.4 "$bitrate"
   ip link set "$interface" down
   iw dev "$interface" set type monitor
   ip link set "$interface" up
   iw dev "$interface" set freq $frequency
done

iwconfigc
```
{% endcode %}



