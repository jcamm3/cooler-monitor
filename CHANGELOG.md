# Changelog

All notable changes to this project will be documented in this file.

---

## [1.2.1] - 2025-06-28

### Fixed
- Freeze Recovery script was never triggered — added `freeze_recovery_script.execute()` call to the main 5s control loop when fins ≤ 32 °F and recovery is not already active.
- Heater activation conditions now check `fan_state` to prevent `ac_state` and `fan_state` flags both being set to true in the same evaluation tick (relay interlock prevented physical conflict, but state flags were inconsistent).
- `Currently Running` text sensor now correctly surfaces `Manual Deadband` as the highest-priority status, matching OLED page 2 behavior.
- `show_test_card` set to `false` — removes the test pattern flash on every reboot.
- Corrected stale log message from `"15s interval logic executed"` to `"5s interval logic executed"`.

---

## [1.2.0] - 2025-06-18

### Changed
- Bumped version to 1.2.0 in file header.

---

## [1.1.2] - 2025-06-27

### Fixed
- Manual Deadband now forcibly disables heater and fans when active, regardless of temperature logic state.

---

## [1.1.1] - 2025-06-22

### Added
- `manual_deadband_state` global variable and `Enable Deadband (Manual)` template switch.
- Manual Deadband binary sensor and OLED page 2 status label ("MANUAL DB").
- Manual deadband override block in main 5 s control loop.

### Changed
- OLED page 2 "Currently Running" now surfaces MANUAL DB as the highest-priority status.

---

## [1.1.0] - 2025-06-18

### Added
- Freeze Recovery script: pauses cooling when fins ≤ 32 °F, waits until fins warm above `32 + safety_margin` before resuming.
- `freeze_recovery` global and `Freeze Recovery` binary sensor.
- `Freeze Recovery Margin` number control (runtime-settable, 0–4 °F added to 32 °F floor).
- `Freeze Recovery Temp` template sensor reflecting current recovery threshold.
- OLED pages 7 (Hysteresis) and 8 (Recovery Margin).
- Web server v3 with sorting groups: Relays, Environment, Config, Status.
- Manual `OLED Display` template switch.
- ` Power Switch` template switch with LED feedback (green on / red off).
- Sensor error handler: readings above 125 °F log an error and retry after 30 s.
- Safe boot: relays disabled if room sensor is NaN at startup.

### Changed
- Board updated from Seeed XIAO ESP32-C3 to **ESP32-C3-DevKitM-1**.
- Temperature sensors changed from SHT30 (I2C temp/humidity) to **3× DS18B20** (1-Wire: Room, Fins, Outside).
- GPIO assignments revised to match DevKitM-1 layout (see README).
- LED driver changed from `neopixelbus` to `esp32_rmt_led_strip` (WS2811 GRB).
- I2C SDA/SCL updated to GPIO9/GPIO7.
- NTP timezone set to `America/New_York`.
- Heater and fan relays configured with `interlock` to prevent simultaneous activation.

---

## [1.0.0] - 2025-06-09

### Added
- Initial release for Smart Cooler.
- Core logic for relay / temperature / fan control.
- OLED status display with multi-page info.
- Web server & Home Assistant integration.
- Runtime configuration for setpoint, hysteresis, and margin.
- Secure OTA and API integration.
