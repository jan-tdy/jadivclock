# Getting started

You need: an Elecrow CrowPanel Advance 3.5" (ESP32-S3), ESPHome 2026.9 or newer,
and (optionally) Home Assistant.

1. Create `secrets.yaml` next to your ESPHome config, based on
   [secrets.example.yaml](../secrets.example.yaml):

   ```yaml
   wifi_ssid: "..."
   wifi_password: "..."
   api_encryption_key: "..."   # openssl rand -base64 32
   ota_password: "..."
   todoist_token: "..."        # only with the todoist feature
   ```

2. Copy [examples/full.yaml](../examples/full.yaml) or
   [examples/minimal.yaml](../examples/minimal.yaml) next to it, save it as
   `my-clock.yaml` (any name works; use it in the commands below) and edit the
   substitutions. Remove the packages you do not want.
3. Connect the panel over USB-C and flash it:

   ```
   esphome run my-clock.yaml
   ```

   Later updates go over the air.
4. Add the device in Home Assistant (*Settings > Devices > ESPHome*) and enter
   the `api_encryption_key`. Features that talk to Home Assistant (lights,
   thermometers, vacuum, calendar, broadcast) need the device to be adopted and
   allowed to call actions (*Allow the device to perform Home Assistant
   actions* in the device's options).

## Substitutions

Every package documents its substitutions at the top of the file and in
[features/](features/). Define them in your own config; yours always win over
the package defaults.

Common ones:

| Substitution | Default | Meaning |
|---|---|---|
| `name` / `friendly_name` | `jadivclock` / `JadivClock` | device name |
| `timezone` | `UTC` | IANA time zone, e.g. `Europe/Berlin` |
| `user_name` | empty | greeting on the home screen |
| `http_verify_ssl` | `true` | TLS certificate check for Todoist / Open-Meteo; set `"false"` only to debug |
| `ui_default_mode` | `dashboard` | first-boot orientation (`dashboard` or `phone`) |
| `font_extra_glyphs` | empty | extra characters (accents) for the fonts |

## Pinning a version

The examples use `@main`. For a stable setup, replace `@main` with a release
tag (`@v0.1.0`) once there is one.

## Local development

```
git clone https://github.com/jan-tdy/jadivclock
cd jadivclock
cp secrets.example.yaml tests/secrets.yaml   # put a valid base64 api key in it
esphome config tests/full.yaml
```

`tests/*.yaml` use local `!include`s instead of `github://` so changes can be
validated before they are pushed.
