---
description: >-
  Embedded into raw protocol. Used to send single messages to drone or ground
  station. Used to change settings. Implemented into communication module.
---

# Communication protocol

## What can it do?

* **Request settings** and configuration of drone, ground station and camera
* **Change settings** for all kinds of applications
* **Request & send firmware/hardware version**
* **Easily to be extended to send any command to ground station or drone**. Like shell commands

The communication protocol is implemented by the communication module. It is designed to send single messages that do not need high update rates. That way it is not necessary to map an RC channel to the desired function. RC switches are updated about 60 times/second and thus create a lot of interference.

This protocol is based on JSON which can be transported using the DroneBridge Raw protocol \(Groundstation &lt;--&gt; Drone\) or any standard network protocol like UDP/TCP \(wifi mode or USB tethering\)

## Structure

|  JSON | CRC32 |
| :--- | :--- |
|  UTF-8 | 4 bytes |

## Defined Messages

### Settings request

 Request of specific sections and keys of the config file. Destination can be `1, 2, 5`

```javascript
{ "destination": 1, 
  "type": "settingsrequest",
  "request": "db",
  "id": 1234,
  "settings": {
    "COMMON": ["communication_id", "rc_proto"],
    "GROUND": ["datarate", "en_control", "wifi_ap_if"],
    "AIR": ["datarate", "keyframerate"]
  }
}
```

### Settings response

```javascript
{
  "destination": 4,
  "type": "settingsresponse",
  "response": "db",
  "origin": "drone",
  "id": 1234,
  "settings": {
    "COMMON": {
      "name1": "value1",
      "name2": "value2"
    },
    "GROUND": {
      "name3": "value3",
      "name4": "value4"
    },
    "AIR": {
      "name5": "value5",
      "name6": "value6"
    }
  }
}
```

### Settings change request

 Change specific settings of the specified sections. Destination can be `1, 2 or 5`

```javascript
{
  "destination": 2,
  "type": "settingschange",
  "change": "db",
  "id": 4321,
  "settings": {
    "COMMON": {
      "freq": "2472",
      "communication_id": "202"
    },
    "GROUND": {
      "joy_interface": "0",
      "wifi_ap_if": "internal"
    },
    "AIR": {
      "en_plugin": "Y",
      "video_bitrate": "auto"
    }
  }
}
```

### Settings change success message

On successful settings change.

```javascript
{ "destination": 4, 
  "type": "settingssuccess",
  "origin": "drone",
  "id": 4321
}
```

### System identification request

```javascript
{ "destination": 1, 
  "type": "system_ident_req",
  "id": 4321
}
```

### System identification response

```javascript
{ "destination": 4,
  "type": "system_ident_rsp",
  "origin": "groundstation",
  "HID": 0,
  "FID": 101,
  "id": 4321
}
```

### "Calibrate" RC/Joystick

This is no real calibration. "jscal" is told to use the full range for each axis provided by the HID \(eg -127 to 127\). By default the system may only use values from -125 to 125 and adds a dead zone. As the user needs to calibrate his RC locally it is not necessary to do it all over again on the ground station. We just make sure the fully range is used.

```javascript
{ "destination": 1,
  "type": "adjustrc",
  "device": "/dev/input/js0",
  "id": 4321
}
```

### Ping request

```javascript
{ "destination": 2, 
  "type": "pingrequest",
  "id": 4321
}
```

### Ping response

```javascript
{ "destination": 4, 
  "type": "pingresponse",
  "origin": "drone",
  "id": 4321
}
```

### Error response

```javascript
{ "destination": 4, 
  "type": "error",
  "origin": "groundstation",
  "message": "Some kind of error message",
  "id": 4321
}
```

### ACK response

Universal acknowledgement message. May be returned by any kind of request/change message.

```javascript
{ "destination": 4, 
  "type": "ack",
  "id": 4321
}
```

## Fields

