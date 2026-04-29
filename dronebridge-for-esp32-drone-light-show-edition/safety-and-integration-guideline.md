---
description: >-
  Valid for DroneBridge for ESP32 and specifically the Drone Light Show Edition
  (DB32 DLSE)
---

# Safety & Integration Guideline

**DroneBridge for ESP32 Drone Light Show Edition (DB32 DLSE) requires the following conditions to apply in order to operate nominally:**

* DB32 DLSE is supporting the following chips by Espressif Systems: ESP32-C3, ESP32-C6 & ESP32-C5
* DB32 DLSE is intended for use with the supported flight controller firmware of skybrush.io
* Updating any software or hardware in the command & control chain (e.g. DB32 DLSE firmware, flight controller firmware or ground control software) requires testing to check if the show system operates safely and as expected.
* The DB32 DLSE requires sufficiently powered ESP32 hardware during all of its operation\
  \&#xNAN;_An insufficient power supply may cause unexpected behaviour and system reboots._
* A flight controller must be connected to the ESP32s' hardware running DB32 DLSE using UART
* The flight controller & the DB32 DLSE must share a valid UART configuration (correct connection of pins and configuration of baud rate)
* The UART protocol must be set to MAVLink on both the flight controller & DB32 DLSE
* The flight controller must send regular heartbeats to the ESP32 DLSE via the UART connection\
  \&#xNAN;_Background: The ESP32 DLSE detects the armed state of the flight controller via heartbeat messages. In case no valid MAVLink message is received within a period of 1000ms, the DB32 DLSE assumes that the armed state is no longer valid & unknown. A power off command via the power management will be ignored._
* The flight controller may use MAVLink V1 or V2 as a serial protocol. Other protocols are not supported and may lead to unexpected behaviour.
* If the power management feature of DB32 DLSE is used, its correct and intended behaviour must be tested in combination with the used hardware before any flight operation. It remains the responsibility of the hardware & software integrator to configure and test this feature for robust & safe operation.
* If the DB32 DLSE is operated in EVALUATION mode, the flight controller or ground control station must send regular MAVLink SYSTEM\_TIME messages via the DB32 DLSE's serial or Wi-Fi port.
* When operated in TRAIL mode, the ESP32 shuts down the serial connection after 10 minutes. TRAIL mode is not permitted nor safe to use for operation. It is intended for testing & setup only.
* An OTA firmware update is only to be done while the drone is powered down. DB32 DLSE will abort an OTA firmware update if the flight controller is reported to be armed. In case of a MAVLink connection loss, the OTA update may be performed anyway since the DB32 DLS cannot determine the state of the flight controller correctly.
* Updating the installed license of DB32 DLSE is only to be done while the drone is disarmed to prevent unexpected behaviour. DroneBridge tries to block license updates while the UAV is armed.
* Changing the parameters/settings of DB32 DLSE is only to be done while the drone is disarmed to prevent unexpected behaviour. DroneBridge tries to block setting updates while the UAV is armed.

The DLSE is based on the well-tested public release of DroneBridge for ESP32. All features are designed with a lot of care and safety in the field in mind. However, a safe or crash-free operation cannot be guaranteed! Therefore, **the integrator of DLSE must respect the following constraints**:

1. DroneBridge for ESP32 DLSE may crash, reset, freeze or stop operating abruptly and without prior notice. Never rely on DLSE solely for the safe operation of your UAV. A loose contact, e.g., on the antenna connector, may cause a crash of the ESP32 firmware, requiring a reboot.\
   \&#xNAN;_Proposed mitigation action:_\
   &#xNAN;_&#x4E;ever use DLSE as the sole communication link. Make sure to have a backup link in place that allows for the safe operation (or safe backup mode) of your device._
2. DroneBridge for ESP32 DLSE may send wrong power control commands using the DLSE power management function.\
   \&#xNAN;_Proposed mitigation action:_\
   &#xNAN;_&#x54;he power module shall not be able to cut the power to the drone while the drone is airborne. E.g. by preventing a power off (even if commanded by the ESP32) when the current draw of the drone is higher than the idle current when on the ground._
3. DroneBridge for ESP32 may lock up internally, sending the same message continuously to the connected device.\
   \&#xNAN;_Proposed mitigation action: An additional data link should be able to override DLSE UART traffic in case the DLSE commands to the FC are compromised. E.g. this may be achieved by sending high rates of correct commands or by having a mode on the FC that does not rely on UART traffic from the DLSE._

While DB32 DLSE is a software-only product and hardware is the responsibility of the integrator, **please also consider the following incomplete list of hardware integration rules**:

* The ESP32 must be supplied with sufficient current as specified by the hardware specification of your board. Other consumers on the same power supply line may cause issues with the ESP32 power supply. E.g. a booting flight controller may pull the ESP32 power supply down, causing a reboot.
* When using the Power Management Function of DLSE it is highly recommended to use an active-low logic (low signal=power; high signal=no power) to trigger the power supply to the rest of the drone. This improves safety since a faulty ESP32 will likely pull all pins to low and thus not interrupt the power to the rest of the drone.\
  An alternative solution is to have a signal with a specific pulse length (not just a puls in general) to trigger the power-on/-off circuit. Such a signal is also less likely to be created by a failing ESP32.
* In case of U.FL antennas connected to the ESP32, consider securing the antenna and connector with additional means to protect them from loosening during flight due to vibrations. A drop of resin or super glue on the connector (antenna already connected) may give additional strength to the solder connection of the connector.
* ESP32 hardware must be protected from moisture. Water or moisture may cause a failure of the hardware.
* Check the temperature of the ESP32 hardware during all operating modes & environmental conditions to prevent overheating. Overheating of the ESP32 may cause degradation of the radio signal or unexpected (erratic) behaviour like reboots or crashes of the system.
* The placement of the ESP32 hardware and antenna may greatly impact its radio performance.
* Check for interference of the ESP32 hardware with the rest of the drone's hardware.
* Only used shielded ESP32 modules to prevent interference.
* The legal operation of DLSE according to the local regulations is the responsibility of the operator.\
  \&#xNAN;_Note: In Wi-Fi Client Mode, the DLSE will use the regulatory domain published by the access point to adjust the maximum transmission power of the ESP32._
