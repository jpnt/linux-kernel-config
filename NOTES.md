# Linux Kernel Notes

## Downloading

Linux kernel sources can be obtained from the official website: https://kernel.org/

## Configuration

All configurations for the Linux kernel are located at the root of the source 
in a file called .config.

While it's possible to configure directly from this file, it can be confusing 
and less intuitive. To simplify this process, various tools have been developed.
One such tool is menuconfig, which provides a curses interface for kernel 
configuration. To open it, type: make menuconfig. Alternatively, you can use 
xconfig or gconfig for a graphical interface.

## Compiling

* To compile the Linux Kernel, use the following command: make -j$(nproc).

* To compile Linux Kernel modules separately, use: make modules.

Kernel modules are pieces of code that can be dynamically loaded into the 
running kernel, providing additional functionality or support for specific 
hardware. The compiled modules are typically placed in a separate directory, 
often named modules. This directory structure is created to organize the modules.

## Installing

* To install the Linux Kernel, execute: make install.

The kernel image, which is the core executable of the Linux kernel, is copied 
to the /boot directory or another location specified in the configuration.
If applicable, the bootloader configuration is updated to include the new kernel.

* For installing Linux Kernel modules, use: make modules_install.

Running make modules_install focuses on installing the kernel modules that were 
compiled in the earlier make modules step. The compiled kernel modules are copied 
to the appropriate system directories, typically under /lib/modules/[kernel_version]/.

## Finding Drivers

## Initramfs or Initrd

## Debugging

## Performance

## Power Management

## Kernel Modules

## Hardware Virtualization

## Kernel Hardening

## Networking Configuration

## Benchmarking