<table>
  <thead>
    <tr>
      <th style="text-align:left">JSON Key</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Possible values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"> <b>destination</b>
      </td>
      <td style="text-align:left">What device is supposed to process the message</td>
      <td style="text-align:left">
        <ul>
          <li><code>1</code>: ground station only</li>
          <li><code>2</code>: ground station & UAV</li>
          <li><code>3</code>: GoPro</li>
          <li><code>4</code>: App/GCS</li>
          <li><code>5</code>: UAV only</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>request</b>
      </td>
      <td style="text-align:left">What application settings are requested?</td>
      <td style="text-align:left"><code>db</code>: DroneBridge</td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>response</b>
      </td>
      <td style="text-align:left">What application settings are included</td>
      <td style="text-align:left">
        <p></p>
        <p><code>db</code>: DroneBridge</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>id</b>
      </td>
      <td style="text-align:left">Random 4 digit ID to identify request and response</td>
      <td style="text-align:left"><code>0000-9999</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>origin</b>
      </td>
      <td style="text-align:left">The origin/sender of the message</td>
      <td style="text-align:left">
        <ul>
          <li><code>drone</code>
          </li>
          <li><code>ground station</code>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>change</b>
      </td>
      <td style="text-align:left">What application settings shall be changed</td>
      <td style="text-align:left">
        <p></p>
        <p><code>db</code>: DroneBridge</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>settings</b>
      </td>
      <td style="text-align:left">JSON Object with all settings inside. Same keys as inside config file</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>HID</b>
      </td>
      <td style="text-align:left">Hardware ID</td>
      <td style="text-align:left">
        <ul>
          <li><code>0</code>: DroneBridge for RPi,</li>
          <li><code>1</code>: DroneBridge for ESP32 (WiFi mode)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> <b>FID</b>
      </td>
      <td style="text-align:left">Firmware version</td>
      <td style="text-align:left">divided by 100 to get exact formatting (101 = firmware version v1.10)</td>
    </tr>
  </tbody>
</table>## CRC32

 CRC32 of the entire JSON as implemented by the Python package `binascii`

 `binascii.crc32('my payload JSON')`

## Sequence Diagramms

![Settings request &amp; response between app, ground station &amp; UAS](../.gitbook/assets/comm_sequence_settingsrequest.jpg)

## Adding a custom message to the communication module

The communication module can transport messages of all kind. If a known message is received it does something. What it does is totally up to the users implementation. Often a user wants to send some sort of trigger to the UAV or ground station. Back in the days it was common to use one of the RC channels to trigger all sorts of actions. This is not very efficient since the RC sends all channels about 100 times/second even if you do not need that high update rates for your action. A simple camera ON/OFF switch would be an example.

 So let's say you want to change some setting of a camera connected to your AirPi. To do that you need to execute a command like `mycam -- shuterspeed 500` on the AirPi. What you do to implement this into DroneBridge is the following:

1. **Define a custom message that follows the specification of the communication protocol.** Just like [the new adjustrc message](https://github.com/seeul8er/DroneBridge/wiki/DroneBridge-communication-protocol#calibrate-rcjoystick). You can put as many parameters in there as you need. Your message could look like this: 

   ```javascript
   { "destination": 5,
     "type": "mycamshutter",
     "speed": 500,
     "id": 4321
   }
   ```

   _The parameters destination, type and id are mandatory._

2. **Make the communication module aware of the new message & write the code that should be executed**. [Add an if-clause that runs if your defined message-type is received](https://github.com/seeul8er/DroneBridge/blob/master/communication/DroneBridge_Protocol.py#L365)\*\*\*\*
3. **Generate a return message** to let the user know your message was received. Eg. it is as simple as calling [rtrn\_message = new\_ack\_message\(\)](https://github.com/seeul8er/DroneBridge/blob/master/communication/DroneBridge_Protocol.py#L367)

That's it! Now just send your newly defined message via UDP to the communication module and let it do the rest. [Here is an example on how to generate the entire communication message including the CRC32 at the end from a json](https://github.com/seeul8er/DroneBridge/blob/master/communication/db_comm_messages.py#L84). This will return the pure bytes of the message that now can be sent to the communication module \(UDP: Port 1605\) of the ground station.

With just a few lines of code, a user can implement any custom functionality with ease. No need to waste or flood the carrier with RC channels. No need to generate huge amounts of traffic.

