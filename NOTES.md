# Linux Kernel Notes

---

## Linux Kernel Build Process

This guide outlines the steps involved in building a custom Linux kernel:

### 1. Downloading the source code

The official website for obtaining Linux kernel source code is: https://www.kernel.org/.

### 2. Kernel Configuration

* The kernel configuration file, .config, resides in the root directory of the source tree.
* While manual editing is possible, it's recommended to use dedicated tools for better usability.
* `make menuconfig` (curses interface) simplifies configuration.
* Alternatively, use xconfig or gconfig for graphical interfaces.
* During configuration, you choose which features to compile directly into the kernel and which ones will be loaded as modules later.
* You can search a configuration in the curses interface by typing `/`.

### 3. Compiling the Kernel

* To compile the entire kernel, use: `make -j$(nproc)`. The `-j$(nproc)` flag utilizes all available CPU cores for faster compilation.
* To compile kernel modules separately, use: `make modules`.

### 4. Installation

* To install the compiled kernel image: `make install`.
* This command copies the kernel executable to the /boot directory (or a location specified in your configuration).
* The bootloader configuration might be updated to recognize the new kernel.
* To install the compiled kernel modules: `make modules_install`.
* This command installs the modules built earlier with `make modules` into designated system directories.

---

## Kernel Modules

* Kernel modules are code sections that can be loaded dynamically into the running kernel.
* They provide additional functionalities or support for specific hardware.
* Compiled modules are typically placed in a dedicated directory named modules for better organization
  (/lib/modules/KERNEL_VERSION).

---

## Finding Drivers

* Use `dmesg` to view kernel boot messages for driver loading info.
* Run `lsmod` to list currently loaded kernel modules (modules not actively in use won't be listed, even if they exist in the system).
* Use `lspci`, `lsusb`, or `lshw` to gather details like vendor, model, and bus type.
* Look for files related to drivers or hardware within the `Documentation` directory of the kernel source.
* Exploring the kernel source code can reveal information about included drivers. Look for directories containing driver code specific to hardware types  

---

## Initramfs or Initrd

<TODO>

---

## Debugging

<TODO>

---

## Performance

<TODO>

---

## Power Management

<TODO>

---

## Hardware Virtualization

<TODO>

---

## Kernel Hardening

<TODO>

---

## Networking Configuration

<TODO>

---

## Benchmarking

<TODO>
