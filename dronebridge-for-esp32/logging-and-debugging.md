# Logging & Debugging

## Optain Serial Logs

DroneBridge for ESP32, like all other ESP32 projects, logs data via the serial output. In case of bugs or errors, you can read those logs in real time.

For officially supported boards, just connect the USB port of the board to your computer. Alternatively, you can connect a serial-to-USB adapter to the debugging port of the board (TX & RX pins).

Any serial monitor application will work; however, for ease of use, I do recommend [esptool-js](https://espressif.github.io/esptool-js/) or the [ESPHome WebTools](https://esphome.github.io/esp-web-tools/). This will only work on Chrome-based browsers (e.g. Google Chrome or Microsoft Edge).

### Steps for esptool-js

1. Connect the ESP32 board to your computer (USB or serial to USB adapter)
2. Go to [esptool-js](https://espressif.github.io/esptool-js/) using Chrome or Edge as a browser
3. Under `Console` click connect selecting `115200` as the baud rate
4. Select the connected serial device
5. Once the console opens, click `Reset` to reboot the esp32 and start logging
