# 360-ATX-Power-Board
ATX to Xbox power adapter board for (pre-Slim) Xbox 360

![360_atx_power_board](https://github.com/user-attachments/assets/663e8637-58c1-4229-a87a-30c058479aa1)

# Rationale
Powering an Xbox 360 without using the original antique brick, instead using a modern, quiet, smaller, efficient ATX power supply. Also doing the same without just having components and wire connections dangling in air or stuck on breadboard.

# Compatibility
Xbox 360 models Xenon, Zephyr, Opus, Falcon, Jasper, i.e. anything with the large six-sided power connector. I tested this with a Xenon. It theoretically works with the Slim (2 pin barrel jack) and E (1 pin barrel jack) models but I don't have that hardware to test. The most power-hungry model (Xenon) came with a 203W power adapter, so a 300W or higher ATX PSU should do things well without brushing up against current maxima.

# Prerequisites
- An ATX power supply capable of supplying at least 203W over 12V
  - If you know it'll be used only with newer, more efficient models then you can reduce the power budget accordingly
- The power cable from an Xbox 360 power brick or compatible

# Instructions
1. Desolder or otherwise harvest the device-side power cable (i.e. not the wall end) from an Xbox 360 power brick or compatible; I think you can get the connector itself on Aliexpress if you don't have access to a brick at all and fabricate your own cable with thick enough wiring to handle the current
2. Assemble PCB
3. Connect your power supply's 24 pin ATX connector and at least one other connector (preferably all) to the PCB; the peak power draw is 203W at 12V and the 24 pin connector is only rated to deliver about 140W over 12V, the more connectors are plugged in the better distributed the load is
4. Connect the Xbox 360 power cable to screw terminal J2; wire colors should match markings on PCB silkscreen
   -   If you harvested a cable from a power brick, consider soldering appropriate gauge short extensions to each connector with solder seal connectors, and then crimping ferrules on the other end of the extensions so that it connects better to the screw terminals
5. Connect the other end of the Xbox 360 power cable to your console
6. Turn on your power supply; +5VSB LED (if installed) should light up
7. Set switch SW1 to on
8. Turn on Xbox 360; all LEDs (if installed) should light up

# Building the PCB
Import project files into KiCad 8 and export to your PCB manufacturing service of choice, or just use the files in the grbl subfolder.

# BOM
- Required
  - Connectors
    - Input
      - J1: 24-pin ATX connector, Molex 39-30-1240 or compatible
      - J3: 4-pin "Molex" peripheral connector, TE 641737-1 or compatible
      - J4: 8-pin EPS12V connector, Molex 39-30-1080 or compatible
      - J5: 8-pin PCIe connector, Molex 45586-0005 or compatible  
    - Output
      - J2: Phoenix Contact MKDS 3/ 5-5,08 / 1905201 or any compatible 5 hole screw terminal that can handle maximum power draw (18A or higher)
  - Components
    - Power signaling
      - Required to translate ATX PS_ON# into the 360's power on signal. The other three outputs are purely passthrough.
      - R6: 100K, 1206
      - Q1: MMBT3904 (or any compatible jellybean part)
    - Switching
      - SW1: E-Switch 500SSP1S2M2QEA or compatible, or just bridge pin 1 and 2 to always have the board active.
- Optional
  - Indicators
    - Blinkenlights to see if each power rail is working. If you want these, install the following resistor-LED pairs:
    - +12V
      - R3: 1K, 1206
      - D3: 20mW LED, 1206  
    - +5V
      - R5: 330R, 1206
      - D5: 20mW LED, 1206  
    - +5VSB
      - R4: 330R, 1206
      - D4: 20mW LED, 1206
    - +3.3V
      - R1: 150R, 1206
      - D1: 20mW LED, 1206
    - -12V
      - R8: 1K, 1206
      - D6: 20mW LED, 1206  
    - PWR_OK
      - R2: 330R, 1206
      - D2: 20mW LED, 1206
  - 5V Load
    - Older (much older) PSUs are regulated on the 5V rail while more modern ones are regulated on the 12V rail, since the 360 is almost exclusively 12V load some PSUs may shut down prematurely. If you have such a PSU or just want to play it safe, install the following to have a 10W load:
    - SW2: E-Switch 500SSP1S2M2QEA or compatible (or bridge pins 1 and 2 to always have the load active)
    - R7: 2R5, TO220, 15W (ex. Bourns PWR220T-20-2R50F)

# Contributing
Feel free to do what you like with this; build it, change it, fix it, sell it, whatever. If you do make improvements it'd be super cool to make those publicly available.
