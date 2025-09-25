# Home Assistant – Fan & Switch Synchronization Guide

## Introduction

Welcome to a project for synchronizing ceiling fans, wall switches, and lights using Home Assistant.

This guide covers the setup, naming conventions, file placement, and how to activate the automations.

---

## System Components

This setup supports synchronization of multiple ceiling fans and switches.  
The examples in this guide use only **one fan and one switch**, but you can easily expand the configuration by adding more pairs in the mapping sections inside each automation file.

### 1. SONOFF iFan04 Ceiling Fan with Tasmota

- A ceiling fan controlled by SONOFF iFan04 module.
- Flashed with **Tasmota** firmware for flexible control via Home Assistant.
- **Tasmota flashing guide for iFan04:**  
  [Tasmota iFan04 Flash Guide](https://tasmota.github.io/docs/devices/sonoff_ifan04/)

### 2. SONOFF ZBM5 120 Dual Switch (Zigbee)

- Dual wall switch by SONOFF, model ZBM5 120.
- Requires a Zigbee hub to communicate with Home Assistant.

---

## Entity Names

| Device Type         | Example Entity ID                       |
|---------------------|-----------------------------------------|
| Ceiling Fan         | `fan.bedroom_fan`                        |
| Fan Switch          | `switch.bedroom_switch_fan`              |
| Light Switch        | `switch.bedroom_switch_light`            |
| Light               | `light.bedroom_light`                    |

\* **Note:** The automations support multiple fans and switches.  
To add more, extend the mappings in the YAML files accordingly.

---

## File Placement & Activating Automations

1. **Save the Files**  
   Place the following YAML files in the `automations` folder inside your Home Assistant config directory (usually `/config/automations/`):

   - `sync_fan_to_switch_rooms.yaml`
   - `sync_light_from_switch.yaml`
   - `sync_switch_to_fan.yaml`
   - `sync_switch_from_light.yaml`

2. **Load Automations in Home Assistant**

   Two common ways to include the automations:

   - **A. Append contents to `automations.yaml`:**  
     If you're using a single `automations.yaml` file, copy and paste the contents of each automation YAML file into it.

   - **B. Use `!include_dir_merge_list`:**  
     To keep your automations modular and organized, add the following line to your `configuration.yaml` file:

     ```yaml
     automation: !include_dir_merge_list automations/
     ```

     This tells Home Assistant to automatically load all YAML files inside the `automations` folder.

3. **Activate Automations**

   After saving the files, go to Home Assistant:  
   **Settings → Automations & Scenes → ⋮ Menu → Reload Automations**  
   Alternatively, restart Home Assistant to apply all changes.

---

## Expanding Mappings – Add More Fans or Switches

To add additional fans or switches, simply expand the `variables` section in the relevant automation files.  
For example:

```yaml
variables:
  switch_map:
    fan.bedroom_fan: switch.bedroom_switch_fan
    # fan.kidsroom_fan: switch.kidsroom_switch_fan
    # fan.playroom_fan: switch.playroom_switch_fan

  light_map:
    switch.bedroom_switch_light: light.bedroom_light
    # switch.kidsroom_switch_light: light.kidsroom_light
    # switch.playroom_switch_light: light.playroom_light
```

Uncomment or add more entries as needed.

---

## Electrical Wiring Notes

- The **fan is connected directly to power**, so it always remains online and controllable.
- The **switch is also directly powered**, and only sends control signals without cutting physical power.
- On/Off operations happen **at the device level**, not by cutting electricity.

---

## ⚠️ Safety Warning

The author assumes **no responsibility** for hardware or software damage.  
All actions are performed **at your own risk**.

- Electrical work must be performed **by a certified electrician only**.  
- Always back up your Home Assistant configuration before making changes.

---

## Summary

In this guide, we covered how to synchronize ceiling fans and wall switches using Home Assistant, how to structure automation files, and how to expand the setup for additional devices.

💬 **Feedback is welcome!**  
If you have suggestions to improve the guide or the YAML files, feel free to open an issue or submit a pull request.
