# AT91 Device Tree Overlays and FIT image

## 1. Introduction

This duplicate version of the dt-overlay-at91 repository is used to hold example device
trees for touchscreen development and bring up.  Changes were done primarily to the 
sama5d3_explained_pda4.dtso file.  Follow instructions in the WIKI link below to compile
the device tree files.

   https://github.com/atmel-maxtouch/maXTouch_SPI_linux/wiki/Device-Tree-Files

The original dt-overlay-at91 is located at the following link:

   https://github.com/linux4sam/dt-overlay-at91

A device tree overlay is a file that can be used at runtime (by the bootloader 
in our case) to dynamically modify the device tree, by adding nodes to the tree 
and making changes to properties in the existing tree.

The FIT (Flattened Image Tree) format allows for flexibility in handling images 
of various types and enhances integrity protection of images with stronger checksums.

## 2. Build DT-Overlay

To build the overlays for a board make sure the following steps are done:

* the environment variables ARCH and CROSS_COMPILE are set correctly
* (optional) the environment variable KERNEL_DIR points to Linux kernel and the 
kernel was already built for the board. This is needed because the DT Overlay 
repository uses the Device Tree Compiler (dtc) from the kernel source tree. By default, 
KERNEL_DIR is set to a linux directory that would be under the parent directory 
in the directory tree: ../linux
* (optional) the environment variable KERNEL_BUILD_DIR that points to where the Linux 
kernel binary and Device Tree blob, resulting of your compilation of the kernel, are 
located. By default, KERNEL_BUILD_DIR is set to the same directory as KERNEL_DIR. It 
shouldn't be changed if you have the habit of compiling your kernel within the Linux 
source tree 

The following example shows how to build the overlays for sama5d2_xplained:

    $ make sama5d2_xplained_dtbos

## 3. Build FIT image

 To build the FIT image with overlays for a board make sure the following steps 
are done:

* the environment variables ARCH and CROSS_COMPILE are set correctly
* (optional) the environment variable KERNEL_DIR points to Linux kernel and the 
kernel was already built for the board. This is needed because the DT Overlay 
repository uses the Device Tree Compiler (dtc) from the kernel source tree. By 
default, KERNEL_DIR is set to a linux directory that would be under the parent 
directory in the directory tree: ../linux
* (optional) the environment variable KERNEL_BUILD_DIR that points to where the 
Linux kernel binary and Device Tree blob, resulting of your compilation of the 
kernel, are located. By default, KERNEL_BUILD_DIR is set to the same directory 
as KERNEL_DIR. It shouldn't be changed if you have the habit of compiling your 
kernel within the Linux source tree.
* mkimage is installed on the development machine and the Device Tree Compiler 
from Linux kernel is in the PATH environment variable 

The following example shows how to build the FIT image for sama5d2_xplained:

    $ make sama5d2_xplained.itb

## 4. Loading FIT image in u-boot

The FIT image is a placeholder that has the zImage and the base Device Tree, plus 
additional overlays that can be selected at boot time.

The following steps are required to boot the FIT Image from U-boot:

* Load the FIT image like you would normally load the uImage or zImage.
* There is no need to load additional Device Tree Blob, the FIT image includes it
* When booting the FIT image, specify the FIT configuration to use. Several 
configurations can be appended to the basic configuration, which we name 'kernel_dtb' 

Example:

    bootm 0x24000000#kernel_dtb

This will load the FIT image from address 0x24000000 in memory and then run the 
configuration named 'kernel_dtb'. This configuration includes the kernel plus the 
base Device Tree Blob built with the kernel.

To load additional FIT configurations, just append another configuration to the command.

Example to load the image sensor controller Device Tree overlay + sensor omnivision 0v7740:

    bootm 0x24000000#kernel_dtb#isc#ov7740
