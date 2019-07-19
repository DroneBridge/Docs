---
description: >-
  Exemplary commands allowing the video stream to be displayed using a Desktop
  OS.
---

# Video stream playback

{% hint style="warning" %}
At the moment none of the presented commands produces video playback with latency &lt; 1.5s  
Feel free to submit improvements.
{% endhint %}

{% hint style="info" %}
The video stream type needs to set to `raw` inside `DroneBridgeConfig.ini` or the receiver \(`video_gnd` module\) must run on the local computer sending the UDP data to `localhost`
{% endhint %}

Use on of the following commands to play back the live stream. Adjust the IP address if necessary. The computer must be connected to the ground station

```bash
gst-launch-1.0 fdsrc fd=0 ! h264parse ! rtph264pay pt=96 config-interval=5 ! udpsink port=5001 host=127.0.0.1 > /dev/null
```

```bash
ffplay -framerate 48 -flags nobuffer -flags low_delay  udp://127.0.0.1:5000
```

```bash
mplayer -benchmark -fps 48 -demuxer h264es udp://localhost:5000
```

```bash
mpv --no-cache --untimed --no-demuxer-thread --video-sync=audio --vd-lavc-threads=1 --hwdec=auto --no-correct-pts udp://127.0.0.1:5000
```

