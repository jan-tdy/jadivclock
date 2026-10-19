# Vacuum

`features/vacuum.yaml` - status, room, battery, progress, pause/resume and dock
for a Home Assistant `vacuum` entity.

| Substitution | Default |
|---|---|
| `vacuum_entity` | `vacuum.robot` |
| `vacuum_status_entity` | `sensor.robot_vacuum_status` |
| `vacuum_room_entity` | `sensor.robot_vacuum_current_room` |
| `vacuum_battery_entity` | `sensor.robot_vacuum_battery` |
| `vacuum_progress_entity` | `sensor.robot_vacuum_clean_progress` |

Status, room, battery and progress come from separate sensor entities whose
names depend on your vacuum integration; missing ones show `--`. The
pause/resume button follows the vacuum entity's own state.
