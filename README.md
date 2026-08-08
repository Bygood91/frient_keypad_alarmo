
# Frient Keypad with Alarmo for Home Assistant

This project provides automation to link a **Frient Keypad** to the **Alarmo** module (or any alarm control panel) in Home Assistant. It enables seamless communication between the keypad and Home Assistant’s Alarm Control Panel, offering advanced functionality for home security management.

Two blueprints are provided:

- `frient_keypad_with_alarmo.yaml` — **Zigbee2MQTT** version (original, by Bygood91)
- `frient_keypad_with_alarmo_zha.yaml` — **ZHA** (Zigbee Home Automation) version, converted from the Z2M one

## Features

- **Arm All Zones**: Activate all zones with the **Lock** button.
- **Arm Day Zones**: Activate day zones with the **Home** button.
- **Arm Night Zone**: Activate night zones with the **Moon** button.
- **Disarm**: Deactivate the alarm with the **Unlock** button.
- **Emergency Mode**: Trigger an action when the **SOS** button is pressed (Z2M: via `action: emergency`; ZHA: via the keypad panel transitioning to `triggered`).
- **Entry Delay**: The keypad emits beeps and flashes a green light during the entry delay.
- **Exit Delay**: The keypad emits beeps and flashes a red light during the exit delay.
- **Invalid Code**: Flashes an orange light when an incorrect PIN or RFID tag is used.
- **Alarm in Progress**: Flashes a red light when the alarm is active.
- **RFID**: Support for multiple PIN codes and RFID tags.
- **Custom actions**: Up to 3 special PIN codes can run arbitrary actions.

### RFID Tips:
- Use tools like [NFC Tools](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc&hl=en&gl=US) (on Android) to read your RFID tags. Simply copy the tag ID (preceded by `+`) into the list of allowed PIN codes.

---

## ZHA blueprint — setup

The ZHA version requires the frient Intelligent Keypad (KEPZB-110) paired with the
[Zigbee Home Automation](https://www.home-assistant.io/integrations/zha/) integration.

1. **Pair the keypad with ZHA** (Devices & Services → Zigbee Home Automation → Configure → Add Device).
2. **Set a Master Code** in the ZHA integration: *Configure → Alarm Control Panel Options → Master Code*.
   Use a long, random "system" code that is **distinct** from your user PIN codes, and keep it in the
   `zha_keypad_code` field of the blueprint. This code lets Home Assistant command the keypad's display.
3. **Find the keypad's alarm control panel entity** — ZHA creates one automatically
   (e.g. `alarm_control_panel.frient_alarm_keypad_alarm_control_panel`).
4. **Import the blueprint** and fill in:
   - `zha_keypad_device` → your frient keypad device
   - `zha_keypad_panel` → the keypad's alarm control panel entity from step 3
   - `zha_keypad_code` → the Master Code from step 2
   - `control_panel` → your Alarmo / manual alarm control panel
   - `pin_list` → valid PINs and `+RFID` tags, separated by `;`
5. Create the same PIN codes in Alarmo (Alarmo also supports RFID tags).

### ZHA differences / limitations (vs. the Z2M blueprint)

| Z2M | ZHA |
|---|---|
| MQTT trigger on `zigbee2mqtt/<keypad>` | `zha_event` (IAS ACE `arm` command), filtered by device |
| Code/mode in `trigger.payload_json.action*` | `trigger.event.data.params` (older ZHA: `args`) — the blueprint reads both |
| Publish `{"arm_mode": {...}}` to `/set` | Call `alarm_control_panel.alarm_*` on the keypad's own ZHA panel entity |
| Invalid-code feedback via `arm_mode: invalid_code` | ZHA replies natively with `Invalid_Arm_Disarm_Code` when the code ≠ Master Code |
| `exit_delay` / `entry_delay` display | Not natively supported by ZHA; the blueprint lights the keypad's triggered state instead (`alarm_trigger`). Entry/exit delay beeps/colors are a known ZHA limitation. |
| SOS via `action: emergency` | No `zha_event` is fired for SOS in ZHA; detected via the keypad panel → `triggered`, guarded against the blueprint's own sync |

> Note: a code that matches the ZHA **Master Code** but is not in the blueprint's `pin_list` will be
> accepted by the keypad itself but ignored by the blueprint (no arm/disarm). Keep the Master Code
> separate from user codes to avoid this.

---

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.  
