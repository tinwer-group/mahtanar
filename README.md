# Mahtanar
### A hardware controller for Mitsubishi heat pumps using CN105 connector.

<img src="https://github.com/user-attachments/assets/6ce37ea4-a2a8-421e-a000-fe889be8c505" height="180"/>
<img src="https://github.com/user-attachments/assets/b1f2440f-674b-4b0e-aa93-91b00f111d58" height="180"/>

Available for purchase at the [Tinwer Etsy Shop](https://www.etsy.com/listing/1762258422/mahtanar-heat-pump-controller)

# ⚠️

❗This project is **not** affiliated in any way with Mitsubishi.  The hardware and software provided here are as-is, with all faults; and include no warranties express or implied.  Following the procedures herein may void the warranty of your equipment and pose a risk of injury; proceed at your own risk!❗

... that said we're doing our best to make sure things work and to help you through the process.  Please feel free to create an issue here if you run into any trouble.

----

## Getting Started

While *in theory* it's possible to connect the Mahtanar board to your heat pump equipment and *then* provision it, this can make troubleshooting more difficult so it's recommended that at least for your first device you provision it while it's connected/powered over USB and connect it to your heat pump after it's all set up.

#### Prerequisites:

- Mahtanar board (WiFi or Ethernet model)
- CN105 connector cable
- ESPHome installed and running on your network
- Home Assistant configured with ESPHome integration

#### Setup

1. Connect the Mahtanar to a USB power source (ideally your computer for additional serial troubleshooting if needed).
2. If you have the WiFi version of the board, use your computer or mobile device to connect to the `mITP Setup` WiFI SSID with `mahtanar` as the password.  You should be prompted to connect the device to your WiFi network; do so.
If you have the ethernet version of the board, just connect it to your network.
3. After a moment, the device should show up on your ESPHome dashboard as `mitp-heat-pump-MACSUFFIX` with an "Adopt" option.  Press "Adopt", provide a friendly name, and copy the encryption key for later.  When prompted, press "Install".
4. Home Assistant should have discovered the device and will show it with the friendly name you provided.  Adding this device to Home Assistant will require the encryption key you copied in step 3.
5. Now that the device is provisioned, you can connect it to your heat pump using the CN105 connector cable and the port labeled `HP1` on the Mahtanar device.  Because power is provided by the equipment, a USB connection is no longer required unless troubleshooting.  This process can vary a bit depending on equipment, and more detailed information is provided below.
6. More information on configuring the ESPHome `mitsubishi_itp` component is [available here](https://muart-group.github.io/).  The ESPHome configuration should be available for modification on your ESPHome dashboard.

#### Troubleshooting

Frequent troubleshooting tips will be added here, but a common step will likely be to use the configuration files listed below to reset the device back to its factory defaults (or as a starting point for a manual configuration) from the ESPHome dashboard:

- [WiFi v1.1](https://raw.githubusercontent.com/tinwer-group/mahtanar/1.1-release/esphome-configs/mahtanar-wifi-default.yaml)
- [Ethernet v1.1](https://raw.githubusercontent.com/tinwer-group/mahtanar/1.1-release/esphome-configs/mahtanar-ethernet-default.yaml)

## Connecting to Equipment

More details to be added here, though some detailed tutorials can be found online.  The gist of the process is that **WITH THE POWER DISCONNECTED FROM YOUR EQUIPMENT** (breaker off, or lockout switch engaged, depending), you will take the cover off of the wall unit, or panel off the air handler and locate the CN105 connector on the controller board.  The connector is usually red.  There should be some sort of knockout on the cover/panel near the controller board to route the cable outside of the unit.  The cover/panel can then be reinstalled and **THEN** the power turned back on.  You can then connect the other end of the cable to the Mahtanar.

#### MHK2

The `mitsubishi_itp` component also supports MHK2 thermostats which can be connected to the other port (labeled `TS1`) on the Mahtanar.  Once again, configuration details are available from [this project](https://muart-group.github.io/).
