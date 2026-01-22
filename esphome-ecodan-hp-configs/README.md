## Config for esphome-ecodan-hp

This is a board config for the [esphome-ecodan-hp](https://github.com/gekkekoe/esphome-ecodan-hp) component.

ℹ️ This has only been lightly tested so there may be some additional troubleshooting required.

### Instructions

1. In the configuration file that you use following the [build from source](https://github.com/gekkekoe/esphome-ecodan-hp/blob/main/docs/build-from-source.md) instructions in the `esphome-ecodan-hp` project, comment out or remove the line that has the `confs/esp32s3.yaml` file.
2. Add the following yaml to the `packages` section following the existing `remote_package` declaration:

```yaml
  remote_package:
    url: https://github.com/tinwer-group/mahtanar
    ref: 1.4-release
    refresh: always
    files: [ 
            esphome-ecodan-hp-configs/esp32c3-mahtanar.yaml, # Config for Mahtanar 1.4
           ]
```
3. Continue with the remainder of the instructions from the `esphome-ecodan-hp` project.

### Notes
- Essentially the config file included here is a copy of the esp32c3 config from the original project, but with the UART and status LED pins remapped to match the hardware layout of the Mahtanar 1.4 board.
- This could be adapted for older revisions of the Mahtanar board by changing the pins appropriately.
- A detailed writeup of one implementation of this is available [here from Stephane](https://gitlab.raidho.fr/Stephane/heat-pump). A big thanks to them for testing this as well.