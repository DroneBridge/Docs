---
description: How to build a working DroneBridge image for Raspberry Pi
---

# Image Build

It is highly recommended to modify the provided images since they already come with patched drivers and some other optimizations. 

The image for DroneBridge v0.6 is based on Raspbian Jessy Light with an updated/custom Kernel, bootcode and drivers to support the Raspberry Pi 3B+.

With the following steps you should be able to produce a working image yourself.

**The list may be incomplete**

1. Get the latest Raspbian Lite ISO image and write it to a SD card.
2. [Get the latest \(already patched\) Kernel sources for the DroneBridge project](https://github.com/DroneBridge/RPiKernel/releases). Or create a custom Kernel.
3. Compile and install the Kernel for ARM v6 \(Pi0w\) & v7 \(Pi2+\) & v8 \(Pi4\)
4. Create a new patition named "DroneBridge"  & format it with exFAT. Add the new patition to fstab
5. Adjust the resize scripts inside /etc/init.d/
6. Install the following python packages: `apt install python3.7 python3-pip ntp python3-psutil python3-serial python3-sysv-ipc python3-netifaces python3-rpi.gpio python3-evdev python3-pyric python3-pycryptodomex`
7. Install the following packages/libs: `cmake wiringPi udhcpd`[`openvg`](https://github.com/ajstarks/openvg)
8. Deactivate the cron service and all other services that you do not need
9. Clone the DroneBridge for Raspberry Pi git to `/home/pi`
10. Compile the DroneBridge modules using `cmake . && make`
11. Copy the `start_db` file to `/etc/init.d/`

    ```bash
    cp /home/pi/DroneBridge/start_db /etc/init.d/start_db
    sudo update-rc.d -f start_db remove
    sudo update-rc.d start_db defaults
    ```

12. Copy the `DroneBridgeConfig.ini`, `osdconfig.txt`, `apconfig.txt` and plugins folder to `/DroneBridge`
13. Add a line to `init.d/raspbi-config` to start the DroneBridge splash screen `/home/pi/dronebridge/splash`
14. Connect the WiFi adapters and check the EIRP of your system.



