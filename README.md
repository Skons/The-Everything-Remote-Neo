# The Everything Remote - Neo

The Everything Remote Neo is an improved version of [The Everything Remote](https://github.com/TheStockPot/The-Everything-Remote) by The Stock Pot.

The original remote uses 21 of the available 23 GPIO pins, leaving very little room for additional features. The Neo uses a matrix keypad instead. Three GPIO pins are used for the rows and seven for the columns, requiring only ten GPIO pins in total.

This leaves room for additional features, including an RGB status LED. The LED provides feedback about the remote’s current state and shows which device context is active—for example, red for the TV, blue for the lights, and white for central heating.

## Features

The Neo includes all the features of the original remote, plus:

- RGB status LED
  - Deep-sleep status
  - Button-press feedback
  - Home Assistant connection status
  - Active device-context indication
- Adjustable LED brightness
  - Minimum brightness for wake-up feedback
  - Maximum brightness for blinking
- Faster wake-up from deep sleep
- Configurable deep-sleep timer
- Lower cost, because the PCB can be 3D-printed
- All buttons connected to column 1 can wake the remote from deep sleep
- Improved casing design
  - The casing is adapted for the more flexible printed PCB
  - The top no longer clicks into place, making maintenance and experimentation easier
  - Battery protection prevents the battery from being punctured
  - Additional space for battery wires
  - A transparent The Stock Pot logo improves LED visibility
- Experimental design in which all buttons can wake the remote from deep sleep  This has not been tested yet.

## How the context system works

A context represents the device or room currently controlled by the remote. The selected context is shown using the RGB LED.

For example:

- Red: TV
- Blue: lights
- Green: media player
- White: central heating
- Orange: another device or room

The context is managed through Home Assistant using an `input_select`. This makes it possible to switch between devices without needing a separate remote for each one.

## Materials

First, follow the original [Bill of Materials](https://github.com/TheStockPot/The-Everything-Remote) from The Stock Pot.

Skip the original PCB and add the following items:

- [0.5 mm copper wire](https://nl.aliexpress.com/item/1005009078359338.html)
- [RGB LED](https://nl.aliexpress.com/item/4000225253691.html)
- Three 200 Ω resistors
- 24–28 AWG wire, approximately four pieces shorter than 10 cm each

## Printing

**An AMS or equivalent multi-material setup is required.**

Print all plates included in the project. Pay special attention to the printed PCBs. If you have watched The Stock Pot video, then you'll see that his buttons are done different. For know I went the easy way for the icons.

For the holes to print correctly, you may need to set Bambu Studio’s [X-Y Hole Compensation](https://wiki.bambulab.com/en/software/bambu-studio/xy-hole-contour-compensation) to `0.15`. Do this for the Bottom and Top PCB.

The original Neo was printed using PETG because transparent PETG was available for the top parts and black PETG for the remaining parts. Transparent PLA should also work.

## Building the remote

Before starting, watch The Stock Pot’s YouTube build guide for the original remote. The general assembly process is the same, but the Neo uses a printed PCB, a matrix keypad and an RGB LED.

### Build order

1. Add the copper wires to the printed traces.
2. Solder the copper wires together where required.
3. Install all the push buttons.
4. Solder the push buttons to the traces.
5. Install the RGB LED.
6. Install the three resistors.
7. Solder the resistors to the LED leads.
8. Install the ESP32.
9. Solder the trace wires to the ESP32 GPIO pins.
10. Connect the LED wires to the resistors.
11. Solder the LED wires to the ESP32 GPIO pins.
12. Test the remote using the ESPHome configuration.
13. Install the assembled PCB into the casing.

Refer to the SVG file for the complete row, column, LED and ESP32 GPIO assignments.

## Adding the copper traces

After printing, start by adding the copper wire to the traces. First, [install two or three push buttons](#installing-and-soldering-the-push-buttons) without soldering into the PCB so that the two plates stay aligned while you work on them.

Start at the point where the wires will later be connected to the ESP32 on the top PCB. Unwind the copper wire from that point and follow the trace to the first hole. Push the wire through the hole, then press it into the trace from the beginning of the route. This method provides enough wire to fill the complete trace while minimizing excess wire that needs to be cut off.

Some traces consist of several connected sections. After completing the main trace, locate the additional sections and add wire to them. Solder these sections to the main trace where necessary.

Some ESP32 GPIO pins have two copper wires connected to them. Check the PCBEtcher SVG carefully before soldering.

### Working with the printed PCB

The printed PCB can be made from PLA or PETG. You can solder the wires while they are seated in the printed plate, but work quickly to avoid melting the plastic. Keep the soldering iron on the wire only as long as necessary. A small amount of melting is not an immediate problem, but do not puncture the PCB or damage the traces.

### Trace corners and push-button connections

Some traces turn sharply around corners. When a trace connects to a push-button leg, position the copper wire as close as possible to that leg. This makes soldering easier and produces a more reliable connection.

## Installing and soldering the push buttons

Push all buttons into the top PCB. The button legs can become caught between the two PCB plates. If necessary, straighten the legs with pliers before inserting the buttons. Once the buttons are in place, bend the legs slightly toward the PCB. This helps hold each button in position.

Make sure every button is fully seated and straight. A tilted button can cause alignment problems with the buttons when the remote is assembled.

Solder each button to the copper traces.

### Test the traces

Test all traces with a multimeter before installing the ESP32. If a connection is missed, it may be difficult to access it after the ESP32 has been installed. For each button:

1. Place one multimeter probe on the button leg.
2. Place the other probe on the corresponding row or column connection on the ESP32 side.
3. Confirm that the connection is continuous.
4. Repeat this for every leg on every button.

It can be useful to draw a table showing the rows and columns and mark each connection as it is tested. Use the SVG file to determine which row and column belong to each button.

## Installing the RGB LED and resistors

Insert the RGB LED through the PCB. Before soldering, identify the ground connection and the red, green and blue LED leads. The exact pin order depends on the LED type. Cut off the excess LED leads on the underside of the PCB. Insert the three 200 Ω resistors into their designated holes. Solder the resistor legs to the corresponding LED leads on the top side of the PCB, not the underside.

Refer to the PCBEtcher SVG for the LED wiring and GPIO assignment.

## Installing the ESP32

Insert the ESP32 through the bottom PCB. Make sure:

- The GPIO pins point in the correct direction.
- The USB connector aligns with the opening in the bottom casing.
- The ESP32 is seated properly before soldering.

Solder the copper traces to the assigned ESP32 GPIO pins. Make sure the wires are long enough to reach their connections without being stretched.

Connect the LED wires to the resistors and to the assigned GPIO pins. Route the wires between the buttons, not over them, so that they do not interfere with button operation.

## Testing

After completing the soldering:

1. Check all connections for continuity.
2. Check that no adjacent traces are shorted.
3. Confirm that the LED wiring is correct.
4. Flash the [ESPHome](#ESPHome) configuration.
5. Test every button.
6. Test wake-up from deep sleep.
7. Test the LED status and context colours.
8. Confirm that the remote connects to Home Assistant.

## Finishing the assembly

Install the completed PCB in the casing in the same way as the original remote.

Install the buttons and the top casing afterward. The Neo casing does not click into place. Press it into position carefully and make sure the rear of the casing is correctly aligned.

## ESPHome

Add [`everything-remote-neo.yaml`](everything-remote-neo.yaml) to ESPHome and configure it for your device.

Check the following before flashing:

- The GPIO assignments match the printed PCB and SVG.
- The LED pins are configured correctly.
- The deep-sleep timer is suitable for your use.
- The Home Assistant API and Wi-Fi settings are correct.

Flash the ESP32 as usual and verify that the remote wakes up and responds to button presses.

## Home Assistant

Add the following `input_select` to Home Assistant:

```yaml
input_select:
  everything_remote_neo_context:
    name: Everything Remote Neo Context
    icon: mdi:palette
    options:
      - Red
      - Blue
      - Green
      - White
      - Orange
    initial: Red
```

If an `input_select:` section already exists in your configuration, add only the `everything_remote_neo_context` item below it.

After adding the helper, copy the contents of [`automations.yaml`](automations.yaml) into an automation. You can either add the automation directly to `automations.yaml`, or create a new automation through the Home Assistant interface and select **Edit in YAML**.

The example automation is designed to make switching between contexts easy. Pay special attention to:

- The `setcontext` trigger
- The `setcontext` action
- The **Select context** option in the long-press action

The colours in the automation variables must match the options in `everything_remote_neo_context`. If you want to use different colours, update both the variables and the `input_select` options.

## Images

[<img src="images/The Everything Remote - Neo PCB Top.jpeg" alt="The Everything Remote Neo PCB top" width="500">](images/The%20Everything%20Remote%20-%20Neo%20PCB%20Top.jpeg)
[<img src="images/The Everything Remote - Neo PCB Top Full.jpeg" alt="The Everything Remote Neo PCB top, full view" width="500">](images/The%20Everything%20Remote%20-%20Neo%20PCB%20Top%20Full.jpeg)
[<img src="images/The Everything Remote - Neo PCB Bottom.jpeg" alt="The Everything Remote Neo PCB bottom" width="500">](images/The%20Everything%20Remote%20-%20Neo%20PCB%20Bottom.jpeg)

[<img src="The Everything Remote - Neo PCBEtcher.svg" alt="The Everything Remote Neo SVG PCBetcher layout">](The%20Everything%20Remote%20-%20Neo%20PCBEtcher.svg)

[<img src="images/The Everything Remote - Neo.svg" alt="The Everything Remote Neo button layout">](images/The%20Everything%20Remote%20-%20Neo.svg)

## Experimental features

### Wake-up using all buttons

The `PCBEtcher` file contains additional layers for experimental wake-up traces:

[Open the PCBEtcher file](The%20Everything%20Remote%20-%20Neo%20PCBEtcher.svg)

These traces have not been printed or tested yet. The idea is to connect the column lines to a dedicated wake-up GPIO through diodes. To use GPIO36 as the wake-up input, add one diode to each wake-up trace. The diodes must be oriented with the cathode—the side marked with a stripe—towards GPIO36.

Another possible design is to connect a diode directly to each column GPIO and combine the diode outputs on GPIO36. This feature is experimental and has not been validated. Check the diode orientation and the ESP32 wake-up requirements before connecting the battery or flashing the ESPHome configuration.

### Battery voltage measurement

GPIO35 can be used as an ADC input to measure the battery voltage. However, not every ESP32 Wemos Lolin Lite board has this input wired in the same way. Verify the board schematic before relying on this feature.

A voltage divider can be added using two 100 kΩ resistors, in this case you will not have to rely on the schematic of your board:

```text
3.3v ── 100 kΩ ──┬── VP / GPIO35
                 │
               100 kΩ
                 │
                GND
```

Add this to you ESPHome config in case you have used your own voltage divider.

```yaml
sensor:
  - platform: adc
    pin: GPIO36
    name: "Battery Voltage"
    id: battery_voltage
    attenuation: 11db
    update_interval: 60s
    filters:
      - multiply: 1.941
      - median:
          window_size: 5
          send_every: 5
          send_first_at: 1
 
  - platform: template
    name: "Battery Percent"
    unit_of_measurement: "%"
    device_class: battery
    state_class: measurement
    update_interval: 60s
    lambda: |-
      float v = id(battery_voltage).state;
      float pct = (v - 3.0) / (4.2 - 3.0) * 100.0;
      if (pct > 100.0) pct = 100.0;
      if (pct < 0.0)   pct = 0.0;
      return pct;
```

[Measuring battery voltage and capacity ESP32 Lite V1.0.0](https://community.home-assistant.io/t/measuring-battery-voltage-and-capacity-esp32-lite-v1-0-0/670376/)

## Note

It was a complex process developing a tool and "improving" The Everything Remote. I hope I have documented everything. If something is missing, please let me know.

## Credits

This project is based on [The Everything Remote](https://github.com/TheStockPot/The-Everything-Remote) by The Stock Pot.
