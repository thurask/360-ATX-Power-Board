# 360-ATX-Power-Board
ATX to Xbox power adapter board for Xbox 360 (and maybe Xbox One)

![360_atx_power_board](https://github.com/user-attachments/assets/34ac3775-7240-4082-a9f8-f59188915a75)


# Rationale
Powering an Xbox 360 without using the original antique brick, instead using a modern, quiet, smaller, efficient ATX power supply. Also doing the same without just having components and wire connections dangling in air or stuck on breadboard.

# Compatibility
At least Xbox 360 models Xenon, Zephyr, Opus, Falcon, Jasper, i.e. anything with the large six-sided power connector. I tested this with a Xenon. In theory it should work with the Slim (2 pin barrel jack) and E (1 pin barrel jack) models, and also the Xbox One models that use an external brick, but I don't have that hardware to test. The most power-hungry model (Xenon) came with a 203W power adapter, so a 300W or higher ATX PSU should do things well without brushing up against current maxima.

# Prerequisites
- An ATX power supply capable of supplying at least 220W over 12V
- The power cable from an Xbox 360 power brick or compatible
- Fuses (automotive style ATO fuses), 2A and 20A

# Instructions
1. Desolder or otherwise harvest the device-side power cable (i.e. not the wall end) from an Xbox 360 power brick or compatible; I think you can get the connector itself on Aliexpress if you don't have access to a brick at all and fabricate your own cable with thick enough wiring to handle the current
2. Assemble PCB, including installing the fuses
3. Connect your power supply's 24 pin ATX connector and the 4/8 pin EPS12V connector to the PCB; the absolute maximum power draw for 20 pin ATX and 4 pin EPS12V is just barely enough (20A) to fully supply the most power-hungry Xbox so ideally plug in 24 pins and 8 pins respectively
4. Connect the Xbox 360 power cable to screw terminal J2; wire colors should match markings on PCB silkscreen
   -   If you harvested a cable from a power brick, consider soldering appropriate gauge short extensions to each connector with solder seal connectors, and then crimping ferrules on the other end of the extensions so that it connects better to the screw terminals
5. Connect the other end of the Xbox 360 power cable to your console
6. Turn on your power supply; +5VSB LED (if installed) should light up
7. Set switch SW1 to on
8. Turn on Xbox 360; all LEDs should light up, as should your Xbox

# Building the PCB
Import project files into KiCad 8 and export to your PCB manufacturing service of choice, or just use the files in the grbl subfolder. It's a four layer PCB and it'll carry quite a bit of current, so I recommend thicker than default copper.

# BOM
- Connectors
  - Input
    - J1: 24-pin ATX connector, Molex 39-30-1240 or compatible
    - J3: 8-pin EPS12V connector, Molex 39-30-1080 or compatible
  - Output
    - J2: Phoenix Contact MKDS 3/ 6-5,08 / 1905201 or any compatible 6 hole screw terminal that can handle maximum power draw (>=20A)
  - Fan connector
    - If you want to keep things cool there's a 4 pin PC fan plug J4 intended for a 5V fan, Molex 47053-1000 or compatible. Use some slim Noctua 5V fan or something.
- Components
  - Power signaling
    - Required to translate ATX PS_ON# into the 360's power on signal. The other outputs are purely passthrough.
    - R7, R8: 2.2K, 1206
    - U1: SN74LVC1G14DBVR or compatible part
    - C1: 1uF 1206 25V
    - C2: 0.1uF 1206 25V
  - Switching
    - SW1: E-Switch 500SSP1S2M2QEA or compatible, or just bridge pin 1 and 2 to always have the board active.
  - Fuses
    - F1-F2: Keystone Electronics 3557-2 or compatible. Uses ATO/ATC automotive fuses.
  - Filtering:
    - C3-C4: 100uF 25V electrolytics, 6.3mm x 7.7mm footprint. 
  - Indicators
    - Blinkenlights to see if each power rail is working. Install the following resistor-LED pairs:
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
      - R6: 1K, 1206
      - D6: 20mW LED, 1206  
    - PWR_OK
      - R2: 330R, 1206
      - D2: 20mW LED, 1206
  - 5V Load
    - Older (much older) PSUs are regulated on the 5V rail while more modern ones are regulated on the 12V rail, since the 360 is almost exclusively 12V load some PSUs may shut down prematurely. If you have such a PSU or just want to play it safe, install the following to have a 5W (1A) load:
    - SW2: E-Switch 500SSP1S2M2QEA or compatible (or bridge pins 1 and 2 to always have the load active), same part as SW1
    - R9: 5R, TO220, >=15W (ex. Bourns PWR220T-20-5R00F)
  

# Contributing
Feel free to do what you like with this; build it, change it, fix it, sell it, whatever. If you do make improvements it'd be super cool to make those publicly available.

# License
CC0-1.0
