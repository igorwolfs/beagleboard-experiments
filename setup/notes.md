**Source Link:** https://labs.dese.iisc.ac.in/embeddedlab/build-u-boot-for-beaglebone/

## Install the ARM-7 AM355x Toolchain

<aside>
🔴 `cc1: error: bad value (‘generic-armv7-a’) for ‘-mtune=’ switch` is due to an incorrect toolchain chosen for compilation.

</aside>

These are toolchains made to run on x86_64 and compile for arm cortex A8 and A7

```bash
wget https://developer.arm.com/-/media/Files/downloads/gnu-a/9.2-2019.12/binrel/gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf.tar.xz
tar -Jxvf gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf.tar.xz -C $HOME
wget https://developer.arm.com/-/media/Files/downloads/gnu-a/9.2-2019.12/binrel/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz
tar -Jxvf gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu.tar.xz -C $HOME
```

https://software-dl.ti.com/processor-sdk-linux/esd/docs/latest/linux/Overview_Building_the_SDK.html

```bash
export TOOLCHAIN_PATH_ARMV7=$HOME/gcc-arm-9.2-2019.12-x86_64-arm-none-linux-gnueabihf
export TOOLCHAIN_PATH_ARMV8=$HOME/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu
```

## Clone + Compile U-Boot Repository as well as differentials

<aside>
🔴 .patch files are generated with git diff `git diff > some-changes.patch` and applied with `git apply /path/to/some-changes.patch`

```bash
git clone -b v2022.04 https://github.com/u-boot/u-boot --depth=1
cd u-boot/
git pull --no-edit https://git.beagleboard.org/beagleboard/u-boot.git v2022.04-bbb.io-am335x-am57xx
export CROSS_COMPILE=$TOOLCHAIN_PATH_ARMV7/arm-none-linux-gnueabihf-
export ARCH=arm
make distclean
make am335x_evm_defconfig
make -j4
```

</aside>

## USART Boot-loader load

1. Command

Default working:

```
*Baud rate: 115,200
*Data bits: 8
*Parity: None
*Stop bits: 1
*Flow control: None
```

```bash
# sx: send with x-modem protocol
# vv: 
sudo picocom -b 115200 /dev/ttyUSB0 --send-cmd "sx -vv" 
```

1. press ctrl-a + ctrl-s
2. enter the file-name (u-boot-spl.bin) → first stage bootloader
3. hit enter while entering boot mode.

```
U-Boot SPL 2022.04-00038-gbaca7b469d (May 21 2024 - 11:41:05 +0200)
Trying to boot from UART

Sending u-boot.img, 7741 blocks: Give your local XMODEM receive command now.
Xmodem sectors/kbytes sent:   0/ 0kRetry 0: NAK on sector
Retry 0: NAK on sector
Bytes Sent: 990976   BPS:8506
Transfer complete

- ** exit status: 0 ***
Loaded 990908 bytes

U-Boot 2022.04-00038-gbaca7b469d (May 21 2024 - 11:41:05 +0200)
```