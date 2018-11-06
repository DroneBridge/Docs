---
description: Embedded into raw protocol. Used to send RC channel data to UAV.
---

# RC message/packet

The DroneBridge RC packet is transported via the DroneBridge raw protocol. It is a possible payload.

With DroneBridge Beta v3 and higher you can decide whether you want to control your UAV by generating MSP/MAVLink messages on the groundstation or use the DroneBridge RC packet. If you choose to generate the final MSP/MAVLink-set-RC messages on the ground you can have up to 16 channels with MSP and 8 channels with MAVLink. Just send the message with the DroneBridge raw protocol to DB\_PORT\_CONTROL and the control module on the UAV will pass the message on to the flight controller.

Using the DroneBridge RC packet has the advantage of being lighter and thus causing less lost packets on your data down-link from the drone. This is a natural issue when you have a bi directional link on just one frequency. In case you do not want to have a second adapter operating on a different frequency on the drone and groundstation and still want to have good video performance it is recommended you control your drone with the DroneBridge control module \(groundstation\). It uses the DroneBridge RC packet and allows for 12 channels with 1024 steps of resolution. Note that when you are using a MAVLink based flight controller you have to set up a serial port for SUMD. MAVLink RC messages are not supported!

**DroneBridge RC messages are 52% shorter compared to MSPv2 messages with an equal number of channels while security and latency remain the same!**

## The packet

### Specification

| 12 channels | crc |
| :---: | :---: |
| 10bit/channel \(values from 0-1000\) | CRC-8K/3 of all channel data |
| Little-Endian | 8 bit |

CRC-8K/3 is defined as: `--width 8 --poly 0xA6 --reflect-in False --xor-in 0x00 --reflect-out False --xor-out 0x00`

It has a HD3 up to word lengths of 247 bit.

In the future a 16bit CRC with polynomial `0x9eb2` ****would deliver HD6 up to 135 bit word length. Further research must show how down link packet loss is affected by longer RC messages and if HD6 is necessary to guaranty a secure RC link.

### Example

A valid RC message could look like this with desired channel values of 1500. Note that we are sending a value of 500 with the RC protocol. The control module adds 1000 to every DB-RC value before creating the MSP/MAVLink message.

`0xf4 0xd1 0x47 0x1f 0x7d 0xf4 0xd1 0x47 0x1f 0x7d 0xf4 0xd1 0x47 0x1f 0x7d 0xC0`

Where `0xC0` is the crc and `0xf4 0xd1` contains data for RC channel 1 & 2

## Integration

The messages can be sent using the DroneBridge common-lib and the raw protocol. They are received on the drone by the control module on port 0x07. The control module builds valid MSP/MAVLink messages from the information and sends those to the flight controller. The control module adds 1000 to every value from the DB-RC message. That way valid MSP/MAVLink RC messages are created.

