# Todoist agenda

`features/todoist.yaml` - up to six tasks from Todoist, sorted by time.

* Needs `todoist_token` in `secrets.yaml` (Todoist > Settings > Integrations >
  Developer > API token).
* Filters: *Today* (today + overdue) and *Overdue*, both excluding tasks
  assigned to other people (`!assigned to: others`). In a shared workspace, set
  `todoist_assignee_filter: "assigned%20to%3A%20me"` to see only tasks assigned
  to you.
* A failed request (HTTP error, bad token) leaves the current rows in place.
* The checkbox completes a task. **NOW** reschedules a task to the next round
  quarter hour. NOW is hidden for recurring tasks, because rescheduling via
  `due_datetime` would drop the recurrence rule.
* Priorities are coloured: P1 red, P2 orange, P3 blue, P4 white.
* Task text uses the fonts' glyph set; add accented characters with
  `font_extra_glyphs`.

## Calendar row

`features/todoist_calendar.yaml` adds the next Home Assistant calendar event as
a row (purple, `CAL:`), sorted among the tasks. Home Assistant exposes only one
event (the next or current) as attributes, so it is one row. It is shown only
for events that start today (or already started), and not in the *Overdue* view.

| Substitution | Default |
|---|---|
| `calendar_entity` | `calendar.my_calendar` |
