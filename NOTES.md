# Linux Kernel Notes

---

## Linux Kernel Build Process

This guide outlines the steps involved in building a custom Linux kernel:

### 1. Downloading the source code

* The official website for obtaining Linux kernel source code is: https://www.kernel.org/.

* Verify it using the associated PGP signature.

### 2. Kernel Configuration

* The kernel configuration file, .config, resides in the root directory of the source tree.

* While manual editing is possible, it's recommended to use dedicated tools for better usability.

* `make menuconfig` or `make nconfig` (curses interface) simplifies configuration.

* Alternatively, use xconfig or gconfig for graphical interfaces.

* During configuration, you choose which features to compile directly into the
  kernel and which ones will be loaded as modules later.

* You can search a configuration in the curses interface by typing `/`.

### 3. Compiling the Kernel

* To compile the entire kernel, use: `make -j$(nproc)`. The `-j$(nproc)` flag 
  utilizes all available CPU cores for faster compilation. Be sure to check 
  temperatures and adjust this variable as needed.

* To compile kernel modules separately, use: `make modules`.

### 4. Installation

* To install the compiled kernel modules: `make modules_install`. This command 
  installs the modules built earlier with `make modules` into designated system directories.

* To install the compiled kernel image: `make install`. This copies the kernel 
  executable to the /boot directory (or a location specified in your configuration).

* You may have to change the names of the kernel image at `/boot`.

* If you have compiled dynamic kernel drivers (modules) then you will need to 
  generate a initramfs for the kernel, using tools like `dracut` or `mkinitcpio`.

* Finally, if needed, update your bootloader configuration to detect the new 
  image. In the case of grub run: `update-grub` or `grub-mkconfig -o /boot/grub/grub.cfg`.

---

### 5. Updating the kernel

* To update to a newer kernel version just copy your old `.config` file to the root
  of the new kernel version and do `make oldconfig` to update the config file to match the newer version.

* `make oldconfig` will ask the user for new configuration options. If you just want
  to set all the options to their default without being asked do `make olddefconfig`.

---

## Kernel Modules

* Kernel modules are code sections that can be loaded dynamically into the running kernel.

* They provide additional functionalities or support for specific hardware.

* Compiled modules are typically placed in a dedicated directory named modules for better organization
  (/lib/modules/KERNEL_VERSION).

* `make localmodconfig` creates minimal custom kernel config based on loaded modules for faster build times.

* `modprobed-db` is a tool that serves as a database to manage linux kernel modules,
  in order to reduce compile times and bloat. After installing modprobed-db, you
  will use the command `make LSMOD=$HOME/.config/modprobed.db localmodconfig` to
  build a kernel with the modules in the database. For more: https://github.com/graysky2/modprobed-db

---

## Finding Drivers

* Use `dmesg` to view kernel boot messages for driver loading info.

