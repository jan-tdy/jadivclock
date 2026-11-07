# Changelog

## Unreleased

- **CrowPanel 2.4"** (ESP32, ILI9341, XPT2046): new `hw/crowpanel_24.yaml` and
  `ui/base_24.yaml` (320x240 landscape clock, settings, Wi-Fi status). Feature
  packages are not adapted to this panel yet. See
  [docs/hardware-crowpanel-24.md](docs/hardware-crowpanel-24.md).

## v2.0.0

- **Message history** in `features/broadcast.yaml`: incoming messages and answers
  to questions are kept (newest first, last 30). The new *MESSAGES* tile shows
  the unread count and opens the history page; the answer to a question sent
  with `broadcast_send.yaml` lands in the same list.
- **Breaking:** `features/broadcast_send.yaml` now requires
  `features/broadcast.yaml`.
- Home header shows the Wi-Fi signal (useful in portrait/"phone" mode).
- Slider values of the lights are sent on release instead of on every change.
- Review fixes: TLS verification on by default (`http_verify_ssl`), NaN handling
  for unavailable Home Assistant entities, Todoist keeps its list on request
  errors, send errors are shown instead of losing the typed message.

## v1.0.0

First release: core, CrowPanel Advance 3.5" hardware package, base UI, and the
weather, Todoist, lights, thermometers, vacuum and broadcast features.
