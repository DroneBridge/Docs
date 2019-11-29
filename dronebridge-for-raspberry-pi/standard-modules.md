---
description: Information on all standard modules on what they do and their inputs & outputs
---

# Standard Modules

## General

All modules use the common library to send & receive data via the long range link. Some modules exist in different implementations depending where they are running \(UAV, GND station\). All modules log to RSysLog.

## Video Module

Capable of transmitting a continuous stream of data that is **protected by forward error correction \(FEC\)**. **Any kind of data** can be fed to the UAV module and is stream to the ground.

**UAV**  
Input of data stream via STDIN. FEC packets are computed and sent to GND interleaved with data packets. FEC parameters and packet size can be set via config/command line parameters.

```c
-n Name of a network interface that should be used to receive the stream. 
   Must be in monitor mode. Multiple interfaces supported by calling this 
   option multiple times (-n inter1 -n inter2 -n interx)
-c [communication id] Choose a number from 0-255. Same on ground station and UAV!
-d Number of data packets in a block (default 8). Needs to match with RX
-r Number of FEC packets per block (default 4). Needs to match with RX
-f Bytes per packet (default 1024. max 1500). This is also the FEC block size. 
   Needs to match with RX
-b bit rate in Mbps [1|2|5|6|9|11|12|18|24|36|48|54]
   (bitrate option only supported with Ralink chipsets)
-t [1|2] DroneBridge v2 raw protocol packet/frame type: 
   1=RTS, 2=DATA (CTS protection)
-a [0|1] disable/enable. Offsets the payload by some bytes so that it sits outside 
   the 802.11 header. Set this to 1 if you are using a non DB-Rasp Kernel!
```

Example: Send data received via STDIN.

`cat mydatasource | video_air -n wlx185 -n wlx185 -c 200 -t 2`

**GND**  
Data from the UAV module is received and FEC decoded. Depending on the configuration the decoded data is sent to specified outputs. UDP endpoints can register with the module by sending a &gt;0 length UDP packet. This will change the UDP receiver to the senders IP. Destination port is `5000`.

```c
-n Name of a network interface that should be used to receive the stream. 
   Must be in monitor mode. Multiple interfaces supported by calling this 
   option multiple times (-n inter1 -n inter2 -n interx)
-c [communication id] Choose a number from 0-255. Same on ground station and UAV!
-d Number of data packets in a block (default 8). Needs to match with tx
-r Number of FEC packets per block (default 4). Needs to match with tx
-f Bytes per packet (default 1024. max 1500). This is also the FEC block size. 
   Needs to match with TX
-u [Y|N] to enable or disable UDP forwarding of decoded data
-i UDP DST IP overwrite: Send data to this IP
-p [Y|N] to enable/disable pass through of encoded FEC packets via UDP to 
   port: 5001
-v Destination port of video stream when set via UDP (IP checker address) or TCP
-o Send to output to unix domain socket at /tmp/db_video_out so that DroneBridge 
   USBBridge can forward it
-s Disable decoded output to stdout
```

Example call: Receive and forward STDOUT to a video player

`video_gnd -n wlx185 -n wlx185 -c 200 -u N | MyVideoPlayer -supportsH264`​

## Control Module

The control module is responsible for RC & telemetry on the UAV.

**UAV**  
On the UAV the control module operates the serial connections to the flight controller.

* Receive RC messages & forward them to the FC \(SUMD\)
* Receive messages from proxy module and forward them to the FC
* Receive data from the FC, parse data if configured and forward to proxy module
* Send status messages to status module \(CPU temp, RC packet-loss\)

```c
-n Name of a network interface that should be used to send & receive data. 
   Must be in monitor mode. Multiple interfaces supported by calling this 
   option multiple times (-n inter1 -n inter2 -n interx)
-u [MSP/MAVLink_Interface_TO_FC] - UART or VCP interface that is connected to FC
-v Protocol over serial port [1|2|3|4]:"
   1 = MSPv1 [Betaflight/Cleanflight]"
   2 = MSPv2 [iNAV] (default)"
   3 = MAVLink v1 (RC unsupported)"
   4 = MAVLink v2"
   5 = MAVLink (plain) pass through (-l <chunk size>) - recommended with MAVLink,
       FC needs to support MAVLink v2 for RC
-l Only relevant with -v 5 option. Telemetry bytes per packet over long range
   (default: %i)
-e [Y|N] enable/disable RC over SUMD. If disabled -v & -u options are used for RC.
-s Specify a serial port for use with SUMD. Ignored if SUMD is deactivated. Must be
   different from one specified with -u
-c [communication id] Choose a number from 0-255. Same on ground station and UAV!
-r Baud rate of the serial interface -u (MSP/MAVLink) 
   2400, 4800, 9600, 19200, 38400, 57600, 115200 (default: 115200))
-b bit rate in Mbps [1|2|5|6|9|11|12|18|24|36|48|54]
   (bitrate option only supported with Ralink chipsets)
-a [0|1] disable/enable. Offsets the payload by some bytes so that it sits outside 
   the 802.11 header. Set this to 1 if you are using a non DB-Rasp Kernel!
```

