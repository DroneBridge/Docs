# Raw protocol

This protocol is used for long range communication in monitor mode. The basic idea is the same as with WifiBroadcast. The protocol allows simple plug & fly as no configuration is needed. Changing the `comm id` is required if multiple pilots want to use the same frequency.

## Protocol structure

| **Radiotap header** | **custom header** | **payload** |
| :---: | :---: | :---: |


#### Radiotap header

Use any that works. You can set the transmission bit rate here \(works with Ralink cards only\). Higher bit rate means less latency and more data but also less range.

## Raw Protocol v2 header

10 bytes in length compared to 24 bytes of v1. That means v2 header is 58% smaller than v1 causing less collisions and faster transfers without sacrificing features. Note that direction bytes and endianness have changed compared to v1! 

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
        <p>unsigned int 16bit</p>
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
</table>## Raw protocol v1 header \(deprecated - use v2!\)

It is a IEEE 802.11 data/beacon frame format with different meaning of the bytes. DroneBridge raw v1 uses 802.11 data frames with Ralink cards and beacon frames with Atheros cards to communicate.

| FCF | odd | direction | comm id | src. mac | version | port | direction | payload length | crc | Seq. num |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| 4 bytes | 1 byte | 1 byte | 4 bytes | 6 bytes | 1 byte | 1 byte | 1 byte | 2 bytes | 1 byte | 2 bytes |

<table>
  <thead>
    <tr>
      <th style="text-align:center">field</th>
      <th style="text-align:center">description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center"><b>FCF</b>
      </td>
      <td style="text-align:center">
        <p>Data frame: <code>0x08 0x00 0x00 0x00</code>
        </p>
        <p>Beacon frame: <code>0x80 0x00 0x00 0x00</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center"><b>odd</b>
      </td>
      <td style="text-align:center">First byte has to be <code>0x01</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:center"><b>direction</b>
      </td>
      <td style="text-align:center">see further down. For compatibility reasons with libpcap (this is part
        of dest mac address)</td>
    </tr>
    <tr>
      <td style="text-align:center"><b>comm id</b>
      </td>
      <td style="text-align:center">Has to be the same on drone and ground station. To determine for whom
        the packet is</td>
    </tr>
    <tr>
      <td style="text-align:center"><b>src. mac</b>
      </td>
      <td style="text-align:center">use the mac of sending interface. Nowhere used yet</td>
    </tr>
    <tr>
      <td style="text-align:center"><b>version</b>
      </td>
      <td style="text-align:center"><code>0x01</code>
      </td>
    </tr>
    <tr>
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
      </td>
    </tr>
    <tr>
      <td style="text-align:center"><b>direction</b>
      </td>
      <td style="text-align:center"><code>0x01</code> if packet is for drone <code>0x02</code> if packet is for
        ground station</td>
    </tr>
    <tr>
      <td style="text-align:center"><b>payload length</b>
      </td>
      <td style="text-align:center">
        <p>unsigned int 16bit</p>
        <p>little endian</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:center"><b>crc</b>
      </td>
      <td style="text-align:center">crc8 of <code>[version][port][direction][payload-length]</code> NOT USED
        YET</td>
    </tr>
    <tr>
      <td style="text-align:center"><b>sequ. num</b>
      </td>
      <td style="text-align:center">is overwritten anyway</td>
    </tr>
  </tbody>
</table>## Payload

Can be anything. Just make sure the transmission bit rate is set appropriate. In the case of DroneBridge the payload can be on of the following things:

* DroneBridge communication protocol \(change settings etc.\)
* MSP \(Multiwii Serial Protocol\)
* H264 in MPEG-TS container in case of GoPro video transmission using DroneBridge video module
* LTM \(Light Telemetry Protocol\)

