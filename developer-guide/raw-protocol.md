---
description: Specification of the DroneBridge raw protocol
---

# Raw protocol

This protocol is used for long range communication in monitor mode. The basic idea is the same as with WifiBroadcast. The protocol allows simple plug & fly as no configuration is needed. Changing the `comm id` is required if multiple pilots want to use the same frequency.

{% hint style="info" %}
While it is optimized for use in UAS applications it is designed to carry any kind of payload
{% endhint %}

## Protocol structure

| Radiotap header | DroneBridge custom header | payload |
| :-------------: | :-----------------------: | :-----: |

#### Radiotap header

Use any that works. You can set the transmission bit rate here (works with Ralink cards only). Higher bit rate means less latency and more data but also less range.

## Raw Protocol v2 header

Total length of the header is 10 bytes. Allows frames to be injected by most/all drivers supporting injection & monitor mode

| Byte |        Field        |                                                                                                                         Description                                                                                                                        |
| ---- | :-----------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| 0-3  |   **FCF duration**  |                                                                            <p>RTS frame: <code>0xb4 0x00 0x00 0x00</code></p><p>Data frame: <code>0x08 0x00 0x00 0x00</code></p>                                                                           |
| 4    |    **direction**    |                                                                           <p><code>0x01</code> if packet is for drone</p><p><code>0x03</code> if packet is for ground station</p>                                                                          |
| 5    |     **comm id**     |                                                                                    Has to be the same on drone and ground station. Associates drone with ground station                                                                                    |
| 6    |       **port**      | <p>DB-control: <code>0x01</code></p><p>DB-telemetry: <code>0x02</code></p><p>DB-video: <code>0x03</code></p><p>DB-communication: <code>0x04</code></p><p>DB-status: <code>0x05</code></p><p>DB-proxy: <code>0x06</code></p><p>DB-RC: <code>0x07</code></p> |
| 7-8  |  **payload length** |                                                                                                       <p>unsigned int 16 bit </p><p>little endian</p>                                                                                                      |
| 9    | **sequence number** |                                                                           increases with every frame sent to same port. Used to make software diversity possible. Range: `0-255`                                                                           |

{% hint style="info" %}
If **DroneBridge compatibility mode** is enabled the **DB raw header gets extended by 10 bytes** of random data. These bytes are likely to overwritten on reception by un-patched WiFi drivers. The 10 byte padding makes sure no payload data is overwritten. Compatibility mode should be used in case a receiver with un-patched drivers is used (e.g. default Ubuntu etc.)

**Compatibility mode can automatically be detected** on reception if the **length of the received frame is bigger than the sum of: Radiotap header length, DB header length & payload length**
{% endhint %}

## Payload

{% hint style="danger" %}
Minimal payload length depends on used frame type (tested with AR9271):

* Data frame: minimal payload length: **14 bytes**
* RTS frame: minimal payload length: **6 bytes**
* Beacon frame: minimal payload length: **14 bytes**
{% endhint %}

Can be anything. Just make sure the transmission bit rate is set appropriate. In the case of DroneBridge the payload can be on of the following things:

* DroneBridge communication protocol (change settings etc.)
* MSP (Multiwii Serial Protocol)
* H264 in MPEG-TS container in case of GoPro video transmission using DroneBridge video module
* LTM (Light Telemetry Protocol)

## Encrypted Payload

The specification for encrypted messages is as follows:

| field       | length         | description                                                                                                                                                     |
| ----------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Nonce**   | 16 bytes       | <p>Nonce as part of the encryption. Must not be reused for any other </p><p>message with same key</p>                                                           |
| **MAC**     | 16 bytes       | MAC or TAG to verify the integrity of the message & authenticate the sender                                                                                     |
| **Payload** | 0 - 1458 bytes | Encrypted data. Length must not exceed 1458 bytes since the [MTU ](https://de.wikipedia.org/wiki/Maximum_Transmission_Unit)in most WiFi drivers is \~1500 bytes |

The encrypted payload lies inside the DroneBridge raw protocol payload field. DroneBridge libraries support AES encryption with 128, 192, 256 bit key length. Authentication is done using [EAX](https://en.wikipedia.org/wiki/EAX_mode). The Python library uses the [PyCryptodomex ](https://pycryptodome.readthedocs.io/en/latest/)implementation. That way DroneBridge allows for end to end encryption of all messages. It is not recommended to enable it on latency sensitive applications like real-time video & audio streams.
