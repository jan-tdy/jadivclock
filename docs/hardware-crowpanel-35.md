# Hardware: Elecrow CrowPanel Advance 3.5"

`packages/hw/crowpanel_35.yaml` - tested with revisions V1.2 / V1.3 / V1.4
(DIS01835A).

| Function | Part | Pins |
|---|---|---|
| MCU | ESP32-S3, 16 MB flash, octal PSRAM | |
| Display | ILI9488, SPI, write-only | CLK GPIO42, MOSI GPIO39, CS GPIO40, DC GPIO41, RST GPIO2 |
| Backlight | PWM | GPIO38 (not default-on) |
| Touch | GT911 on I2C | SDA GPIO15, SCL GPIO16, address 0x5D |
| RTC | PCF8563 on I2C | 0x51 |
| Buzzer | self-oscillating, PWM duty | GPIO8 |

Notes:

* The panel is natively portrait 320x480. Landscape is applied at runtime with
  `lvgl.display.set_rotation: 270` (done by `goto_home` in `ui/base.yaml`).
* The ILI9488 needs `invert_colors: true`.
* The buzzer has a fixed pitch; sounds differ by pulse count (`beep` script).
* GPIO45 multiplexes the microphone circuit with the on-board LoRa module; this
  project does not use either.
* The touch controller answers at 0x5D on this board (not 0x14).

## Adding another panel

Create `packages/hw/<panel>.yaml` that provides the contract in
[ARCHITECTURE.md](ARCHITECTURE.md#hardware-contract). Pages in `ui/` and the
features are laid out for 480x320 landscape / 320x480 portrait; a panel with a
different resolution needs a different UI base.
