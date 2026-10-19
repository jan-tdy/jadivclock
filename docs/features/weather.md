# Weather

`features/weather.yaml` - current conditions, a bar per hour for the next 12
hours, and a 6-day outlook from [Open-Meteo](https://open-meteo.com/) (no API
key). The icons are drawn from plain LVGL shapes.

| Substitution | Default | |
|---|---|---|
| `weather_lat`, `weather_lon` | London | location in decimal degrees |
| `weather_title` | `Weather` | page title |
| `weather_bar_min`, `weather_bar_max` | `-10`, `40` | temperature range (°C) of the hourly bars |

The forecast is requested in the location's time zone and matched against the
device clock: set `timezone` to the location's. Data is fetched when the page is
opened or *Refresh* is pressed, not continuously.
