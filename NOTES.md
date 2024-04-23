# Linux Kernel Notes

---

## Linux Kernel Build Process

This guide outlines the steps involved in building a custom Linux kernel:

### 1. Downloading the source code

The official website for obtaining Linux kernel source code is: https://www.kernel.org/.

### 2. Kernel Configuration

* The kernel configuration file, .config, resides in the root directory of the source tree.
* While manual editing is possible, it's recommended to use dedicated tools for better usability.
* `make menuconfig` or `make nconfig` (curses interface) simplifies configuration.
* Alternatively, use xconfig or gconfig for graphical interfaces.
* During configuration, you choose which features to compile directly into the kernel and which ones will be loaded as modules later.
* You can search a configuration in the curses interface by typing `/`.

### 3. Compiling the Kernel

* To compile the entire kernel, use: `make -j$(nproc)`. The `-j$(nproc)` flag utilizes all 
  available CPU cores for faster compilation. Be sure to check temperatures and adjust this variable as needed.
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
* `make localmodconfig` creates minimal custom kernel config based on loaded modules for faster build times.
* `modprobed-db` is a tool that serves as a database to manage linux kernel modules, in order to reduce compile
  times and bloat. After installing modprobed-db, you will use the command `make LSMOD=$HOME/.config/modprobed.db 
  localmodconfig` to build a kernel with the modules in the database. For more: https://github.com/graysky2/modprobed-db

---

## Finding Drivers

* Use `dmesg` to view kernel boot messages for driver loading info.
* Run `lsmod` to list currently loaded kernel modules (modules not actively in use won't be listed, even if they exist in the system).
* Use `lspci`, `lsusb`, or `lshw` to gather details like vendor, model, and bus type.
* Look for files related to drivers or hardware within the `Documentation` directory of the kernel source.

---

## Initramfs or Initrd

* Initramfs/Initrd is the temporary root filesystem loaded during boot.
* Kernel loads Initramfs/Initrd early in the boot process.
* Initramfs/Initrd provides essential drivers and tools needed to access and mount the real root filesystem.
* Once mounted, the kernel hands over control to the real root FS for full system startup.
* If your system uses very basic hardware with drivers already compiled directly into the kernel image (vmlinuz), 
  it might be possible to boot without an initramfs.

---

## Debugging

* Use `dmesg` to view kernel log messages, potentially including details about errors or warnings during boot or system operation.
* Enable KGDB during kernel compilation (CONFIG_KGDB=y), which allows connecting a debugger (like GDB) to the running kernel.

---

## Performance and Power Management

* Choose the right CPUFreq governor for your needs. `powersave`, `performance`, etc.
* Enable deeper CPU idle states (C-states) like C3 or C6 for lower power consumption during idle periods (search for idle= options).
* Consider enabling workqueue power efficiency (workqueue.power_efficient=1) for potential power savings on laptops (might impact performance slightly).
* Unload unused kernel modules (lsmod to see loaded modules, rmmod to unload). This frees up memory and reduces overhead.
