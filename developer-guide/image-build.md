---
description: How to build a working DroneBridge image for Raspberry Pi
---

# Image Build

It is highly recommended to modify the provided images since they already come with patched drivers and some other optimizations. 

The image for DroneBridge v0.6 is based on Raspbian Jessy Light with an updated/custom Kernel, bootcode and drivers to support the Raspberry Pi 3B+.

With the following steps you should be able to produce a working image yourself.

**The list may be incomplete**

1. Get the latest Raspbian Lite ISO image and write it to a SD card.
2. [Compile a custom Kernel](https://github.com/DroneBridge/RPiKernelBuilder) and add it to your image in order to get higher output power etc. Skip this step if you do not know what you are doing! Your system might send with too much power afterwards! \(check EIRP\)
3. Create a new partition named "DroneBridge"  & format it with exFAT. Add the new partition to fstab
4. Adjust the resize scripts inside /etc/init.d/
5. Add `/lib/udev/rules.d/51-android.rules`to enable USBBridge \(already part of custom Kernel\)
6. Install the following python packages: `apt install python3.7 python3-pip ntp python3-psutil python3-serial python3-sysv-ipc python3-netifaces python3-rpi.gpio python3-evdev joystick hostapd`
7. `sudo pip3 install pyric pycryptodomex`
8. Install the following packages/libs: `cmake wiringPi udhcpd`[`openvg`](https://github.com/ajstarks/openvg)`libusb-1.0-0.dev libpcap-dev exfat-fuse exfat-utils dos2unix pump`  
9. Compile Raspberry Pi media libs

   ```bash
   cd /opt/vc/src/hello_pi/
   make
   ```

10. Deactivate the cron service and all other services that you do not need
11. Add following lines to `/etc/dhcpcd.conf`to make the hotpot work. For the hotspot we use udhcpd `denyinterfaces wifihotspot0 denyinterfaces wlan0`
12. Clone the DroneBridge for Raspberry Pi git to `/home/pi`
13. Compile the DroneBridge modules using `cmake . && make`
14. Copy the `start_db` file to `/etc/init.d/`

    ```bash
    sudo cp /home/pi/DroneBridge/start_db /etc/init.d/start_db
    sudo update-rc.d -f start_db remove
    sudo update-rc.d start_db defaults
    ```

15. Edit log rotation to manage `DroneBridge/log/db_modules.log`
16. Copy the `DroneBridgeConfig.ini`, `osdconfig.txt`, `apconfig.txt` and plugins folder to `/DroneBridge`
17. Create a folder named `/DroneBridge/osdfonts` and place the fonts you want to use for osd inside it. Do not forget about the `osdicons.ttf`
18. Create a symbolic link from `/DroneBridge/osdconfig.txt` to `/home/pi/DroneBridge/osd/osdconfig.h`
19. Add a line to `init.d/raspbi-config` to start the DroneBridge splash screen `/home/pi/dronebridge/splash`
20. Connect the WiFi adapters and check the EIRP of your system.



