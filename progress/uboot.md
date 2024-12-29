# Why is the old kernel entered right away? Why doesn’t it wait for UBoot?

## Checking UBoot version

U-boot version changes actually does change something

### Checking U-Boot version

Actually does change something:

U-Boot SPL 2024.04-00038-gbaca7b469d-dirty (May 21 2024 - 13:06:46 +0200)

U-Boot 2024.04-00038-gbaca7b469d-dirty (May 21 2024 - 13:06:46 +0200)

So the U-boot launched here is the one we compiled on our laptop.

https://docs.u-boot.org/en/latest/develop/version.html

### Actually enabling debug logging for UBOOT to see what’s going on

CHANGING LOG LEVEL to 7

- **LOG 1**
    
    ```bash
    Sending spl/u-boot-spl.bin, 829 blocks: Give your local XMODEM receive command now.
    Bytes Sent: 106240   BPS:7060                            
    
    Transfer complete
    
    *** exit status: 0 ***
    
    U-Boot SPL 2024.04-00038-gbaca7b469d-dirty (May 21 2024 - 13:38:50 +0200)
    Trying to boot from UART
    CC
    *** file: u-boot.img
    $ sx -vv u-boot.img
    Sending u-boot.img, 7741 blocks: Give your local XMODEM receive command now.
    Xmodem sectors/kbytes sent:   0/ 0kRetry 0: NAK on sector
    Bytes Sent: 990976   BPS:8691                            
    
    Transfer complete
    
    *** exit status: 0 ***
    Loaded 990924 bytes
    
    U-Boot 2024.04-00038-gbaca7b469d-dirty (May 21 2024 - 13:38:50 +0200)
    
    CPU  : AM335X-GP rev 2.1
    Model: TI AM335x BeagleBone Black
    DRAM:  512 MiB
    Reset Source: Power-on reset has occurred.
    RTC 32KCLK Source: External.
    Core:  150 devices, 14 uclasses, devicetree: separate
    WDT:   Started wdt@44e35000 with servicing (60s timeout)
    MMC:   OMAP SD/MMC: 0, OMAP SD/MMC: 1
    Loading Environment from EXT4... Board: BeagleBone Black
    <ethaddr> not set. Validating first E-fuse MAC
    BeagleBone Black:
    BeagleBone Cape EEPROM: no EEPROM at address: 0x54
    BeagleBone Cape EEPROM: no EEPROM at address: 0x55
    BeagleBone Cape EEPROM: no EEPROM at address: 0x56
    BeagleBone Cape EEPROM: no EEPROM at address: 0x57
    Net:   eth2: ethernet@4a100000, eth3: usb_ether
    Press SPACE to abort autoboot in 0 seconds
    board_name=[A335BNLT] ...
    board_rev=[00C0] ...
    gpio: pin 56 (gpio 56) value is 0
    gpio: pin 55 (gpio 55) value is 0
    gpio: pin 54 (gpio 54) value is 0
    gpio: pin 53 (gpio 53) value is 1
    switch to partitions #0, OK
    mmc1(part 0) is current device
    Scanning mmc 1:1...
    libfdt fdt_check_header(): FDT_ERR_BADMAGIC
    Scanning disk mmc@48060000.blk...
    Disk mmc@48060000.blk not ready
    Scanning disk mmc@481d8000.blk...
    Found 2 disks
    No EFI system partition
    BootOrder not defined
    EFI boot manager: Cannot load any image
    gpio: pin 56 (gpio 56) value is 0
    gpio: pin 55 (gpio 55) value is 0
    gpio: pin 54 (gpio 54) value is 0
    gpio: pin 53 (gpio 53) value is 1
    switch to partitions #0, OK
    mmc1(part 0) is current device
    gpio: pin 54 (gpio 54) value is 1
    Checking for: /uEnv.txt ...
    Checking for: /boot/uEnv.txt ...
    gpio: pin 55 (gpio 55) value is 1
    2005 bytes read in 2 ms (978.5 KiB/s)
    Loaded environment from /boot/uEnv.txt
    Checking if uname_r is set in /boot/uEnv.txt...
    gpio: pin 56 (gpio 56) value is 1
    Running uname_boot ...
    loading /boot/vmlinuz-4.19.94-ti-r42 ...
    10095592 bytes read in 638 ms (15.1 MiB/s)
    debug: [enable_uboot_overlays=1] ...
    debug: [enable_uboot_cape_universal=1] ...
    debug: [uboot_base_dtb_univ=am335x-boneblack-uboot-univ.dtb] ...
    uboot_overlays: [uboot_base_dtb=am335x-boneblack-uboot-univ.dtb] ...
    uboot_overlays: Switching too: dtb=am335x-boneblack-uboot-univ.dtb ...
    loading /boot/dtbs/4.19.94-ti-r42/am335x-boneblack-uboot-univ.dtb ...
    162266 bytes read in 14 ms (11.1 MiB/s)
    Found 0 extension board(s).
    uboot_overlays: [fdt_buffer=0x60000] ...
    uboot_overlays: loading /lib/firmware/BB-ADC-00A0.dtbo ...
    867 bytes read in 3 ms (282.2 KiB/s)
    uboot_overlays: loading /lib/firmware/BB-BONE-eMMC1-01-00A0.dtbo ...
    1584 bytes read in 4 ms (386.7 KiB/s)
    uboot_overlays: loading /lib/firmware/BB-HDMI-TDA998x-00A0.dtbo ...
    4915 bytes read in 11 ms (435.5 KiB/s)
    uboot_overlays: loading /lib/firmware/AM335X-PRU-RPROC-4-19-TI-00A0.dtbo ...
    3801 bytes read in 10 ms (371.1 KiB/s)
    loading /boot/initrd.img-4.19.94-ti-r42 ...
    6589689 bytes read in 422 ms (14.9 MiB/s)
    debug: [console=ttyS0,115200n8 bone_capemgr.uboot_capemgr_enabled=1 root=/dev/mmcblk1p1 ro rootfstype=ext4 rootwait coherent_pool=1M net.ifnames=0 lpj=1990656 rng_core.default_quality=100 quiet] ...
    debug: [bootz 0x82000000 0x88080000:648cf9 88000000] ...
    Kernel image @ 0x82000000 [ 0x000000 - 0x9a0be8 ]
    ## Flattened Device Tree blob at 88000000
       Booting using the fdt blob at 0x88000000
       Loading Ramdisk to 8f9b7000, end 8ffffcf9 ... OK
       Loading Device Tree to 8f92b000, end 8f9b6fff ... OK
    
    Starting kernel ...
    
    ```
    

