# Thermometers

`features/thermometers.yaml` - temperature, humidity and a trend arrow (red `^`
warmer, green `v` colder, `-` steady, compared to a reference value that is
refreshed every hour, so at most an hour old; threshold 0.2 °C) for three sensors. Sensor 1 is meant to be the outdoor sensor: its
temperature is also shown in the greeting line on the home screen.

| Substitution | Default |
|---|---|
| `thermo_N_name` | `Outdoor`, `Living room`, `Office` |
| `thermo_N_temp` | `sensor.outdoor_temperature`, ... |
| `thermo_N_humidity` | `sensor.outdoor_humidity`, ... |

(N = 1, 2, 3.)
