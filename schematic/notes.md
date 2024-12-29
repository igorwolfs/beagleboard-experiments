# USB

## USB0_ID (PC connector)

Mini-usb connector

Connected to CPU

- P16: USB0_ID
- N18: USB0_DM
- N17: USB0_DP
- UNCONNECTED: USB0_CE
- UNCONNECTED: USB0_DRVVBUS/GPIO0_18

(https://prod-files-secure.s3.us-west-2.amazonaws.com/03f75d9a-aac8-497f-b398-b25b8b1d8147/8a28affa-4d66-4a32-a5d2-eedaac42ffad/Untitled.png)

## USB1_ID (Host)

USB A connector

- R17: USB1_DP
- R18: USB1_DM
    - P17: USB1_ID
- F15: USB1_DRVVBUS/GPIO3_13
- T18: USB1_VBUS


(https://prod-files-secure.s3.us-west-2.amazonaws.com/03f75d9a-aac8-497f-b398-b25b8b1d8147/b17c1096-d9cd-43e4-b772-bd684dc9923c/Untitled.png)

- TPS2051BDGNR (power switch)
    - Power distribution switch
    - 2 switches, each switch is controlled by logic enable channel
    - Operates from 2.7 volts input
    - When the output load exceeds the current limit threshold, the device limits the output current by switching to constant-current mode (pin NOToc)
- TPD4S012DRYR (ESD-component)
- UB11123-4H9-4F (USB-A connector)

# Power

## TPS65217x Single-chip PMIC for BP systems

- 2 A output current on power path
- Linear charge: max 0.7 A
- 20 V tolerant USB and AC inputs

Through

- 3 buck-converters (more efficient for high-power)
- 2 LDO’s (less efficient, low power and smaller)
- 2 load switches 

Load switch datasheet: https://www.ti.com/lit/an/slva652a/slva652a.pdf?ts=1713291078674&ref_url=https%253A%252F%252Fwww.google.com%252F)

    - Connect /disconnect load from source
    - Can be used as LDO
- Boost converter (to power LED-strings)

### Power sources

- USB-port (USB-mini)
- 5-V adapter (coax)
- Li-Ion battery (UNCONNECTED)

### Inductor pins

- L1, L2, L3 connect the buck-converter inductor
- L4 (WLED boost converter, unconnected)

### PB-in

Pin button connected to ground pin typically.

### DC-DC inputs

VDCDCi

- output and feedback voltage-sense input
- Output 1: connects to DDR
- Output 2: connects to MPU
- Output 3: connects to CORE (AM335X is powered from here)

VIN_VDCDCi

- input voltage for DCDCi, pin must be connected to SYS pin.

SYS:

- System voltage pin
- Output of power path
- Voltage regulators are powered from this output.

LDO

- LDO4 (LS2_OUT), output for LDO4 → VDD_3v3A
- LDO3 (LS1_OUT), output for LDO3 → VDD_1V8
- LDO_inputs → powered by SYS_5V

### I2C

I2C interface (SCL, SDA) can control the output of the 3 Buck-converters, LDO’s, thresholds, etc.. 

- These 2 lines are pulled up at 3v3a
- NOTE: I2C lines are pulled high by default during communication
    - This way the I2C bus can be shared with multiple slave
    - They can switch to master mode and pull the data line low on communication initialization

Link: https://www.ti.com/lit/an/slva704/slva704.pdf?ts=1713249516699&ref_url=https%253A%252F%252Fwww.google.com%252F


### Other

PWR_EN

- Should be pulled high to start power-up sequence

INT

- High by default (probably interrupt)

## TL5209DR

0.5 A LDO with shutdown

Based on whether the VDD_3v3A LDO is enabled it takes VSYS and regulates it down to VDD_3V3B.

EN high: > 2 V

EN low: < 0.4 → shutdown

# Memory

Kingston guide to memory: https://dcc.ufrj.br/~gabriel/microarq/umg.pdf

SD-card: https://en.wikipedia.org/wiki/SD_card#microSD

## DDR3L SDRAM (D2516EC4BXGGB)

**sources**: https://resources.pcb.cadence.com/blog/ddr-bus-design-for-pcb-engineers

### Communication

- DDR_Di (0..15)
    - Data line transferring bits on each leading and falling edge of the clock signal
    - Bidirectional
- DDR_CLK(n)
- DDR_CSn
- DDR_RASn
- DDR_CASn
    - Command line/addre
- DDR_WEn
- DDR_RESETn
- DDR_Ai (0..15): Address inputs
- DDR_BAi (0..3): Bank select
- DDR_ODT
- DDR_DQ(S)(N)(M)1(0)
    - Data strobe encoding: helps with timing, improving jitter tolerance and clock recovery

### Power

- VDDS_DDR: (1.5 V)
    - Supply voltage for internal circuit
- VREF: reference voltages

### Registers

The standard for DDR communication and drivers can be found:

https://en.wikipedia.org/wiki/DDR3_SDRAM

## EMMC (KE4CN2H5A-A58)

These chips are distributed by kingston, and bought by kingston from one of the manufacturers (in this case micron)

- KE4CN2H5A-A58: kingston model number
- MTFC4GLDEA 0M WT: micron model number


### Communication

Happens through – MMCplus™ and MMCmobile™ protocols

- MMC1_DATi, i:0..7 (I/O’s for data transfer)
    - Options: 1 (default), 4 or 8 lines
    - internal pull-ups only work when low number of data lines are active (which is why we need external pull-ups)
- MMC1_CMD: (slave only)
    - 2 modes: Open-drain mode, Push-pull mode
- MMC1_CLK (52 MHz max):
- eMMC_RSTn: Move device to pre-idle state
    - ECSD register byte must be set before usable

All these lines are in open-drain configuration with a 10 kOhm resistor.

Example of the MMCplus protocol specification:

[www.mikrocontroller.net](https://www.mikrocontroller.net/attachment/101561/AN_MMCA050419.pdf)

### Power

- Vcci: NAND interface + NAND flash power supply
- VCCQi: EMMC controller core and EMMC I/F I/O power supply
- VssQi (digital ground): controller and I/F ground connection
- VSSi (digital ground): NAND I/F I/O flash ground connection
- VCCI: Internal voltage, attach supply capacitor

### Registers

- CID: card identification (device identification information)
- CSD: Card-specific data (info about accessing the device content)
- ECSD: Device properties and selected modes (read-only)

## uSD

YL004-030-001 (Connector)

http://www.dgyuliang.net/d/file/Produtcs/Card_Socket/SD_Card_Socket/7a09129dce8bc53ef69040e9e7f9282e.pdf

**Link:** https://en.wikipedia.org/wiki/SD_card#microSD

### Communication

All communication lines are assumed open drain (pulled high by default)

Different SD-bus mode communication exists

- MMC0_DATi, i:0..4
- CLK
- CMD
    - 48 bit commands and responses
- CD (Card detect line)

MicroSD cards

# CPU

## DDR

- DDR_VREF: half of the power supply votlage generated by an on-board regulator

# Display

## **TDA19988BHN/C1,557**

**Link:** https://www.mouser.de/datasheet/2/302/NXP_TDA19988-1189083.pdf

## 10118241-001RLF

HDMI connector

# Connectivity

## LAN8710A-EZC-TR

https://www.mouser.de/datasheet/2/268/00002164B-977414.pdf, Ethernet transceiver

## LPJ0011BBNL

https://www.micros.com.pl/mediaserver/info-z%20lpj0011bbnl.pdf
Ethernet connector