* Run `lsmod` to list currently loaded kernel modules (modules not actively in
  use won't be listed, even if they exist in the system).

* Use `lspci`, `lsusb`, or `lshw` to gather details like vendor, model, and bus type.

* Use `lspci -nk` to display kernel drivers used by PCI devices.

* Use `lsusb -t` to display kernel drivers used by USB devices.

* Use `ls -l /sys/dev/*/*/device/driver` to try and grab more device drivers information.

* Look for files related to drivers or hardware within the `Documentation` directory of the kernel source.

---

## Initramfs or Initrd

* Initramfs/Initrd is the temporary root filesystem loaded during boot.

* Kernel loads Initramfs/Initrd early in the boot process.

* Initramfs/Initrd provides essential drivers and tools needed to access and mount the real root filesystem.

* Once mounted, the kernel hands over control to the real root FS for full system startup.

* If your system uses very basic hardware with drivers already compiled directly 
  into the kernel image (vmlinuz), it might be possible to boot without an initramfs.

---

## Generating initramfs

* Using dracut: Example: dracut initramfs-6.9.9.img 6.9.9

* For more information `man dracut`; https://man.voidlinux.org/dracut.8

* Using mkinitcpio: Example: mkinitcpio --config /etc/mkinitcpio-custom.conf --generate /boot/initramfs-custom.img --kernel 5.7.12-arch1-1

* For more information `man mkinitcpio`; https://wiki.archlinux.org/title/mkinitcpio

---

## Debugging

* Use `dmesg` to view kernel log messages, potentially including details about 
  errors or warnings during boot or system operation.

* Enable KGDB during kernel compilation (CONFIG_KGDB=y), which allows connecting 
  a debugger (like GDB) to the running kernel.

---

## Performance and Power Management

* Choose the right CPUFreq governor for your needs. `powersave`, `performance`, etc.

* Enable deeper CPU idle states (C-states) like C3 or C6 for lower power consumption 
  during idle periods (search for idle= options).

* Consider enabling workqueue power efficiency (workqueue.power_efficient=1) for 
  potential power savings on laptops (might impact performance slightly).

* Unload unused kernel modules (lsmod to see loaded modules, rmmod to unload). This 
  frees up memory and reduces overhead.

* For more: https://wiki.gentoo.org/wiki/Power_management/Processor
            https://wiki.gentoo.org/wiki/Power_management/Guide

---

## Intel Xe Graphics

* To try the new experimental driver you need: linux >=6.8, tigerlake integrated
  driver or newer or discrete intel graphics and mesa built with -Dintel-xe-kmd=enabled.

* Mesa 24.1 already comes with Intel Xe enabled by default.

* Then, run `lspci -nn | grep VGA` and add to your kernel parameters with the correct
  PCI ID: `i915.force_probe=!9a49 xe.force_probe=9a49`. For grub you can do it
  under /etc/defaults/grub in the GRUB_CMDLINE_LINUX_DEFAULT variable.

* Add the following modules to your initramfs, either by dracut or by iniramfs:
  `add_drivers+=" xe gpu_sched drm_suballoc_helper drm_gpuvm drm_exec "`.

* NOTE: If you want to use the Xe drivers and not the i915, then the i915 drivers
  cannot be STATICALLY compiled into the kernel, either don't compile them (risky)
  or compile them as a kernel module.

* Intel Xe graphics do not seem to be able to be compiled statically into the kernel.

* Intel Xe graphics driver modules:
```c
  SYMLINK /lib/modules/6.9.9/build
  INSTALL /lib/modules/6.9.9/modules.order
  INSTALL /lib/modules/6.9.9/modules.builtin
  INSTALL /lib/modules/6.9.9/modules.builtin.modinfo
  INSTALL /lib/modules/6.9.9/kernel/drivers/gpu/drm/drm_exec.ko
  SIGN    /lib/modules/6.9.9/kernel/drivers/gpu/drm/drm_exec.ko
  INSTALL /lib/modules/6.9.9/kernel/drivers/gpu/drm/drm_gpuvm.ko
  SIGN    /lib/modules/6.9.9/kernel/drivers/gpu/drm/drm_gpuvm.ko
  INSTALL /lib/modules/6.9.9/kernel/drivers/gpu/drm/drm_suballoc_helper.ko
  SIGN    /lib/modules/6.9.9/kernel/drivers/gpu/drm/drm_suballoc_helper.ko
  INSTALL /lib/modules/6.9.9/kernel/drivers/gpu/drm/scheduler/gpu-sched.ko
  SIGN    /lib/modules/6.9.9/kernel/drivers/gpu/drm/scheduler/gpu-sched.ko
  INSTALL /lib/modules/6.9.9/kernel/drivers/gpu/drm/xe/xe.ko
  SIGN    /lib/modules/6.9.9/kernel/drivers/gpu/drm/xe/xe.ko
  DEPMOD  /lib/modules/6.9.9
```
