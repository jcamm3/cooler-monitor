# Cooler Monitor

Controls temperature in a cold storage room using a heater and external fans. Monitors temperatures (AC Fins, External, Internal) and includes OLED display with real-time status. Features runtime-settable target, margin, and hysteresis. Visual status indication via onboard RGB LED (green/blue/red flashing).

## Features

- 3x Dallas digital temperature sensors
- SSD1306 OLED display for local readout
- SNTP time sync (PCF8563 RTC support)
- Built-in web interface for live readings
- Compact and power-efficient ESP32-C3 platform
- Turns AC and external fans on/off based on temperature

## Hardware

- Seeed Studio XIAO ESP32-C3
- Dallas Temperature Sensors (OneWire)
- SSD1306 OLED (128x64, I2C)
- PCF8563 RTC (I2C, optional)
- 2-Channel 5V Relay Board

## Wiring

| Component     | ESP32-C3 Pin |
|---------------|--------------|
| OLED SDA      | GPIO3        |
| OLED SCL      | GPIO2        |
| RTC SDA       | GPIO3        |
| RTC SCL       | GPIO2        |
| RELAY 1       | GPIO4        |
| RELAY 2       | GPIO5        |

## Getting Started

1. Flash the ESP32-C3 using ESPHome.
2. Update `secrets.yaml` with your Wi-Fi credentials.
3. Power the board and connect to the fallback AP if Wi-Fi fails.
4. Access the web UI at `http://<device-ip>`.

## License

MIT License
