# Logging & Debugging

## Optain Serial Logs

DroneBridge for ESP32, like all other ESP32 projects, logs data via the serial output. In case of bugs or errors you can read those logs in real time.

For officially supported boards just connect the USB port of the board to your computer. Alternatively, you can connect a serial to USB adapter to the debugging port of the board (TX & RX pins).

Any serial monitor application will work, however, for ease of use I do recommend [esptool-js](https://espressif.github.io/esptool-js/) or the [ESPHome WebTools](https://esphome.github.io/esp-web-tools/). This will only work on Chrome-based browsers (e.g. Google Chrome or Microsoft Edge).

### Steps for esptool-js

1. Connect the ESP32 board to your computer (USB or serial to USB adapter)
2. Go to [esptool-js](https://espressif.github.io/esptool-js/) using Chrome or Edge as a browser
3. Under `Console` click connect selecting `115200` as the baud rate
4. Select the connected serial device
5. Once the console opens click `Reset` to reboot the esp32 and start logging

![grafik](https://private-user-images.githubusercontent.com/24637325/313427839-3944bed7-2a99-42c0-8aec-ea2d4dd251df.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjUxNDY0MDMsIm5iZiI6MTcyNTE0NjEwMywicGF0aCI6Ii8yNDYzNzMyNS8zMTM0Mjc4MzktMzk0NGJlZDctMmE5OS00MmMwLThhZWMtZWEyZDRkZDI1MWRmLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MzElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODMxVDIzMTUwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTVhMDQwNGFhZWU2ZmUwMTg4ZmUxYWEzMjhmMjg0M2VmYWUyYWJiNWRjZDkyMTNjMTJiYmNhNDIyMTI0YjdmMTImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.8n0jIrbmI-GtJmZY2xHm_te_XV1HL4Vm5if-Qip7fr0)
