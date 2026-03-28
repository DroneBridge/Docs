# Troubleshooting/Help

### Support Forum

Visit the Discord channel for community support and further discussion:

[https://discord.gg/pqmHJNArE3](https://discord.gg/pqmHJNArE3)

### The ESP32 is not connecting to my access point

The ESP32 must be set to WiFi client mode. The access point must support 801.11b mode (sometimes also referred to as "legacy mode" - lower data rate but more range). The access point must have encryption enabled (min. WEP). The ESP32 can only connect to 2.4GHz networks. If in doubt reset the ESP32 to defaults and re-configure.

### I am locked out. How do I reset my ESP32 running DroneBridge?

There may be cases where you locked yourself out of the system & web interface. This may be the case when you no longer remember the access point password, ESP32 is configured as a WiFi client and the access point it tries to connect to is unavailable or when you have enabled an LR mode (WiFi or ESP-NOW).

You have the following options for resetting your ESP32:

* You can short-press the boot button on your ESP32 to force the reset of the mode to WiFi access point mode with the password "dronebridge"
* You can long-press the boot button on your ESP32 to force a complete reset of all settings to default settings incl. reboot.

### I flashed and the access point is not showing up

* Check the power connection (initially use USB to be sure)
* On boards with two USB sockets, the preferred socket for flashing is the one labelled as `COM`
* The online flasher does not verify that the flash was 100% successful. [Check the logs on startup the get a better view on the issue.](logging-and-debugging.md)
* Try scanning for the access point using your phone.
* On some Lenovo notebooks, you seem only to be able to connect to the ESP32 access points and website if you deactivate Bluetooth

### The web interface is not loading

* Try typing the ESP32's IP (AP mode it is `http://192.168.2.1`) directly instead of `dronebridge.local`

### I see no data in my ground station

1. Check the `UART received bytes` indicator in the web interface. If it is not showing data, then your wiring or your UART config is wrong
2. With the ESP32 in WiFi client mode and in case you want to use a UDP connection between ESP32 and the GCS: The GCS must send a UDP packet to the ESP32 to register and to be able to receive UDP data [(see configuration page for QGroundControl)](https://github.com/DroneBridge/ESP32/wiki/Configure-Ground-Station-&-Drone#qgroundcontrol--px4). Alternatively, go to the web interface and click the button to manually add a custom UDP target. Enter the PORT and IP of your GCS when being asked. [See the docs in the Wiki](https://github.com/DroneBridge/ESP32/wiki/Configuration#wifi-client-mode). Alternatively, try TCP.
3. Make sure your baud rates are aligned
4. Try a TCP connection
5. DroneBridge for ESP32 only supports continuous streams of data. It will only send a packet over the air when the defined packet size is reached (default is 64 bytes - see web interface config).