**GND**  
Read values from connected RC hardware and forward the channel data to the UAV module using the defined method/protocol. 

RC values can be overwritten by other applications \(e.g. plugins\) using a [shared memory object](https://github.com/seeul8er/DroneBridge/blob/nightly/common/shared_memory.c#L137).

{% hint style="info" %}
Depending on the protocol \(`-v` argument\) the data is sent to the DB-RC raw port of the control module or to its transparent pass through raw port \(forward to serial port\). In case of `-v [1-4]` the final serial message for the flight controller is generated on the ground station by the control module. The finished message is then sent to the Air module \(to same raw port the proxy module uses\) and forwarded to the flight controller via the serial connection. In case of `-v 5` the final serial message \(RC protocol\) is generated on the Air side. In that case the more efficient DB-RC message is used on the long range link.
{% endhint %}

```c
-n Name of a network interface that should be used to send & receive data. 
   Must be in monitor mode. Multiple interfaces supported by calling this 
   option multiple times (-n inter1 -n inter2 -n interx)
-j Index of joystick interface of RC (e.g. 0 for /dev/input/js0)
-v Protocol [1|2|4|5]: 
   1 = MSPv1 [Betaflight/Cleanflight]
   2 = MSPv2 [iNAV]
   3 = MAVLink v1
   4 = MAVLink v2
   5 = DB-RC (default)
-o [Y|N] enable/disable RC overwrite
-c [communication id] Choose a number from 0-255. Same on ground station and UAV!
-t [1|2] DroneBridge v2 raw protocol packet/frame type: 
   1=RTS
   2=DATA (CTS protection)
-r RC frequency in Hz (default 60 Hz)
-b bit rate in Mbps [1|2|5|6|9|11|12|18|24|36|48|54]
   (bitrate option only supported with Ralink chipsets)
-a [0|1] disable/enable. Offsets the payload by some bytes so that it sits outside 
   the 802.11 header. Set this to 1 if you are using a non DB-Rasp Kernel!
```

Example: Read from RC connected to `/dev/input/js0` and send to control module on Air side

`control_ground -n wlx684864 -c 200 -j 0 -v 5 -t 5 -r 70`

## Proxy Module

Runs on the ground station computer.

* Receives any kind of data via the long range link
* Forwards received data to all connected TCP clients
* Bi-Directional: Forwarding of TCP data via long range link to control module raw port
* Fully transparent
* Writes data received via long range link to `/root/telemetryfifo1` for further processing by the OSD

```c
-n Name of a network interface that should be used to send & receive data. 
   Must be in monitor mode. Multiple interfaces supported by calling this 
   option multiple times (-n inter1 -n inter2 -n interx)
-c [communication id] Choose a number from 0-255. Same on ground station and UAV!
-o [Y|N] Write telemetry to /root/telemetryfifo1 FIFO (default: Y)
-b bit rate in Mbps [1|2|5|6|9|11|12|18|24|36|48|54]
   (bitrate option only supported with Ralink chipsets)
-t [1|2] DroneBridge v2 raw protocol packet/frame type: 
   1=RTS, 2=DATA (CTS protection)
-a [0|1] disable/enable. Offsets the payload by some bytes so that it sits outside 
   the 802.11 header. Set this to 1 if you are using a non DB-Rasp Kernel!
```

Example: Wifi to DroneBridge long range link bridge:

`db_proxy -n wlx785 -c 200 -o N -t 2`

## Status Module

Receives and processes status messages like CPU load and temperature of the Air-Unit. Used for information with higher update rates \(&gt;5Hz\)

```c
-n Name of a network interface that should be used to send & receive data. 
   Must be in monitor mode. Multiple interfaces supported by calling this 
   option multiple times (-n inter1 -n inter2 -n interx)
-c [communication id] Choose a number from 0-255. Same on ground station and UAV!
```

## USBBridge

Used to forward TCP/UDP messages transparently between and android device and the DroneBridge modules. Implements the Android Open Accessory Protocol and the DroneBridge USB message.

## OSD

Runs on the ground station. Creates an OSD overlay based on telemetry received via the proxy module \(FIFO: `/root/telemetryfifo1`\).

{% hint style="warning" %}
Only compiles on the Raspberry Pi. Used OpenVG with some transparency mods.
{% endhint %}

## Syslog Server

Receives messages from RSysLog and forwards them to all connected TCP clients.



