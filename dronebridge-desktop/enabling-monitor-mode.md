---
description: Ways to prepare for receiving & sending data
---

# Enabling monitor mode

For DroneBridge to work the WiFi adapter must be in monitor mode. You can set the cards to monitor mode using the following bash script \(root privileges required\):

```bash
interface='yourinterfacename'
ip link set $interface down
iw dev $interface set type managed
ip link set $interface up
iw dev $interface set bitrates legacy-2.4 54
ip link set $interface down
iw dev $interface set type monitor
ip link set $interface up
iw dev $interface set freq 2472

echo $interface
iwconfig
```

In DroneBridge the script `/startup/init_wifi.py` is used to setup the cards. You may want to use it.

