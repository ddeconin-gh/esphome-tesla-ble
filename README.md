# ESPHome Tesla BLE

This project lets you use an ESP32 device to manage charging a Tesla vehicle over BLE. Tested with M5Stack NanoC6 and Tesla firmware 2024.20.9.

<img src="./docs/ha-device.png" width="300">

## Features
- [x] Pair BLE key with vehicle
- [x] Wake up vehicle (requires firmware 2024.26.x or newer when using the [Charging Manager](https://github.com/teslamotors/vehicle-command/blob/main/pkg/protocol/protocol.md#roles) role)
- [x] Set charging amps
- [x] Set charging limit (percent)
- [x] Turn on/off charging
- [x] Sleep state sensor 

## Usage

### Pre-requisites
- Python 3.10+
- GNU Make

### Finding the BLE MAC address of your vehicle

1. Copy and rename `secrets.yaml.example` to `secrets.yaml` and update it with your WiFi credentials (`wifi_ssid` and `wifi_password`) and vehicle VIN (`tesla_vin`).
1. Enable the `tesla_ble_listener` package in `packages/base.yml` and build the firmware.
1. Flash the firmware to your ESP32 device.
1. Open the ESPHome logs in Home Assistant andto wake it up. watch for the "Found Tesla vehicle" message, which will contain the BLE MAC address of your vehicle.
    > Note: The vehicle must be in range and awake for the BLE MAC address to be discovered. If the vehicle is not awake, open the Tesla app and run any command 
    ```log
    [00:00:00][D][tesla_ble_listener:044]: Parsing device: [CC:BB:D1:E2:34:F0]: BLE Device name 1
    [00:00:00][D][tesla_ble_listener:044]: Parsing device: [19:8A:BB:C3:D2:1F]: 
    [00:00:00][D][tesla_ble_listener:044]: Parsing device: [19:8A:BB:C3:D2:1F]:
    [00:00:00][D][tesla_ble_listener:044]: Parsing device: [F5:4E:3D:C2:1B:A0]: BLE Device name 2
    [00:00:00][D][tesla_ble_listener:044]: Parsing device: [A0:B1:C2:D3:E4:F5]: S1a87a5a75f3df858C
    [00:00:00][I][tesla_ble_listener:054]: Found Tesla vehicle | Name: S1a87a5a75f3df858C | MAC: A0:B1:C2:D3:E4:F5
    ```
1. Clean up your environment before the next step by disabling the `tesla_ble_listener` package in `packages/base.yml` and running
    ```sh
    make clean
    ```


### Building and flashing ESP32 firmware

1. Copy and rename `secrets.yaml.example` to `secrets.yaml` and update it with your WiFi credentials (`wifi_ssid` and `wifi_password`) and vehicle details (`ble_mac_address` and `tesla_vin`)


2. Build the image with [ESPHome](https://esphome.io/guides/getting_started_command_line.html)

    ```sh
    make compile
    ```

3. Upload/flash the firmware to the board.

    ```sh
    make upload
    ```

### Pairing the BLE key with your vehicle
1. Get into your vehicle
1. In Home Assistant, go to Settings > Devices & Services > ESPHome > Tesla BLE device and click "Pair BLE key"
1. Tap your NFC card to your car's center console
1. Hit confirm on the screen
1. [optional] Rename your key to "ESPHome BLE" to identify it easier
