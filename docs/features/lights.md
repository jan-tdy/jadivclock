# Lights

`features/lights.yaml` - switch, brightness and colour temperature (2000-6500 K)
for two Home Assistant lights. The widgets follow the real state in Home
Assistant, including changes made elsewhere.

| Substitution | Default |
|---|---|
| `light_1_entity`, `light_1_name` | `light.living_room`, `Living room` |
| `light_2_entity`, `light_2_name` | `light.office`, `Office` |

The device must be allowed to perform Home Assistant actions. Lights without
colour-temperature support simply ignore the slider.
