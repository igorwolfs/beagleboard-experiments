# General boot sequence

5 different boot methods exist:

- eMMC Boot
- SD boot
- Serial boot UART
- USB boot
- Ethernet boot

[AM335x and AMIC110 Sitara™ Processors](https://www.notion.so/AM335x-and-AMIC110-Sitara-Processors-6de2a3cbc0fe467dbaf1c9937ce3f098?pvs=21)

1. Boot config pin is sampled at power-on-reset
2. Depending on boot mode, copies image to internal RAM nad executes it
    
    NOTE: Sometimes it might take minutes for that boot peripheral to time-out!
    
    Without button press:
    
    eMMC→microSD→USBport→serial
    
    With button press:
    
    microSD→USB port→serial
    
3. Maximum boot image size: 128 kb
    1. Modes
    2. This is also the reason why you don’t ismply load U-Boot in there.
4. SPL (Secondary program Loader) / MLO (MMC Loader) is loaded
5. U-Boot is then run 
6. U-Boot loads the Linux Kernel
7. Linux kernel loads the RFS

<aside>
💡 Assumption: we need to somehow get the U-Boot image and put it in flash memory so it can be copied to RAM OR copy it to RAM directly from the USB / UART device. Then we need to using U-Boot running on the CPU copy the image to ram or copy it into flash and then run it into RAM.

</aside>

## Boot sequences and pins

Pull-down / Pull-up: 100k

- Use 100 Ohm pull-down / Pull-up to modify (3.3/1k = 3.3 mA)
- VDD_3V3B:
    - used to pull high
    - max output current 0.5 A
- VDD_5V → Should be zero, this is input from power connector
- SYS_5V → VSYS output of the power path
    - All voltage regulators are powered from this output
    - Power path: (page 27 on datasheet: direct input from USB / AC)
- Limits for HIGH and LOW for GPIOS


| Boot pin | Default state | Functionality | Pin |
| --- | --- | --- | --- |
| SYS_BOOT0 | 0 | Boot sequence | LCD_DATA0 |
| SYS_BOOT1 | 0 | Boot sequence | LCD_DATA1 |
| SYS_BOOT2 | 1 | Boot sequence | LCD_DATA2 |
| SYS_BOOT3 | 1 | Boot sequence | LCD_DATA3 |
| SYS_BOOT4 | 1 | Boot sequence | LCD_DATA4 |
| SYS_BOOT5 | 1 | CLKOUT |  |
| SYS_BOOT6 | 0 | Phy mode |  |
| SYS_BOOT7 | 0 | Phy mode |  |
| SYS_BOOT8 | 0 | XIP bus width |  |
| SYS_BOOT9 | 0 | NAND / NANDI2C / NAND / Fast external |  |
| SYS_BOOT10 | 0 | XIP / NAND / MUX |  |
| SYS_BOOT11 | 0 | XIP / NAND / MUX |  |
| SYS_BOOT12 | 0 | reserved (0) |  |
| SYS_BOOT13 | 0 | reserved (0) |  |
| SYS_BOOT14 | 1 | Clock |  |
| SYS_BOOT15 | 0 | Clock |  |

Boot sequences are described in the image
- 111000: default sequence
- 110000: when uSD boot switch is pulled to ground

FOR UART
- Boot sequence: 100000
    - Goes through XIP first
    - Might be that there was direct code execution from serial data

For UART to be first, boot sequence should be 000001

- SYS_BOOT0-SYS_BOOT4
- LCD_DATA0-LCD_DATA4

Normally pulled up are: 2, 3, 4. To be pulled down must be: 2, 3.
Connect LCD_DATA2 and LCD_DATA3 to DGND. For UART (v2)
For that case, 2, 3 and 4 have to be pulled down. And 1 has to be pulled up.