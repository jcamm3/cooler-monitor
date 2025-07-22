# Cooler Monitor Documentation

This device monitors the conditions inside a refrigerated space, using an ESP32-C3 microcontroller and I2C-connected sensors and display.

## Components

- **ESP32-C3**: Main controller, with Wi-Fi and low power consumption
- **SHT30**: Digital sensor for temperature and humidity
- **SSD1306 OLED**: Displays real-time sensor data and clock
- **PCF8563 RTC** (optional): Keeps time across reboots

## Display Interface

- Page 1: Temperature & Humidity
- Page 2: Current Time
- Page 3: IP Address

## Web Interface

Accessible at `http://<device-ip>`, showing:
- Temperature (°C / °F)
- Humidity (%)
- Uptime
- IP Address

## Wiring Notes

All I2C devices (SHT30, OLED, RTC) share GPIO2 (SCL) and GPIO3 (SDA). Use appropriate pull-ups if needed.
