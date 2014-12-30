---
layout: post
title: Notes about linux kernel development
category: articles
---

I am getting back into linux kernel development
after a pretty long hiatus.
My muscle memory is gone, 
and while I wrote clever, very close to the hardware kernel modules,
they were never sophisticated in their interaction with userland.

These are notes about learning linux kernel development using 
Dr. Jerry Cooperstein's book "Writing Linux Device Drivers - a guide with exercises".

## Setting up the development environment

I set up a simple development environment using qt creator, 
in a parallels virtual machine running Ubuntu 14.04.
I added the kernel headers to the project's includes file:

	/lib/modules/3.11.0-26-generic/build/include

I added the `__KERNEL__` define to the project's config file.
This way, QT creator knows to chose the right header code when autocompleting.

I found that QT creator reacts weirdly on some of the kernel macros,
but haven't a workaround yet. 
The kernel code is heavy on macros.
Despite that, QT creator remains very nice to use, 
as it autocompletes most of the useful functions,
and allows me to quickly jump to the definition of a function I don't know.

I compile and load the modules from the command-line, 
entirely manually for now.
A more verbose compilation (with the command line for gcc calls) can be shown with:

	KBUILD_VERBOSE=1 make

I used a very standard kernel module makefile:

	ccflags-y := -Werror
	
	obj-m += ldd-trivial.o
	# ldd-objs := ldd_main.o ldd_file.o ldd_rw.o ldd_init.o
	
	KDIR := /lib/modules/$(shell uname -r)/build
	PWD := $(shell pwd)
	
	all: default
	
	clean:
		rm -f *.o *.ko
	
	default:
		$(MAKE) -C $(KDIR) SUBDIRS=$(PWD) modules

I don't like having very long C files, 
so I also included the (commented out) line to define multi-file modules.
The linux kernel has absurdly long device driver source files (3000+ lines),
which I am not comfortable with when writing code.

I check in my exercise results in a private git, 
and use the following `.gitignore` file:

	*.o
	*.ko
	modules.order
	.tmp_versions
	*.cmd
	*.mod.c
	Module.symvers

## First modules

The first module in the book is the trivial load/unload and printk example.
Not much to see here.

### Adding module parameters

The next step is to add module parameters.
I am not sure how useful this will turn out to be, 
but I am going to use this as a jumping board for some exploration of the headers.

It is indeed surprisingly easy to add 
typechecked named parameters using the `module_param_*` family of macros.
The defined parameters 
(which should be named the same as the variable they are going to set)
are then accessible as command line arguments when loading the module,
or through the `/sys/module/` filesystem.

There are a lot of `MODULE_*` macros which can be used to add interesting metadata.
The following caught my interest:

- `MODULE_SUPPORTED_DEVICE`
- `MODULE_FIRMWARE`
- `MODULE_VERSION`

This information can then be queried by running the `modinfo` command:

	parallels@ubuntu:~/ldd$ modinfo ldd-trivial.ko
	filename:       /home/parallels/code/wesen/wesen-misc/linux/ldd/ldd-trivial.ko
	license:        GLP v2
	author:         A GENIUS
	srcversion:     C030D2956A00B85FD276DB5
	depends:
	vermagic:       3.11.0-26-generic SMP mod_unload modversions
	parm:           foobar:int

### Creating a device file

A character device number can be allocated dynamically with `alloc_chrdev_region`.
The resulting character device number can be found in `/proc/devices`.

	$ grep cdrv /proc/devices
	251 mycdrv

Device file handling is now done by the `udev` daemon.
The kernel driver now registers what is called a `class` with the kernel.
The kernel then ppulates the device class and device information into `/sys`.
The `udev` daemon picks up on this information, and creates device files accordingly.
This allows userland to tune the permissions, names and types of device files.

Using the functions `class_create` followed by `device_create`, 
a new device entry is signalled to `udev`. 
The `udev` then consults its intricate set of rules 
(under `/etc/udev/rules.d` in ubuntu) 
and creates a device file with the appropriate major and minor node numbers
and the appropriate permissions.

Here is an entry to make usb module world read/writable:

	SUBSYSTEM=="usb", ATTRS{idVendor}=="ffff", TAG+="udev-acl", TAG+="uaccess", MODE="0666"

### Adding file operations

You add file operations for your device file using a `struct file_operations`.

The book mentions the `ioctl` entry, 
which has been deprecated in favor of `unlocked_ioctl` and `compat_ioctl`.

`ioctl` is this funky little way of doing IPC in an unchecked way.
You pass it an integer along with another integer argument, and off you go.
`ioctl` runs under the Big Kernel Lock, 
which can lead to stalls in completely different parts of the kernel when an ioctl takes too long. There is now an `unlocked_ioctl` that runs outside the BKL, 
and requires the driver to use its own locking.
This function is preferred over the `compat_ioctl` call which still runs under the BKL.

