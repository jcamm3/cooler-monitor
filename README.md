# Cooler Monitor

Controls temperature in a cold storage room using a heater and external fans. Monitors Dallas 1-Wire temperature sensors (Room, Fins, Outside) and includes an OLED display with real-time status. Features runtime-settable target temperature, hysteresis, and freeze-recovery margin. Visual status indication via onboard RGB LED (green / blue / red flashing).

---

## Features

- **3× DS18B20** Dallas 1-Wire digital temperature sensors (Room, Fins, Outside)
- **SSD1306 OLED** 128×64 display with 8 rotating status pages
- **SNTP** time sync (America/New_York)
- **Built-in web interface** (v3) with grouped, sortable controls
- **Home Assistant API** integration with encrypted connection
- **Heater + External Fan** relay control with interlock
- **Freeze Recovery** mode — pauses cooling when fins ≤ 32 °F until fins warm above recovery threshold
- **Manual Deadband** switch — forces heater and fans off for scheduled/manual override
- **Runtime-configurable** target temperature, hysteresis range, and freeze-recovery margin
- **Onboard NeoPixel LED** status: Solid Green (on), Solid Red (off), Blink Blue (fans running), etc.
- **Safe boot** — relays disabled on boot if room sensor reads NaN
- **OTA updates** over ESPHome

---

## Hardware

| Component | Details |
|---|---|
| Microcontroller | ESP32-C3-DevKitM-1 |
| Temperature Sensors | DS18B20 Dallas 1-Wire × 3 (Room, Fins, Outside) |
| Display | SSD1306 OLED 128×64 (I2C) |
| Relay Board | 2-Channel GPIO relay (Heater AC + External Fans) |
| Status LED | Onboard WS2811 NeoPixel (GPIO8) |

---

## GPIO Pin Assignments

| Signal | GPIO |
|---|---|
| Heater (AC) Relay | GPIO04 |
| External Fans Relay | GPIO05 |
| I2C SCL (OLED) | GPIO07 |
| I2C SDA (OLED) | GPIO09 |
| NeoPixel (Onboard LED) | GPIO08 |
| 1-Wire Bus (DS18B20) | GPIO10 |

---

## OLED Display Pages

The display cycles through 8 pages every 10 seconds:

| Page | Content |
|---|---|
| 1 | Clock + Room Temp + Target Temp |
| 2 | Currently Running (AC / FANS / DEADBAND / FREEZE / OFF / MANUAL DB) |
| 3 | Target Temperature |
| 4 | Room Temperature |
| 5 | Fins Temperature |
| 6 | Outside Temperature |
| 7 | Hysteresis Range |
| 8 | Freeze Recovery Margin |

---

## Control Logic

The main interval (5 s) evaluates these conditions in order:

1. **Power off** → all relays off, all state flags cleared.
2. **Manual Deadband active** → heater and fans forced off regardless of temperature.
3. **Fins ≤ 32 °F and recovery not already running** → trigger Freeze Recovery script (heater + fans off for 60 s, then wait until fins > `32 + margin` before resuming normal control).
4. **External cooling available** (`outside ≤ room − 20 °F`) and room above upper threshold → fans on, heater off.
5. **Deadband active** and room ≥ upper threshold and fans not running → exit deadband, turn heater on.
6. **Room above target** (no deadband, no freeze recovery, fans not running) → heater on.
7. **Room at or below lower threshold** → heater off, enter deadband.

---

## Runtime Configuration (Web UI / Home Assistant)

| Parameter | Default | Range |
|---|---|---|
| Target Temperature | 34 °F | 34 – 121 °F |
| Hysteresis Range | 0.5 °F | 0 – 2 °F |
| Freeze Recovery Margin | 4 °F | 0 – 4 °F (added to 32 °F) |

---

## Getting Started

1. Clone this repo and open `cooler.yaml` in your ESPHome environment.
2. Create a `secrets.yaml` with your Wi-Fi credentials:
   ```yaml
   wifi_ssid: "YourSSID"
   wifi_password: "YourPassword"
   ```
3. Place the required font files in a `fonts/` subdirectory:
   - `arial.ttf`
   - `OpenSans-Regular.ttf`
   - `BebasNeue-Regular.ttf`
   - `Silkscreen-Regular.ttf`
   - *(Roboto is fetched via `gfonts://`)*
4. Flash the ESP32-C3-DevKitM-1 via ESPHome.
5. If Wi-Fi fails, connect to the fallback AP **"Cooler Fallback Hotspot"** and configure.
6. Access the web UI at `http://<device-ip>` (credentials: `saintmoor` / `saintmoor`).
7. Add the device to Home Assistant via the ESPHome integration.

---

## Sensor Addresses (DS18B20)

Update these in `cooler.yaml` to match your physical sensors:

| Sensor | 1-Wire Address |
|---|---|
| Room | `0x690417a1115aff28` |
| Fins | `0x990417a10ca3ff28` |
| Outside | `0x0e0417a116ffff28` |

---

## Version History

| Version | Date | Summary |
|---|---|---|
| 1.2.1 | 2025-06-28 | Bug fixes: freeze recovery trigger, fan/heater flag conflict, text sensor, test card |
| 1.2.0 | 2025-06-18 | Hardware pivot to ESP32-C3-DevKitM-1 + DS18B20; freeze recovery; manual deadband; web server v3 |
| 1.1.2 | 2025-06-27 | Manual deadband now forces relays off |
| 1.0.0 | 2025-06-09 | Initial release |

See [CHANGELOG.md](CHANGELOG.md) for full details.

---

## License

MIT License
