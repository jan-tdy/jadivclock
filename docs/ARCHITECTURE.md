# Architecture & extraction plan

## Decisions (2026-10-19)

| Topic | Decision |
|---|---|
| Scope | Firmware without attendance (Firebase / JapySoft backend stays private) |
| Hardware, first release | CrowPanel Advance 3.5" only; 2.4" and 4.3" later as extra `hw/` packages |
| License | MIT |
| UI language | English only (Slovak strings translated during extraction) |
| Repo | Brand-new repository, **do not reuse** `/home/pi/esphome` git history |

## Why a new repo

The existing `/home/pi/esphome` history never contained `secrets.yaml`
(verified: 0 commits touch it), but it holds 257 commits of private device
configs and personal entity names. A clean repo avoids leaking any of it.

## Package layout

```
packages/
  core.yaml                 wifi, api, ota, time/sntp, globals shared by all
  hw/crowpanel_35.yaml      spi, display, touch, backlight, buzzer, rtc, i2c, esp32 sdkconfig
  ui/base.yaml              lvgl: block, fonts, theme, home pages, settings, idle auto-return
  features/
    weather.yaml            Open-Meteo page + fetch script
    todoist.yaml            agenda page, complete/reschedule, optional calendar row
    lights.yaml             lights page + resync scripts
    thermometers.yaml       smart page: room temps/humidity + trend arrows
    vacuum.yaml             vacuum widget
    communication.yaml      code grid + confirm / value dialogs
    broadcast.yaml          incoming popup + composer + question/answer
```

Open design questions to prove with a small spike before the full split:

1. **LVGL pages across packages.** ESPHome merges lists of dicts by `id:`, so a
   feature package can contribute a `pages:` entry. The *home-screen tiles* that
   link to those pages need the same treatment: tiles must live in each feature
   package and be appended to a flex-wrap container (fixed grid cells will not
   work for optional features).
2. **Conditional UI.** Cross-page references (`lvgl.page.show: weather_page`)
   break if a package is not included, so every link must live inside the
   package that owns its target page.
3. **ESPHome version pin.** Remote packages should be documented with a tested
   minimum ESPHome version (LVGL 9.5.0 API, `lvgl.display.set_rotation`).

## Things that must become substitutions / user config

Everything personal found in `jadivclock35-p02.yaml` (4441 lines, ~60 unique HA
entity IDs hard-coded). Defaults go in each feature package, users override.

- Device identity: `name`, `friendly_name`, `emp_id`, `emp_label`, `att_zona` (attendance only, dropped)
- Location: `weather_lat`, `weather_lon` (currently Piconcillo)
- Lights: `light.svetla_jedalen`, `light.svetlo_pracovna` (+ room names)
- Thermometers: `sensor.teplomer_{vonku,jedalen,pracovna}_{temperature,humidity}`
- Vacuum: `vacuum.jedalen_rv30_max`, `sensor.jedalen_rv30_max_battery`,
  `sensor.podlazie_0_rv30_max_{status,current_room,clean_progress}`
- Calendar: `calendar.janik`
- Communication: `input_button.kod_*` (30 codes), `input_number.hodnota_hlad_332`,
  `input_number.hodnota_teplota`, `input_select.hlasenie_kto`,
  `input_select.hodnota_pavol_nalada` and the code labels/colours themselves.
  These belong to a private family "DevControl" scheme: ship them as an
  **example code table** users replace, not as built-in behaviour.
- Broadcast: `text.jadivclock4_3_jedalen_jadivclock_broadcast`,
  `text.podlazie_0_..._question`, `..._question_answer`
- Secrets: `wifi_*`, `jclock430001__{encryption_key,ota_password}`, `todoist_token`
  (rename to generic `api_encryption_key`, `ota_password`, ...)
- Dropped entirely: `firebase_url`, `firebase_secret`, punch / attendance pages,
  history list, `refresh_status`, `last_punch_*` globals, attendance dialogs.

## Extraction checklist

- [ ] Fill in the copyright holder in `LICENSE`
- [ ] Spike: core + hw + ui/base + one feature package compiles on the device
- [ ] Remove attendance code paths and everything that references them
- [ ] Translate Slovak UI text to English (fonts: glyph lists can shrink)
- [ ] Convert hard-coded entity IDs to substitutions with neutral defaults
- [ ] Split `lvgl:` pages / top_layer dialogs into feature packages
- [ ] `docs/` pages per feature + wiring photo for the CrowPanel
- [ ] Final scan for secrets, personal names, e-mails, IPs, URLs before the first push
- [ ] Tag a release; test `github://` remote packages from a clean ESPHome install

## Tooling note

No ESPHome install was available in the working environment (PyPI blocked by the
sandbox), so nothing here has been compiled or `esphome config`-validated yet.
