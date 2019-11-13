---
description: Description of the different modules and how they communicate with each other.
---

# System Architecture

### Startup Sequence

All DroneBridge applications are started by the `start_db.service`. Most of the startup scripts are located inside the `startup` folder. Like any service it can be started, stopped & re-started. During stopping or restarting there might still be some bugs. 

![Startup Sequence of DroneBridge for Raspberry Pi](../.gitbook/assets/db_concept-start-up.svg)

### Release v0.6

Following Block Diagram shows how the different DroneBridge modules communicate with each other on the network & long range link. Some modules that do not have complex communication paths are not displayed \(SysLogServer, OSD\). Same goes for communication via shared memory.

#### GCS connected via network

In this case a GCS like DroneBridge for Android or QGroundControl is connected via TCP to the DroneBridge GND side. A UDP data stream \(video\) is sent via unicast to `192.168.2.1`. A different destination IP can be registered by sending a UDP packet of any size and content to the video module on UDP port `5000`.

![Ground Control Station \(GCS\) connected to DroneBridge GND station via network](../.gitbook/assets/db_concept-gcs-network-configuration.svg)

#### DroneBridge for Android via USB connection

In this case of a connected android phone to the DB GND station, the USB Bridge module auto-connects to all relevant modules and forwards the messages to the phone using the Android Accessory Protocol. At this point only the DroneBridge for Android app supports this mode.

![DroneBridge for Android connected via USB](../.gitbook/assets/db_concept-usb-configuration.svg)

