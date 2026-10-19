# Home Assistant side of broadcast and questions

## Receiving (features/broadcast.yaml)

The panel exposes a `text` entity, e.g. `text.my_clock_broadcast`. Write to it
from an automation or script:

```yaml
action: text.set_value
target:
  entity_id: text.my_clock_broadcast
data:
  value: "Dinner is ready"
```

The panel shows the text in a popup, beeps twice, and closes the popup after
60 seconds or on tap.

## Sending a question (features/broadcast_send.yaml)

The sending panel writes the question into the target's *question* entity and
reads the target's *answer* entity. The target can be another JadivClock or any
device you build yourself. You provide the glue in Home Assistant; for example
with three `input_text` helpers and a notification to a phone:

```yaml
automation:
  - alias: Panel question
    triggers:
      - trigger: state
        entity_id: input_text.kitchen_panel_question
    conditions:
      - condition: template
        value_template: "{{ trigger.to_state.state | length > 0 }}"
    actions:
      - action: input_text.set_value
        target: {entity_id: input_text.kitchen_panel_answer}
        data: {value: "waiting"}
      - action: notify.mobile_app_phone
        data:
          message: "{{ trigger.to_state.state }}"
          data:
            actions:
              - {action: PANEL_YES, title: "Yes"}
              - {action: PANEL_NO, title: "No"}
  - alias: Panel answer
    triggers:
      - trigger: event
        event_type: mobile_app_notification_action
        event_data:
          action: PANEL_YES
      - trigger: event
        event_type: mobile_app_notification_action
        event_data:
          action: PANEL_NO
    actions:
      - action: input_text.set_value
        target: {entity_id: input_text.kitchen_panel_answer}
        data: {value: "{{ 'Yes' if trigger.event.data.action == 'PANEL_YES' else 'No' }}"}
```

Contract with the sending panel:

1. When a question arrives, set the answer entity to `waiting` (this starts the
   60 second window on the sender).
2. Set the answer entity to the real answer when it is known. The sender shows
   it and beeps twice.

Then set `bc_target_*` in the sending panel's substitutions to those entities.
Because they are `input_text` helpers, also set `bc_question_domain: input_text`
(the default, `text`, is for `text` entities such as another JadivClock's
broadcast entity). Create the helpers with a maximum length of 120 characters,
the length of the on-screen composer:

```yaml
input_text:
  kitchen_panel_question: {name: Kitchen panel question, max: 120}
  kitchen_panel_answer: {name: Kitchen panel answer, max: 120}
```
