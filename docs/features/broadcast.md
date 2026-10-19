# Broadcast and questions

* `features/broadcast.yaml` - **receive**: a `text` entity on the panel; writing
  to it shows a popup and beeps (see [home-assistant.md](../home-assistant.md)).
* `features/broadcast_send.yaml` - **send**: *Send message* tile, an on-screen
  keyboard, and either a broadcast to another panel or a question with a
  60 second wait for the answer.

| Substitution (send) | Default |
|---|---|
| `bc_target_name` | `Remote panel` |
| `bc_target_broadcast_entity` | `text.remote_panel_broadcast` |
| `bc_target_question_entity` | `text.remote_panel_question` |
| `bc_target_answer_entity` | `text.remote_panel_question_answer` |

The keyboard is ASCII only.
