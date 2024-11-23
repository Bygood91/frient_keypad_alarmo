
# Frient Keypad with Alarmo for Home Assistant

This project provides an automation to link a **Frient Keypad** to the **Alarmo** module in Home Assistant. It enables seamless communication between the keypad and Home Assistant’s Alarm Control Panel, offering advanced functionality for home security management.

## Features

- **Arm All Zones**: Activate all zones with the **Lock** button.  
- **Arm Day Zones**: Activate day zones with the **Home** button.  
- **Arm Night Zone**: Activate night zones with the **Moon** button.  
- **Disarm**: Deactivate the alarm with the **Unlock** button.  
- **Emergency Mode**: Trigger an emergency by long-pressing the **SOS** button.  
- **Entry Delay**: The keypad emits beeps and flashes a green light during the entry delay.  
- **Exit Delay**: The keypad emits beeps and flashes a red light during the exit delay.  
- **Invalid Code**: Flashes an orange light when an incorrect PIN or RFID tag is used.  
- **Alarm in Progress**: Flashes a red light when the alarm is active.  
- **RFID**: Support for multiple PIN codes and RFID tags.
- 
### RFID Tips:
- Use tools like [NFC Tools](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc&hl=en&gl=US) (on Android) to read your RFID tags. Simply copy the tag ID (preceded by `+`) into the list of allowed PIN codes.  

---

## License
This project is licensed under the MIT License. See the `LICENSE` file for details.  
