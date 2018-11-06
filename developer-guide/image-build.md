---
description: How to build a working DroneBridge image for Raspberry Pi
---

# Image Build

It is highly recommended to modify the provided images since they already come with patched drivers and some other optimizations. 

The image for DroneBridge v0.6 is based on Raspbian Jessy Light with an updated/custom Kernel, bootcode and drivers to support the Raspberry Pi 3B+.

With the following steps you should be able to produce a working image yourself.

**This list is incomplete**

1. Get the latest Raspbian Lite ISO image and write it to a SD card.
2. [Get the latest \(already patched\) Kernel sources for the DroneBridge project](https://github.com/DroneBridge/RPiKernel/releases). Or create a custom Kernel.
3. Compile and install the Kernel for ARM v6 \(Pi0w\) & v7 \(Pi2+\)
4. Install the following python packages: `apt install python3 python3-pip ntp python3-psutil python3-serial python3-sysv-ipc python3-netifaces python3-rpi.gpio python3-evdev python3-pyric`
5. Install the following packages/libs: `cmake wiringPi udhcpd`[`openvg`](https://github.com/ajstarks/openvg)
6. Deactivate the cron service and all other services that you do not need
7. Clone the DroneBridge for Raspberry Pi git to `/root/dronebridge`
8. Compile the DroneBridge modules using `cmake . && make`
9. From the cloned repo: Copy the `.profile` file to `/root`
10. Copy the `DroneBridgeConfig.ini`, `osdconfig.txt`, `apconfig.txt` and plugins folder to `/boot`
11. Add a line to `init.d` to start the DroneBridge splash screen `/root/dronebridge/splash width_img height_img 1`
12. Connect the WiFi adapters and check the EIRP of your system.



