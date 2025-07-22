# Cooler Monitor

Monitors temperature and humidity in a refrigerated environment using ESP32-C3 and SHT30 sensor. Displays real-time data on an OLED screen, synchronizes time using SNTP, and serves a built-in web interface for remote access.

## Features

- SHT30 digital temperature & humidity sensor
- SSD1306 OLED display for local readout
- SNTP time sync (PCF8563 RTC support)
- Built-in web interface for live readings
- Compact and power-efficient ESP32-C3 platform

## Hardware

- Seeed Studio XIAO ESP32-C3
- SHT30 sensor (I2C)
- SSD1306 OLED (128x64, I2C)
- PCF8563 RTC (I2C, optional)

## Wiring

| Component     | ESP32-C3 Pin |
|---------------|--------------|
| SHT30 SDA     | GPIO3        |
| SHT30 SCL     | GPIO2        |
| OLED SDA      | GPIO3        |
| OLED SCL      | GPIO2        |
| RTC SDA       | GPIO3        |
| RTC SCL       | GPIO2        |

## Getting Started

1. Flash the ESP32-C3 using ESPHome.
2. Update `secrets.yaml` with your Wi-Fi credentials.
3. Power the board and connect to the fallback AP if Wi-Fi fails.
4. Access the web UI at `http://<device-ip>`.

## License

MIT License
