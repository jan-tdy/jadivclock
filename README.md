# JadivClock

A touch-screen smart-home panel firmware for ESPHome, originally built for the
Elecrow CrowPanel Advance 3.5" (ESP32-S3, ILI9488 SPI display, GT911 touch).

> **Status: pre-release / work in progress.** This repository is being
> extracted from a personal single-file configuration. See
> [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the package layout and the
> extraction checklist.

## Features (planned as optional packages)

- Dashboard and phone (portrait) UI modes with auto-return to home screen
- Weather (Open-Meteo, no API key): current, 12-hour and 6-day forecast
- Todoist agenda (complete / reschedule tasks) plus next Home Assistant calendar event
- Home Assistant lights (brightness + colour temperature), room thermometers with trends
- Robot vacuum widget (pause / resume / dock)
- Quick-message code grid ("Communication") pressing Home Assistant `input_button`s
- Send / receive broadcasts and yes/no questions between panels (Home Assistant `text` entities)
- Buzzer, backlight control, Home Assistant exposed settings

Attendance (punch in/out) is intentionally **not** part of the open-source release.

## Usage (target design)

```yaml
substitutions:
  name: my-clock
  friendly_name: My JadivClock

packages:
  core: github://<owner>/jadivclock/packages/core.yaml@main
  hw: github://<owner>/jadivclock/packages/hw/crowpanel_35.yaml@main
  weather: github://<owner>/jadivclock/packages/features/weather.yaml@main
```

## License

MIT, see [LICENSE](LICENSE).
