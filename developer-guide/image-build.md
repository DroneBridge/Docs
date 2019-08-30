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
6. Add `/lib/udev/rules.d/51-android.rules`to enable USBBridge
7. Install the following python packages: `apt install python3.7 python3-pip ntp python3-psutil python3-serial python3-sysv-ipc python3-netifaces python3-rpi.gpio python3-evdev joystick hostapd`
8. `sudo pip3 install pyric pycryptodomex`
9. Install the following packages/libs: `cmake wiringPi udhcpd`[`openvg`](https://github.com/ajstarks/openvg)`libusb-1.0-0.dev libpcap-dev exfat-fuse exfat-utils dos2unix pump`  
10. Compile Raspberry Pi media libs  


    ```bash
    cd /opt/vc/src/hello_pi/
    make
    ```

11. Deactivate the cron service and all other services that you do not need
12. Clone the DroneBridge for Raspberry Pi git to `/home/pi`
13. Compile the DroneBridge modules using `cmake . && make`
14. Copy the `start_db` file to `/etc/init.d/`

    ```bash
    sudo cp /home/pi/DroneBridge/start_db /etc/init.d/start_db
    sudo update-rc.d -f start_db remove
    sudo update-rc.d start_db defaults
    ```

15. Add the DroneBridge syslog server to `/etc/init.d/rsyslog` script or replace the file with the provided `rsyslog` service file
16. Edit logrotation to manage `DroneBridge/log/db_modules.log`
17. Copy the `DroneBridgeConfig.ini`, `osdconfig.txt`, `apconfig.txt` and plugins folder to `/DroneBridge`
18. Create a folder named `/DroneBridge/osdfonts` and place the fonts you want to use for osd inside it. Do not forget about the `osdicons.ttf`
19. Create a symbolic link from `/DroneBridge/osdconfig.txt` to `/home/pi/DroneBridge/osd/osdconfig.h`
20. Add a line to `init.d/raspbi-config` to start the DroneBridge splash screen `/home/pi/dronebridge/splash`
21. Connect the WiFi adapters and check the EIRP of your system.



