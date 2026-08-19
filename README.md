# FunkeeB - Corne V4 ProMicro Edition

ZMK config and relevant files for my custom built Corne v4 Promicro Edition by klouderone.

This board is based on Corne v4 but uses ProMicro controller (nice!nano v2 clone) for more connection stability and ease of buiding.
Compatible with both Choc and MX swithes!
It features 3x6 column staggered keys and 3 key thumb cluster. 
Compatible with ZMK Studio. 


#### Components

-   `MCU`: nice!nano v2 _compatible board_ (wireless)
-   `PCB`: Corne v4 ProMicro Edition [cornev4 promicro](https://github.com/klouderone/cornev4promicroedition) designs
-   `Case`: Custom Stainless steel 316l case for a Choc version and Nylon PA12S-HP case for the MX version. I highly recommend using JLC3DP for printing those cases!
-   `Batteries`: 110mah Lipo
-   `Sockets`: Hotswap sockets
-   `Diodes`: 1N4148W SMD Diode SOD-123
-   `Switches`: MX or Choc v1 / v2

#### Software

Flash MCU with a software generated in the actions section of Github. Later you can use ZMK Studio. 

#### Keymap:


## Images

|          Corne v4 MX          |            Photos             |
| :---------------------------: | :---------------------------: |
| ![Photo 1](assets/PXL_20260611_052722855.PORTRAIT.jpg) | ![Photo 2](assets/PXL_20260611_172306024.MP.jpg) |
| ![Photo 3](assets/PXL_20260611_171750737.MP.jpg) | ![Photo 4](assets/PXL_20260611_172642045.MP.jpg) |

---

RP2040 conversion notes (branch: rp2040-conversion)

- Goal: convert firmware/device-tree overlays from ProMicro (nRF-based nice!nano) to a RP2040-based Pro Micro clone mapping. Only firmware/config files changed; KiCad and PCB files unchanged.

- Files changed in this branch:
  - config/boards/shields/corne/corne.dtsi
    - Commented original ProMicro/nRF-specific nodes (row/col pinctrl, NRF_PSEL macros, spi3 node).
    - Mapped key matrix rows to &rp2040 GPIOs 4,5,6,7 (original rows used same numeric pins on ProMicro).
    - Added an example /i2c0 node with ssd1306@3c enabled; original &pro_micro_i2c node is commented for reference.
    - Left comments for WS2812/under-glow: original Nordic SPI-based ws2812 node is commented; the RP2040 typically needs PIO or an RP2040-specific driver — review before enabling underglow.

  - config/boards/shields/corne/corne_left.overlay
  - config/boards/shields/corne/corne_right.overlay
    - Both overlays: original col-gpios referencing &pro_micro are commented and replaced with col-gpios referencing &rp2040 with the same numeric GPIO pins (14,15,18,19,20,21). The old lines are retained as comments and marked "do not delete".

  - README.md
    - Added this conversion notes section documenting the changes and next steps.

Notes & next steps:
- This change assumes a "standard" RP2040 Pro Micro clone where GPIO numbers 0..29 correspond to the pins you will wire in the PCB. Verify your board's silk/schematic pin numbers match these GPIO indices before flashing.
- Update i2c SDA/SCL pin numbers in corne.dtsi if your RP2040 board uses different pins for I2C.
- Update LED/WS2812 node to an RP2040-compatible driver (PIO or dedicated ws2812 support) if you use per-key RGB/under-glow.
- I did not delete any original ProMicro/nRF lines — they are commented for traceability.

If this looks good I will continue with any additional overlays or configs you want converted, and I can open a PR with these changes on branch rp2040-conversion.
