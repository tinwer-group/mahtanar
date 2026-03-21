# Mahtanar
### A hardware controller for Mitsubishi heat pumps using CN105 connector.
<img src="https://github.com/user-attachments/assets/01e2992e-c7b9-4d10-be85-d5b3c83018b2" height="180"/>
<img src="https://github.com/user-attachments/assets/86458336-06fa-4938-9e9f-9ac6550a7d26" height="180"/>


Available for purchase at the [Tinwer Etsy Shop](https://tinwer.etsy.com/listing/1762258422/mahtanar-heat-pump-controller)

# ⚠️

❗This project is **not** affiliated in any way with Mitsubishi.  The hardware and software provided here are as-is, with all faults; and include no warranties express or implied.  Following the procedures herein may void the warranty of your equipment and pose a risk of injury; proceed at your own risk!❗

... that said we're doing our best to make sure things work and to help you through the process.  Please feel free to create an issue here if you run into any trouble.

----

## Getting Started

While *in theory* it's possible to connect the Mahtanar board to your heat pump equipment and *then* provision it, this can make troubleshooting more difficult so it's recommended that at least for your first device you provision it while it's connected/powered over USB and connect it to your heat pump after it's all set up.

### Prerequisites:

- Mahtanar board (WiFi or Ethernet model)
- CN105 connector cable
- (Semi-optional, see [note](#setup-note)) ESPHome installed and running on your network
- Home Assistant configured with ESPHome integration

#### Setup Note
ℹ️ The Mahtanar board comes pre-flashed with a default firmware, and can by used in Home Assistant without any ESPHome interaction. If you're fine with the defaults, you don't need to "Take Control" using ESPHome (and can always do so later if needed).

### Setup

1. Connect the Mahtanar to a USB power source (ideally your computer for additional serial troubleshooting if needed).
2. If you have the WiFi version of the board, use your computer or mobile device to connect to the `mITP Setup` WiFi SSID with `mahtanar` as the password.  You should be prompted to connect the device to your WiFi network; do so.  If your device doesn't prompt you, open a browser to http://192.168.4.1 to connect to the ESPHome Wifi setup page.
If you have the ethernet version of the board, just connect it to your network.
3. (Semi-optional, see [note](#setup-note)) After a moment, the device should show up on your ESPHome dashboard as `mitp-heat-pump-MACSUFFIX` with a "Take Control" option.  Press "Take Control", and provide a friendly name.  When prompted, press "Install".
4. Home Assistant should have discovered the device and will show it with the friendly name you provided.
5. Now that the device is provisioned, you can connect it to your heat pump using the CN105 connector cable and the port labeled `Heatpump` (or `HP1`) on the Mahtanar device.  Because power is provided by the equipment, a USB connection is no longer required unless troubleshooting.  This process can vary a bit depending on equipment, and more detailed information is provided below.
6. More information on configuring the ESPHome `mitsubishi_itp` component is [available here](https://muart-group.github.io/).  The ESPHome configuration should be available for modification on your ESPHome dashboard.



### Troubleshooting

Frequent troubleshooting tips will be added here, but a common step will likely be to use the configuration files listed below to reset the device back to its factory defaults (or as a starting point for a manual configuration) from the ESPHome dashboard:

1. Select the branch in this repository that matches the revision number printed on your PCB.
2. Copy the contents of either the `wifi` or `ethernet` yaml files into the config on your ESPHome dashboard, and press "Install".

## Connecting to Equipment

More details to be added here, though some detailed tutorials can be found online.  The gist of the process is that **WITH THE POWER DISCONNECTED FROM YOUR EQUIPMENT** (breaker off, or lockout switch engaged, depending), you will take the cover off of the wall unit, or panel off the air handler and locate the CN105 connector on the controller board.  The connector is usually red.  There should be some sort of knockout on the cover/panel near the controller board to route the cable outside of the unit.  The cover/panel can then be reinstalled and **THEN** the power turned back on.  You can then connect the other end of the cable to the Mahtanar.

For additional information about locating and connecting to the CN105 port, you may want to refer to the Mistubishi documentation for your equipment which will have instructions for installing e.g. the MHK2 RedLink adapter which is plugged into the same CN105 port.

### MHK2

The `mitsubishi_itp` component also supports MHK2 thermostats which can be connected to the other port (labeled `Thermostat` or `TS1`) on the Mahtanar.  Once again, configuration details are available from [this project](https://muart-group.github.io/).

## FAQ

<details>
  <summary>What if I don't have an MHK2?</summary>
  The board will still work fine without an MHK2; just connect only the `HP1` port to your equipment and leave the `TS1` port empty.

  If you don't plan on using a thermostat, the thermostat features can be disabled in the configuration streamline your experience.

  Comment or remove `uart_thermostat` to disable the feature:
  ``` yaml
  climate:
    - platform: mitsubishi_itp
      name: "Climate"
      uart_heatpump: hp_uart
      # uart_thermostat: tstat_uart # This and the uart component below can be removed if no thermostat connected
  ```

  Additionally, you may want to remove the `uart` component to free up pins or allow for USB logging:
  ``` yaml
  uart:
    - id: hp_uart
      baud_rate: 2400 # For some equipment this may need to be 9600
      parity: EVEN
      rx_pin:
        number: GPIO1
      tx_pin:
        number: GPIO0
    # - id: tstat_uart
    #   baud_rate: 2400 # For some equipment this may need to be 9600
    #   parity: EVEN
    #   rx_pin:
    #     number: GPIO20
    #   tx_pin:
    #     number: GPIO21
  ```

  On older configurations where logging had been disabled, you can reenable it if desired:
  ```yaml
  logger:
    # baud_rate: 0
    baud_rate: 115200
  ```
</details>

<details>
  <summary>How do I re-flash the board?</summary>
  In this repository, choose the branch that matched the revision number printed on your PCB (e.g. `v1.3`). Copy either the Ethernet or WiFi configuration from this repository into your ESPHome dashboard and install. If you have previously configured the WiFi, in most circumstances you will not need to reconfigure it.
</details>

## Additional Config
<details>
  <summary>Status LED (v1.2 ONLY)</summary>
  The built-in LED can be used to show current system status by adding the configuration snippets below.  The light can be turned on or off in Home Assistant and will remember its state, but brightness will need to be adjusted in the configuration.

  Add the `light` and a `script` to update it:
  ```yaml
  light:
    - platform: neopixelbus
      variant: ws2812x
      pin: GPIO08
      num_leds: 1
      id: status_led
      name: "Status LED"
      effects: 
        - pulse:
            name: "Standby"
            min_brightness: 40%
            max_brightness: 60%
            transition_length: 1s
            update_interval: 1s
      restore_mode: RESTORE_DEFAULT_ON

  script:
    - id: update_status_led
      parameters:
        mode: int
        action: int
      then:
        - lambda: |-
            auto call = id(status_led).make_call();
            switch (mode) {
              case CLIMATE_MODE_OFF:
                call.set_rgb(1,1,1);
                break;
              case CLIMATE_MODE_HEAT_COOL:
                call.set_rgb(.8,0,1);
                break;
              case CLIMATE_MODE_COOL:
                call.set_rgb(0,0.2,1);
                break;
              case CLIMATE_MODE_HEAT:
                call.set_rgb(1,0.2,0);
                break;
              case CLIMATE_MODE_FAN_ONLY:
                call.set_rgb(0,1,1);
                break;
              case CLIMATE_MODE_DRY:
                call.set_rgb(1,1,0);
                break;
              default:
                call.set_rgb(0,1,0);
            }
  
            if (action == CLIMATE_ACTION_IDLE) {
              call.set_effect("Standby");
            } else {
              call.set_effect("none").set_brightness(0.6);
            }
  
            call.perform();
  ```

  Add an `on_boot` to show that the code is running, but heat pump is not-yet connected:
  ```yaml
  esphome:
    ...
    on_boot:
      - then:
          - lambda: |-
              id(status_led).make_call().set_rgb(1,1,1).set_effect("Standby").perform();
  ```

  Add an `on_state` to the climate component to call the script above:
  ```yaml
  climate:
  - platform: mitsubishi_itp
    ...
    on_state:
    then:
      - lambda: id(update_status_led)->execute(x.mode, x.action);
  ```
</details>

<details>
  <summary>Status LED (v1.4 ONLY)</summary>
  On revision v1.4, there are two LEDs: one to indicate the board has 3.3v power, and the other programmable. By default the programmable LED will use the ESPHome [Status LED](https://esphome.io/components/status_led/) platform to provide basic status:

  ```yaml
  light:
  - platform: status_led
    name: "Status LED"
    pin:
      number: GPIO08
      inverted: true
      ignore_strapping_warning: true
    internal: true
  ```

  The programmable LED can be disabled by commenting out the configuration above, or used for other purposes (e.g. by removing `internal: true` the LED will appear as a light in Home Assistant).
</details>

## Hardware Revisions

See [PCB REVISIONS](PCB-REVISIONS.md).
