# Architecture

JadivClock is a set of [ESPHome packages](https://esphome.io/components/packages/).
You pick the hardware package, the UI and the features you want; your own
config only holds substitutions (names, entity ids, coordinates) and a
`packages:` list.

```
packages/
  core.yaml                 identity, wifi, api, ota, sntp, http_request, diagnostics
  hw/
    crowpanel_35.yaml       ESP32-S3, ILI9488, GT911, RTC, backlight, buzzer, LVGL display
  ui/
    base.yaml               fonts, boot screen, home screen, settings, loading dialog
  features/
    weather.yaml            Open-Meteo page
    todoist.yaml            agenda page
    todoist_calendar.yaml   adds the next HA calendar event to the agenda
    lights.yaml             two Home Assistant lights
    thermometers.yaml       three temperature/humidity sensors with trend arrows
    vacuum.yaml             robot vacuum page
    broadcast.yaml          incoming message popup
    broadcast_send.yaml     composer + question/answer to another panel
```

## How packages combine

* Lists are concatenated and dictionaries merged by ESPHome. Items of an
  existing list that has to be *extended* (a page, a script) use `!extend`:

  ```yaml
  lvgl:
    pages:
      - id: !extend home_page   # add a tile to the home screen
        widgets:
          - button: ...
  ```

  This is why every UI feature requires `ui/base.yaml`: it owns `home_page`
  and the shared scripts (`goto_home`, `hide_loading_delayed`,
  `refresh_ha_display`, ...). Plain `pages:` entries with an `id` are *not*
  merged by ESPHome - a duplicate id is an error - so always use `!extend`.
* The home screen is a wrapping flex container. Each feature appends one tile
  after the header, so any combination of features fits and the same tiles
  reflow between landscape (3 per row) and portrait (2 per row). Up to 9 tiles
  fit in landscape.
* A link to a page lives in the package that owns the page, so a feature that is
  not included can never leave a dangling `lvgl.page.show:`.
* Dialogs and overlays are appended to `lvgl: top_layer: widgets:`.
* Features that need to react to Home Assistant reconnecting add steps to
  `api: on_client_connected: then:`.
* Features that cache state (lights, thermometers) extend `refresh_ha_display`,
  the script behind the *Refresh HA* button on the settings page.
* `todoist_calendar.yaml` writes into globals declared by `todoist.yaml`
  (`todo_cal_*`), so the Todoist code does not depend on the calendar
  package being present.

## Hardware contract

A `hw/` package must provide: `my_display`, `my_touch`, `backlight` (a light),
`buzzer_pwm` (an output), a script `beep` (parameter `count`), the global
`buzzer_muted`, and the `lvgl:` display/touchscreen settings. `rtc_time` is
optional. See [hardware-crowpanel-35.md](hardware-crowpanel-35.md).

## UI modes

The panel is natively portrait (320x480). `ui_mode` (restored from flash) is
either `phone` (portrait, rotation 0) or `dashboard` (landscape 480x320,
`lvgl.display.set_rotation: 270`). In dashboard mode the panel returns to the
home screen after 20 seconds without touch. Swipe down on the home screen in
dashboard mode to switch to portrait; in portrait it opens the quick settings
drawer.

## Known constraints

* `http_request` is synchronous: a script that calls it shows a loading dialog,
  waits `100ms` so LVGL can draw it, and only then does the blocking request.
* LVGL `buffer_size: 25%` is the best responsiveness/ghosting trade-off on the
  ILI9488.
* The main task stack is raised to 16 KiB; the JSON parsing in the Todoist and
  weather callbacks needs it.
* Avoid `bg_opa` alpha blending: with inverted colours and BGR order on this
  panel it renders wrong colours. Use solid colours.
* A Roboto `glyphs:` list *replaces* the default set, so it must contain full
  ASCII. `font_extra_glyphs` appends to it.
* Shapes with a full-circle radius rendered corrupted on this display; the
  weather icons use rounded squares instead.

## Tested with

ESPHome 2026.9.1 (LVGL 9.5, `lvgl.display.set_rotation`). CI validates
`tests/full.yaml` and `tests/minimal.yaml` with `esphome config`.

## Roadmap

* `hw/crowpanel_24.yaml` and `hw/crowpanel_43.yaml` (2.4" / 4.3" panels)
