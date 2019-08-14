# WORK IN PROGRESS 

--- More info later... ---


***Dimensions***
Perfboard 13x23 holes, +/- 34*58mm

***Board layout***
 - Top section (15 holes) for 7-segment digit (7x14 holes) 
 - Bottom section (8 holes) for connectors, 2x 5-pin headers, top pin in row 17, female input left, male output right.

***Connections***
2x 5-pin headers, pin numbering top-to-bottom

**Input (female/left):**
- 1- GND
- 2- Clock SRCLK - connect to SPI-SCK
- 3- Latch RCLK  - connect to SPI-SS
- 4- Serial SER  - connect to SPI-MOSI
- 5- VCC (5V)

**Output (male/right):**
- 1- GND
- 2- Clock SRCLK - connect to SPI-SCK
- 3- Latch RCLK  - connect to SPI-SS
- 4- Cascade SER Qh' - to SER of next module
- 5- VCC (5V)

***Communication Protocol***
595-shift-register SPI-compatible (see pinout below).
8-bit bytes are clocked-in per bit (LSBFIRST) and latched per byte.
Each byte contains two nibbles. The low nibble (bits 0-3) contains the digit as binary coded decimal (BCD). The high nibble (bits 4-7) can be used for module specific control.

high (7:4) | low (3:0)
----|----
control | digit

The highest bit (bit 7) of the control nibble is reserved for an optional decimal point.

**595 Pinouts & connections**
The 74HC595 shift register requires three pins plus VCC/GND.
The shift register can be connected to the hardware SPI pin for best speed.
- Default pins for ESP8266:   SS=15, MOSI=13, SCLK=14
- Default pins for ATmega328/168/8: SS=10, MOSI=11, SCLK=13
- Default pins for ATtiny85:  SS=0, MOSI=1, SCLK=2
- Suggested pins for ESP-01: SS=3 (RX), MOSI=2, SCLK=0

This fine piece of ASCII-art shows the pinout of the 74HC595. Of course you can also review the [TI datasheet](http://www.ti.com/lit/ds/symlink/sn74hc595.pdf) (PDF).
```
Pinout 74HC595 shift register DIP and SMD model:

     +--v--+          PINS 1-8       PINS 9-16
  1 -+     +- 16      1: output Qb   16: VCC
  2 -+     +- 15      2: output Qc   15: output Qa   - expanded pin P0
  3 -+     +- 14      3: output Qd   14: Serial SER  - connect to SPI-MOSI
  4 -+     +- 13      4: output Qe   13: Enable OE   - active low, so connect to GND
  5 -+     +- 12      5: output Qf   12: Latch RCLK  - connect to SPI-SS
  6 -+     +- 11      6: output Qg   11: Clock SRCLK - connect to SPI-SCK
  7 -+     +- 10      7: output Qh   10: Clear SRCLR - active low, so connect to VCC
  8 -+     +- 9       8: GND          9: Cascade SER Qh' - to SER of next chip
     +-----+
```
