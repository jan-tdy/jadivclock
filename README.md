# JadivClock

A touch-screen smart-home panel firmware for [ESPHome](https://esphome.io),
built for the Elecrow CrowPanel Advance 3.5" (ESP32-S3, ILI9488 SPI display,
GT911 touch). You compose it from small packages and configure it with
substitutions - there is no code to edit.

> **Status: pre-release.** The packages validate with `esphome config` and
> compile with ESPHome 2026.9.1. Not every feature has been verified on a
> fresh install yet - see [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) and
> please open an issue if something misbehaves.

## Features (each one an optional package)

- Home screen with clock, date and a greeting; landscape or portrait, with
  auto-return to the home screen
- **Weather** - Open-Meteo, no API key: current, next 12 hours, 6 days
- **Todoist agenda** - complete and reschedule tasks, optionally with the next Home
  Assistant calendar event
- **Lights** - Home Assistant lights with brightness and colour temperature
- **Thermometers** - temperature, humidity and trend arrows
- **Vacuum** - robot vacuum status, pause/resume, dock
- **Broadcast** - receive popups from Home Assistant; send messages and
  questions (with an answer) to another panel
- Buzzer, backlight control, on-device settings

## Quick start

```yaml
substitutions:
  name: my-clock
  friendly_name: My JadivClock
  timezone: Europe/Berlin
  weather_lat: "52.52"
  weather_lon: "13.405"

packages:
  core: github://jan-tdy/jadivclock/packages/core.yaml@main
  hw: github://jan-tdy/jadivclock/packages/hw/crowpanel_35.yaml@main
  ui: github://jan-tdy/jadivclock/packages/ui/base.yaml@main
  weather: github://jan-tdy/jadivclock/packages/features/weather.yaml@main
```

Add the other features the same way. Full walk-through:
[docs/getting-started.md](docs/getting-started.md). Examples:
[examples/minimal.yaml](examples/minimal.yaml),
[examples/full.yaml](examples/full.yaml).

## Documentation

- [Getting started](docs/getting-started.md)
- [Architecture and design](docs/ARCHITECTURE.md)
- [Hardware: CrowPanel Advance 3.5"](docs/hardware-crowpanel-35.md)
- Features: [weather](docs/features/weather.md),
  [todoist](docs/features/todoist.md), [lights](docs/features/lights.md),
  [thermometers](docs/features/thermometers.md),
  [vacuum](docs/features/vacuum.md),
  [broadcast](docs/features/broadcast.md)
- [Home Assistant side of broadcast and questions](docs/home-assistant.md)

## License

MIT, see [LICENSE](LICENSE).
