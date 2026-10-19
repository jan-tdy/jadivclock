# Todoist agenda

`features/todoist.yaml` - up to six tasks from Todoist, sorted by time.

* Needs `todoist_token` in `secrets.yaml` (Todoist > Settings > Integrations >
  Developer > API token).
* Filters: *Today* (today + overdue) and *Overdue*, both limited to tasks
  assigned to you (`assigned to: me`, resolved by Todoist from the token).
* The checkbox completes a task. **NOW** reschedules a task to the next round
  quarter hour. NOW is hidden for recurring tasks, because rescheduling via
  `due_datetime` would drop the recurrence rule.
* Priorities are coloured: P1 red, P2 orange, P3 blue, P4 white.
* Task text uses the fonts' glyph set; add accented characters with
  `font_extra_glyphs`.

## Calendar row

`features/todoist_calendar.yaml` adds the next Home Assistant calendar event as
a row (purple, `CAL:`), sorted among the tasks. Home Assistant exposes only one
event (the next or current) as attributes, so it is one row.

| Substitution | Default |
|---|---|
| `calendar_entity` | `calendar.my_calendar` |
