# LCD

## communication types
### 1. parallel communication
mc --8 wires-- mc --> 1 byte
#### cons:
- high cost (too much wires)
- large weight (wires)
- data is not ensured to be sent at the same time _(data skew)_
### 2. serial communication
single wire 
#### sending data using a standard
- UART
- SPI
- I2C
- LIN
#### sending data frames
- CAN
- Ethernet

## LCD
group of pixels
16 column x 2 rows where each digit is 5 x 8 pixels

### interfacing
through parallel communication
LCD_mc <--command-- AVR_mc
command want to display, what to display

### HW connection
gnd vcc vee rs r/w e d0 d1 d2 d3 d4 d5 d6 d7
#### 1. data pins
- d0 - d7 --> data pins
- d4 - d7 --> 4 bit mode (configuration parameter)
#### 2. power pins
- vcc --> power _vdd_
- gnd --> power _vss_
- vee --> _v0_ (potentiometer) 3k --> 3V (LCD lighting control)
#### 3. control pins
- rs --> register select
0: command mode _instruction input_
1: data mode _data input_
- r/w --> read/write
0: write
1: read (flags _busy flag_)
- e --> enable (triggering) _falling edge of H to L_
default is low
switch high then low --> triggered new action will be performed
lcd_mc goes to read r/w rs data
#### 4. backlight pins
- A --> _BLA 5v_ 5V 330Ohm
- K --> _BLK 0v_ gnd

### datasheet

#### reading busy flag
- bf = H --> internal operation being processed & instruction not accepted
- bf = L --> 

### LCD memories
    arrA[8][5] = {
        0 1 1 1 0
        1 0 0 0 1
        1 0 0 0 1
        1 1 1 1 1
        1 0 0 0 1
        1 0 0 0 1
        1 0 0 0 1
    } 

1. **CGROM** character generation ROM
arrays for all characters already saved in LCD memory (ROM)
all ASCII patterns representations 

2. **CGRAM** character generation RAM
special characters made by me during runtime (volatile)
supports 8 blocks 
(8 * 8) 512 bit...? _last 3 dont care_ reg is 8 bit
8 special characters max

3. **DDRAM** data display RAM
data being displayed now
supports 80 location (40 * 2)

### generic lcd mc convention:
0x00    ----------------------------    0x27
0x40    ----------------------------    0x67    
mc supports 80 location 0x00 ~ 0x27 (40 char)
visible 0x00 ~ 0x0F & 0x40 ~ 0x4F
to write:
- send location & data to be displayed
- send data to be displayed
must handle LCD size (configuration parameter)












