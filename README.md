Out-of-tree irda subsystem and drivers for Linux

## Prerequisites

The different drivers require different kernel features to be enabled, especially USB, the ISA DMA API, PCI and TTY. Others target specific platforms. Dependencies can be seen in src/drivers/Kbuild.

Starting with Linux 5.17 the irda module requires Appletalk protocol support to be enabled.

## Build

Use `make -C src` to build loadable kernel modules for the running kernel.

You can be more specific with the usual Kernel make commands, e.g. `make -C "/lib/modules/$(uname -r)/build" M="$(pwd)/src" net/irda.ko` builds just the irda module for the running kernel.

## DKMS

Generate a dkms.conf with `autoconf -f && ./configure`, so that you can then use `dkms add src` to add it to your DKMS instance. Use `dkms install "irda/$(git show --pretty=format:"%cd~%h" --date="format:%Y%m%d" | head -1)"` to build and install the modules once.

The configuration expects all drivers that do not require a specific platform to be buildable and does not include others, i.e. it expects the Kernel to have support for all features mentioned above.

## Device driver modules

### irtty-sir

IrTTY for use with Linux's serial driver for all IrDA ports that are 16550 compatible. Most IrDA chips are 16550 compatible. Using IrTTY will however limit the speed of the connection to 115200 bps (IrDA SIR mode).

### sh_sir

SIR on SuperH UART.

### esi-sir

ESI JetEye PC dongle. The ESI dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for ESI dongles you will have to start irattach like this: `irattach -d esi`.

### actisys-sir

ACTiSYS IR-220L and IR220L+ dongles. The ACTiSYS dongles attach to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for ACTiSYS dongles you will have to start irattach like this: "irattach -d actisys" or "irattach -d actisys+".

### tekram-sir

Tekram IrMate 210B dongle. The Tekram dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for Tekram dongles you will have to start irattach like this: `irattach -d tekram`.

### toim3232-sir

Vishay/Temic TOIM3232 and TOIM4232 based IrDA dongles.

### litelink-sir

Parallax LiteLink dongle. The Parallax dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for Parallax dongles you will have to start irattach like this: `irattach -d litelink`.

### ma600-sir

Mobile Action MA600 dongle. The MA600 dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. The driver should also support the MA620 USB version of the dongle, if the integrated USB-to-RS232 converter is supported by usbserial. To activate support for MA600 dongle you will have to start irattach like this: `irattach -d ma600`.

### girbil-sir

Greenwich GIrBIL dongle. The Greenwich dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for Greenwich dongles you will have to start irattach like this: `irattach -d girbil`.

### mcp2120-sir

Microchip MCP2120. The MCP2120 dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for MCP2120 dongles you will have to start irattach like this: `irattach -d mcp2120`.

You must build this dongle yourself. For more information see: http://www.eyetap.org/~tangf/irda_sir_linux.html

### old_belkin-sir

Adaptec Airport 1000 and 2000 dongles. Some information is contained in the comments at the top of [old_belkin-sir.c](src/drivers/old_belkin-sir.c).

### act200l-sir

ACTiSYS IR-200L dongle. The ACTiSYS IR-200L dongle attaches to the normal 9-pin serial port connector, and can currently only be used by IrTTY. To activate support for ACTiSYS IR-200L dongle you will have to start irattach like this: `irattach -d act200l`.

### kingsun-sir

KingSun/DonShine DS-620 IrDA-USB dongle. This USB bridge does not conform to the IrDA-USB device class specification, and therefore needs its own specific driver. This dongle supports SIR speed only (9600 bps).

### ksdazzle-sir

KingSun Dazzle IrDA-USB dongle. This USB bridge does not conform to the IrDA-USB device class specification, and therefore needs its own specific driver. This dongle supports SIR speed only (9600 through 115200 bps).

### ks959-sir

KingSun KS-959 IrDA-USB dongle. This USB bridge does not conform to the IrDA-USB device class specification, and therefore needs its own specific driver. This dongle supports SIR speed only (9600 through 57600 bps).

### irda-usb

IrDA USB dongles. IrDA-USB support the various IrDA USB dongles available and most of their peculiarities. Those dongles plug in the USB port of your computer, are plug and play, and support SIR and FIR (4Mbps) speeds. On the other hand, those dongles tend to be less efficient than a FIR chipset.

Please note that the driver is still experimental. And of course, you will need both USB and IrDA support in your kernel...

### stir4200

SigmaTel STIr4200 USB IrDa FIR bridge. USB bridge based on the SigmaTel STIr4200 don't conform to the IrDA-USB device class specification, and therefore need their own specific driver. Those dongles support SIR and FIR (4Mbps) speeds.

### nsc-ircc

NSC PC87108 and PC87338 IrDA chipsets. This driver supports SIR, MIR and FIR (4Mbps) speeds.

### w83977af_ir

IrDA support for the Winbond W83977AF super-io chipset. This driver should be used for the IrDA chipset in the Corel NetWinder. The driver supports SIR, MIR and FIR (4Mbps) speeds.

### donauboe

Toshiba Type-O IR and Donau oboe chipsets. These chipsets are used by the Toshiba Libretto 100/110CT, Tecra 8100, Portege 7020 and many more laptops.

### au1k_ir

IrDA peripheral on the Alchemy Au1000 and Au1100 SoCs.

### smsc-ircc2

SMC Infrared Communications Controller. It is used in a wide variety of laptops (Fujitsu, Sony, Compaq and some Toshiba).

### ali-ircc

ALi M5123 FIR Controller. The ALi M5123 FIR Controller is embedded in ALi M1543C, M1535, M1535D, M1535+, M1535D South Bridge. This driver supports SIR, MIR and FIR (4Mbps) speeds.

### vlsi_ir

VLSI 82C147 PCI-IrDA Controller. This controller is used by the HP OmniBook 800 and 5500 notebooks. The driver provides support for SIR, MIR and FIR (4Mbps) speeds.

### sa1100_ir

SA1100 Internal IR.

### via-ircc

VIA VT8231 and VIA VT1211 IrDA controllers, found on the motherboards using those VIA chipsets. To use this controller, you will need to plug a specific 5 pins FIR IrDA dongle in the specific motherboard connector. The driver provides support for SIR, MIR and FIR (4Mbps) speeds. You will need to specify the 'dongle_id' module parameter to indicate the FIR dongle attached to the controller.

### pxaficp_ir

Intel PXA2xx Internal FICP which can support both SIR and FIR. This driver relies on platform specific helper routines so available capabilities may vary from one PXA2xx target to another.

### mcs7780

MosChip MCS7780 IrDA-USB dongle. USB bridge based on the MosChip MCS7780 don't conform to the IrDA-USB device class specification, and therefore need their own specific driver. Those dongles support SIR and FIR (4Mbps) speeds.

## Q&A

### Minimum Linux versions?

5.4. (See kernel-* tags for versions that support older versions of Linux (down up 4.15).)

### Why?

The IrDA subsystem was moved to staging in Linux 4.14 and scheduled for removal from the Kernel due to being unmaintained. There are obviously some use cases for IrDA, so I thought it makes sense to keep maintaining it out-of-tree.
