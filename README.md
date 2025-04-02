This repository contains a Linux kernel configuration file suitable for a Dell Wyse 3040 thin client. It includes a basic configuration to make use of the hardware the 3040 provides, while excluding unnecessary drivers. This will be enough to get the system up and running, but you might need to enable extra options depending on your specific needs.

The current .config file is targeted at linux-6.14. For other versions, check out the 'releases' section.

A quick overview is provided below.

# Additional Firmware Files
The 3040 uses the following firmware files:

```
intel/fw_sst_22a8.bin   // Audio
mrvl/sd8897_uapsta.bin  // Wifi/BT
rtl_nic/rtl8168h-2.fw   // Ethernet
```

The ethernet firmware is not required, but recommended. If you want audio, you'll need the audio firmware.

The Wifi/BT driver is required, if you have the bundled Wifi card. See the section below for more details.

This firmware is provided by the ```linux-firmware``` package.

# Wifi and Bluetooth
The optional Azurewave AW-CM389MA Wifi and Bluetooth card uses the Marvell 8897 chipset. The driver for this card is configured as a module - if your device has is, the necessary modules will be loaded automatically.

If you know you don't have the card, you can disable the following options:

```
CONFIG_MWIFIEX
CONFIG_MWIFIEX_SDIO
CONFIG_BT
CONFIG_WLAN
```

# Audio
Audio is enabled by default, though requires the firmware file noted above.

To remove audio-related drivers, disable

```
CONFIG_SND_SOC
```

# Graphics Driver
The .config file diables the Intel ```i915``` driver. If you're not running your Wyse headless, enable the following option.

```
CONFIG_DRM_I915
```

Note that having this setting disabled does not disable graphical output entirely, the machine can still be hooked up to a monitor if required.

This reduces the kernel size by around 1.3MB. Disabling this will also disable a few other options related to the ```i915``` driver.

# Kernel Command Line
The kernel command line is disabled. If you want to use ```efistub``` to load your kernel, be sure to turn this on and add your root partition PARTUUID as required.

# Vulnerability Mitigations
Kernel mitigations are disabled, for performance reasons. The 3040 is vulnerable to:

```
Spectre v1
Spectre v2
Meltdown
```

Enable the following to build the kernel with mitigations.

```
CONFIG_CPU_MITIGATIONS
```

If you do compile your kernel with mitigations, they can be disabled at boot adding ```mitigations=off``` to your kernel command line.