## MahtanarM Hardware Revisions

### v1.1
<img src="https://github.com/user-attachments/assets/6ce37ea4-a2a8-421e-a000-fe889be8c505" height="180"/>
<img src="https://github.com/user-attachments/assets/b1f2440f-674b-4b0e-aa93-91b00f111d58" height="180"/>

- First "mass"-produced board.
- Base PCB manufactured, but all components soldered by hand.

### v1.2
<img src="https://github.com/user-attachments/assets/df8850b4-97a7-4426-9edd-21ad0dd7dc35" height="180"/>

- Included level-shifter and connectors in PCB design and had them assembled by manufacturer (other components still hand-soldered).
- Removed LED footprint.

### v1.3
<img src="https://github.com/user-attachments/assets/81a0a335-06e7-4f35-9afa-6b216d1c0c86" height="180"/>

- Switched from ESP32-C3 devboard to custom circuit using ESP32-C3-WROOM-02!
- Switched from white to blue because of fine tolerances for solder mask near USB-C port.
- Issue: Capacitor for debouncing `BOOT` button prevented normal boot on first power up. Pressing `RESET` would work; solution was to remove capacitor (as shown in photo).
- Issue: Learned that WROOM module includes 499Ω resistor in series with UART TX pin preventing this pin from reaching 0v (only got to 0.6v). This was fine for test board but caused issues on actual Mitsubishi equipment. Solution was to remove the pullup resistors on that line (as shown in photo) to achieve 0.25v which was acceptable for Mitsubishi equipment.

### v1.4
<img src="https://github.com/user-attachments/assets/90636464-b2cf-43bf-bb59-67b60907f519" height="180"/>

- Switched to hex buffer instead of bi-direcitonal levelshifter to work around issue with 499Ω resistor.
- Added power and status LEDs.
- Issue: Forgot to update version number on PCB silkscreen.
- Issue: Learned that while wall-units have 5v pullup resistors on their CN105 ports, that some (most?) air handlers do not. This board design only included pullups for the Mahtanar->HP lines, so it has issues receiving messages from air handlers. Solution is to manually install a 5v pullup resistor on the HP->Mahtanar line.

### v1.4b
- Added pullups to the HP->Mahtanar, and Thermostat->Mahtanar lines.
- Fixed silkscreen to actually say `v1.4b`
<img src="https://github.com/user-attachments/assets/e18f191f-88c1-42a3-9810-bfab0cf08eda" height="180"/>
<img src="https://github.com/user-attachments/assets/e74af5bd-6042-4602-8a9c-39e734ce17b1" height="180"/>

### v1.4c (unreleased)
- Attempted to add a switch to disable the status LEDs for dark environments. Ended up being fairly costly for an infrequent request with a simple solution (desoldering the LEDs)

### v1.4d (unreleased)
- Modified BOM to source parts in Europe instead of Asia to work around massive, unpredictable tariffs. Increased production costs more or less offset the tariffs so I stuck with existing supplier.

### v1.4e
- Modified BOM to use surface-mount PA connectors instead of through-hole to streamline production.
- Modified BOM to use more available button switches (previous model was having availability issues).
<img src="https://github.com/user-attachments/assets/01e2992e-c7b9-4d10-be85-d5b3c83018b2" height="180"/>
<img src="https://github.com/user-attachments/assets/86458336-06fa-4938-9e9f-9ac6550a7d26" height="180"/>