Problem with enable debug logging (LOG LEVEL 9)

```bash
u-boot-spl section .data' will not fit in region .sram'
region `.sram' overflowed by 2352 bytes
```

- U-boot SPL linker script:
    
    ```
    
    // MEMORY { .sram : ORIGIN = IMAGE_TEXT_BASE,LENGTH = IMAGE_MAX_SIZE }
    MEMORY { .sram : ORIGIN = 0x402F0400, LENGTH = ((0x4030B800 - 0x00000400) - 0x402F0400) }
    
    // MEMORY { .sdram : ORIGIN = CONFIG_SPL_BSS_START_ADDR, LENGTH = CONFIG_SPL_BSS_MAX_SIZE }
    MEMORY { .sdram : ORIGIN = 0x80a00000, LENGTH = 0x80000 }
    
    OUTPUT_FORMAT("elf32-littlearm", "elf32-littlearm", "elf32-littlearm")
    OUTPUT_ARCH(arm
    # Entry point: at the "_start"-label
    ENTRY(_start)
    SECTIONS
    {
    .text :
    {
    __start = .;
    *(.vectors)
    arch/arm/cpu/armv7/start.o (.text*)
    *(.text*)
    } >.sram
    . = ALIGN(4);
    
    # Read-only data (e.g.: string constants)
    .rodata : { *(SORT_BY_ALIGNMENT(.rodata*)) } >.sram 
    
    # 4 byte align along the current location counter
    . = ALIGN(4);
    
    .data : { *(SORT_BY_ALIGNMENT(.data*)) } >.sram
    . = ALIGN(4);
    .u_boot_list : {
    KEEP(*(SORT(.u_boot_list*)));
    } >.sram
    . = ALIGN(4);
    __image_copy_end = .;
    .end :
    {
    *(.__end)
    }
    _image_binary_end = .;
    
    # Uninitialized global variables
    .bss :
    {
    . = ALIGN(4);
    __bss_start = .;
    *(.bss*)
    . = ALIGN(4);
    __bss_end = .;
    } >.sdram
    }
    ```
    

This comes generated directly from (arch/arm/mach-omap2/u-boot-spl.lds)

u-boot.cfg / autoconf.mk

```markdown
-DIMAGE_TEXT_BASE=0x402F0400

#define CONFIG_SPL_TEXT_BASE 0x402F0400
#define CONFIG_SPL_MAX_SIZE (SRAM_SCRATCH_SPACE_ADDR - CONFIG_SPL_TEXT_BASE)

#define CONFIG_SPL_PAD_TO CONFIG_SPL_MAX_SIZE

#define CONFIG_SPL_BSS_MAX_SIZE 0x80000
#define CONFIG_SPL_BSS_START_ADDR 0x80a00000
#define CONFIG_SYS_SPL_MALLOC_START (CONFIG_SPL_BSS_START_ADDR + CONFIG_SPL_BSS_MAX_SIZE)
```

- board_init_r (called)
    - boot_order:
    - question: why is debug() not displayed?

from there

- boot_from_devices() (called)

from there 

- loader = spl_ll_find_loader(bootdev); (called)

**Removing elements from config**

CONFIG_SPL_ETH=y

When removing it, for some reason no serial information is received by the device anymore and nothing is printed either.

**Issue: how is UART initially initialized inside SPL bootloader, and all other drivers?**

Inside am335x-bone-common.dts, stdout-path is mentioned as “uart0”.

However, in the file are mentioned

- am335x-boneblack.dts
- am33xx.dtsi
- am335x-bone-common.dtsi
    - Here stdout-path = &uart0
    - AM33XX_PADCONF muxes AM335x_PIN_UART0_RXD and AM335x_PIN_UART0_TXD

```bash
	uart0_pins: pinmux_uart0_pins {
		pinctrl-single,pins = <
			AM33XX_PADCONF(AM335X_PIN_UART0_RXD, PIN_INPUT_PULLUP, MUX_MODE0)
			AM33XX_PADCONF(AM335X_PIN_UART0_TXD, PIN_OUTPUT_PULLDOWN, MUX_MODE0)
		>;
	};
	-> This basically just includes the uart address here
```

However: the initialization? It happens where?

board.c and mux.c is compiled I believe with the SPL, the “puts” function however doesn’t show us anything.

Mainly check the .config file

- SPL_SERIAL_PRESENT

is set to yes

probably ns16550.c is used here, not the serial_ns16550.c

## As a consequence of removing certain ethernet-usb drivers the prints stop working for some reason.

Configs for serial connection

- CONFIG_SYS_NS16550
- CONFIG_SYS_NS16550_SERIAL
- CONFIG_SYS_NS16550_CLK (48000000)
- CONFIG_SYS_NS16550_COMi

CONFIG_SYS_IMMR (Physical address of internal memory)

### fdt

In very broad terms, the CONFIG options in general control *what* driver
files are pulled in, and the fdt controls *how* those files work.

```c
#ifdef CONFIG_SYS_NS16550
	do_fixup_by_compat_u32(blob, "fsl,16550-FIFO64",
			       "clock-frequency", CONFIG_SYS_NS16550_CLK, 1);
#endif
```

### clock.c

```c
#ifdef CONFIG_SYS_NS16550
	/* Enable UART1 clocks */
	setbits_le32(&prcm_base->fclken1_core, 0x00002000);
	setbits_le32(&prcm_base->iclken1_core, 0x00002000);

	/* Enable UART2 clocks */
	setbits_le32(&prcm_base->fclken1_core, 0x00004000);
	setbits_le32(&prcm_base->iclken1_core, 0x00004000);

	/* UART 3 Clocks */
	setbits_le32(&prcm_base->fclken_per, 0x00000800);
	setbits_le32(&prcm_base->iclken_per, 0x00000800);
#endif
```

### Makefile

```makefile
obj-$(CONFIG_SYS_NS16550) += ns16550.o
```

So ns16550.c is added

### ns16550.c

no dynamic serial configuration

→ The dynamic configuration allows the device-tree control of the driver.

- **Init function**
    
    ```c
    
    void ns16550_init(struct ns16550 *com_port, int baud_divisor)
    {
    	while (!(serial_in(&com_port->lsr) & UART_LSR_TEMT))
    		;
    
    	serial_out(CONFIG_SYS_NS16550_IER, &com_port->ier);
    	serial_out(0x7, &com_port->mdr1);	/* mode select reset TL16C750*/
    	serial_out(UART_MCRVAL, &com_port->mcr);
    	serial_out(ns16550_getfcr(com_port), &com_port->fcr);
    	/* initialize serial config to 8N1 before writing baudrate */
    	serial_out(UART_LCRVAL, &com_port->lcr);
    	if (baud_divisor != -1)
    		ns16550_setbrg(com_port, baud_divisor);
    
    	/* /16 is proper to hit 115200 with 48MHz */
    	serial_out(0, &com_port->mdr1);
    }
    ```
    
- **I/O**
    
    ```c
    
    void ns16550_putc(struct ns16550 *com_port, char c)
    {
    	while ((serial_in(&com_port->lsr) & UART_LSR_THRE) == 0)
    		;
    	serial_out(c, &com_port->thr);
    
    	/*
    	 * Call watchdog_reset() upon newline. This is done here in putc
    	 * since the environment code uses a single puts() to print the complete
    	 * environment upon "printenv". So we can't put this watchdog call
    	 * in puts().
    	 */
    	if (c == '\n')
    		WATCHDOG_RESET();
    }
    ```
    
    ```c
    char ns16550_getc(struct ns16550 *com_port)
    {
    	while ((serial_in(&com_port->lsr) & UART_LSR_DR) == 0) {
    		WATCHDOG_RESET();
    	}
    	return serial_in(&com_port->rbr);
    }
    ```
    
    ```c
    int ns16550_tstc(struct ns16550 *com_port)
    {
    	return (serial_in(&com_port->lsr) & UART_LSR_DR) != 0;
    }
    ```
    
    tstc. Checks if there is a character in the input buffer
    

### Serial_mtk.c

handles muxing

### serial_ns16550.c

Not included I believe due to makefile situation

### serial_omap.c

```
static int omap_serial_of_to_plat(struct udevice *dev)
```

### am335x_evm.h

Contains a whole bunch of macros

- NANDARGS
- BOOTENV_DEV_LEGACY_MMC
- BOOTENV_DEV_NAME_LEGACY_MMC
- BOOTENV_DEV_NAND
- BOOTENV_DEV_NAME_NAND
- BOOT_TARGET_USB
- BOOT_TARGET_PXE
- BOOT_TARGET_DHCP
- BOOT_TARGET_DEVICES
    - Which lists all boot options
- CONFIG_EXTRA_ENV_SETTINGS
- CONFIG_SYS_BOOTCOUNT_BE
- CONFIG_SYS_NS16550_COMi

### config_whitelist

[check-config.sh](http://check-config.sh) checks the config, raises an error if something is not right unless the configs are in config_whitelist

## Where does the printing in the spl-part of u-boot actually happen?