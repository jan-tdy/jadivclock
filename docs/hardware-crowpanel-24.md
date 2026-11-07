# Hardware: Elecrow CrowPanel 2.4" (Basic series)

`packages/hw/crowpanel_24.yaml` - the 2.4" ESP32 display (ESP32-WROOM-32-N4),
**not** the "Advance" 3.5" model. Use it with `packages/ui/base_24.yaml`.

| Function | Part | Pins |
|---|---|---|
| MCU | ESP32-WROOM-32-N4, 4 MB flash, **no PSRAM** | |
| Display | ILI9341, 240x320, SPI | CLK GPIO14, MOSI GPIO13, MISO GPIO12, CS GPIO15, DC GPIO2, RST GPIO4 |
| Touch | XPT2046, resistive, shares the SPI bus | CS GPIO33 |
| Backlight | PWM | GPIO27 |
| Buzzer | none onboard; optional external module | GPIO25 (`buzzer_pin`) |
| RTC | none | time comes from SNTP only |

Notes:

* Landscape 320x240 is done in hardware (`transform: swap_xy`) and the display
  is given `dimensions: 320x240`. Do not use `lvgl: rotation:` here: the
  touchscreen would then scale touches to the unrotated 240 px width and the
  right quarter of the screen could not be pressed.
* Colours: the package uses `color_order: rgb`, `invert_colors: false`, which
  matched the units tested. If the boot-screen strip is not red, green, blue,
  yellow, cyan, magenta, or the UI has a white background, try
  `color_order: bgr` and `invert_colors: true` (Elecrow's defaults).
* Black screen: Elecrow's own sources disagree about the reset pin. If nothing
  appears, remove `reset_pin: GPIO4` first.
* Touch calibration is `300..3900` on both axes with `swap_xy` + `mirror_x`.
  If a corner is off, log the raw values and adjust `calibration:`.
* No PSRAM: do not add a `psram:` block or PSRAM build flags (copied into
  Elecrow's example from the 2.8" model); the boot fails with "PSRAM chip not
  found".
* No onboard buzzer. An external module (KY-006 passive / KY-012 active) on
  GPIO25 works; override `buzzer_pin` / `buzzer_frequency` if you wire it
  elsewhere. Without it, `beep` is simply silent.
* The clock is blank (`--:--`) for a few seconds after a cold boot until Wi-Fi
  and SNTP are up.

## Limitations

`ui/base_24.yaml` is a fixed 320x240 landscape UI (no portrait mode, no swipe
gestures). The feature packages in `features/` are laid out for 480x320 /
320x480 and are not adapted to 320x240 yet, so only `core` + `crowpanel_24` +
`base_24` (clock, settings, Wi-Fi status) are supported on this panel.
See [examples/minimal_24.yaml](../examples/minimal_24.yaml).
