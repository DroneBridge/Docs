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
| :---: | :---: | :---: |


#### Radiotap header

Use any that works. You can set the transmission bit rate here \(works with Ralink cards only\). Higher bit rate means less latency and more data but also less range.

## Raw Protocol v2 header

Total length of the header is 10 bytes. Allows frames to be injected by most/all drivers supporting injection & monitor mode

<table>
  <thead>
    <tr>
      <th style="text-align:left">Byte</th>
      <th style="text-align:center">Field</th>
      <th style="text-align:center">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">0-3</td>
      <td style="text-align:center"><b>FCF duration</b>
      </td>
      <td style="text-align:center">
        <p>RTS frame: <code>0xb4 0x00 0x00 0x00</code>
        </p>
        <p>or</p>
        <p>Data frame: <code>0x08 0x00 0x00 0x00</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">4</td>
      <td style="text-align:center"><b>direction</b>
      </td>
      <td style="text-align:center">
        <p><code>0x01</code> if packet is for drone</p>
        <p><code>0x03</code> if packet is for ground station</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">5</td>
      <td style="text-align:center"><b>comm id</b>
      </td>
      <td style="text-align:center">Has to be the same on drone and ground station. Associates drone with
        ground station</td>
    </tr>
    <tr>
      <td style="text-align:left">6</td>
      <td style="text-align:center"><b>port</b>
      </td>
      <td style="text-align:center">
        <p>DB-control: <code>0x01</code>
        </p>
        <p>DB-telemetry: <code>0x02</code>
        </p>
        <p>DB-video: <code>0x03</code>
        </p>
        <p>DB-communication: <code>0x04</code>
        </p>
        <p>DB-status: <code>0x05</code>
        </p>
        <p>DB-proxy: <code>0x06</code>
        </p>
        <p>DB-RC: <code>0x07</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">7-8</td>
      <td style="text-align:center"><b>payload length</b>
      </td>
      <td style="text-align:center">
        <p>unsigned int 16 bit</p>
        <p>little endian</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">9</td>
      <td style="text-align:center"><b>sequence number</b>
      </td>
      <td style="text-align:center">increases with every frame sent to same port. Used to make software diversity
        possible. Range: <code>0-255</code>
      </td>
    </tr>
  </tbody>
</table>{% hint style="info" %}
If **DroneBridge compatibility mode** is enabled the **DB raw header gets extended by 10 bytes** of random data. These bytes are likely to overwritten on reception by un-patched WiFi drivers. The 10 byte padding makes sure no payload data is overwritten. Compatibility mode should be used in case a receiver with un-patched drivers is used \(e.g. default Ubuntu etc.\)

**Compatibility mode can automatically be detected** on reception if the **length of the received frame is bigger than the sum of: Radiotap header length, DB header length & payload length**
{% endhint %}

## Payload

Can be anything. Just make sure the transmission bit rate is set appropriate. In the case of DroneBridge the payload can be on of the following things:

* DroneBridge communication protocol \(change settings etc.\)
* MSP \(Multiwii Serial Protocol\)
* H264 in MPEG-TS container in case of GoPro video transmission using DroneBridge video module
* LTM \(Light Telemetry Protocol\)

## Encrypted Payload

The specification for encrypted messages is as follows:

<table>
  <thead>
    <tr>
      <th style="text-align:left">field</th>
      <th style="text-align:left">length</th>
      <th style="text-align:left">description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Nonce</b>
      </td>
      <td style="text-align:left">16 bytes</td>
      <td style="text-align:left">
        <p>Nonce as part of the encryption. Must not be reused for any other</p>
        <p>message with same key</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>MAC</b>
      </td>
      <td style="text-align:left">16 bytes</td>
      <td style="text-align:left">MAC or TAG to verify the integrity of the message &amp; authenticate the
        sender</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Payload</b>
      </td>
      <td style="text-align:left">0 - 1458 bytes</td>
      <td style="text-align:left">Encrypted data. Length must not exceed 1458 bytes since the <a href="https://de.wikipedia.org/wiki/Maximum_Transmission_Unit">MTU </a>in
        most WiFi drivers is ~1500 bytes</td>
    </tr>
  </tbody>
</table>The encrypted payload lies inside the DroneBridge raw protocol payload field. DroneBridge libraries support AES encryption with 128, 192, 256 bit key length. Authentication is done using [EAX](https://en.wikipedia.org/wiki/EAX_mode). The Python library uses the [PyCryptodomex ](https://pycryptodome.readthedocs.io/en/latest/)implementation. That way DroneBridge allows for end to end encryption of all messages. It is not recommended to enable it on latency sensitive applications like real-time video & audio streams.

