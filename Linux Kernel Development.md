# Linux Kernel Development – Entry-Level Guide

This document is a single, self-contained introduction to Linux kernel development for beginners.
It covers kernel vs user space, init and initramfs, booting without a distro, kernel modules vs
drivers, small starter projects, and a minimal testable kernel module.

This guide is suitable for learners coming from Linux From Scratch (LFS), Gentoo, or advanced Linux usage.

---

## 1. Kernel Space vs User Space

The Linux kernel runs in **kernel space** and is responsible for:
- Process scheduling
- Memory management
- Device control
- System calls

User programs run in **user space** and interact with the kernel only through
well-defined interfaces (system calls, `/proc`, `/sys`, device files).

The kernel itself does **not** include shells, services, or utilities.

---

## 2. What Is `init`?

`init` is the **first user-space process** started by the kernel.

- It always has **PID 1**
- It is **not part of the kernel**
- It is provided by the root filesystem or initramfs

Common implementations:
- `systemd` (Ubuntu)
- `OpenRC` or `systemd` (Gentoo)
- `BusyBox init` (minimal systems)

The kernel attempts to execute, in order:
1. Path provided by `init=` kernel parameter
2. `/sbin/init`
3. `/bin/init`
4. `/bin/sh`

If none exist, the kernel panics:
Kernel panic - not syncing: No init found


---

## 3. Is `init` Created During Kernel Compilation?

No.

Kernel compilation produces:
- `vmlinuz` (kernel image)
- kernel modules (`.ko`)
- optional initramfs

It does **not** produce:
- `/sbin/init`
- systemd
- BusyBox
- shells or utilities

Those belong to **user space**, not the kernel.

---

## 4. What Is initramfs?

`initramfs` is a temporary root filesystem loaded into RAM during boot.

It contains:
- `/init` (a small user-space program or script)
- essential tools
- optional kernel modules

Boot flow with initramfs:

Bootloader → Kernel → initramfs:/init → real rootfs → /sbin/init


Boot flow without initramfs:

Bootloader → Kernel → real rootfs → /sbin/init


---

## 5. Booting a Kernel With No Distro

A kernel **cannot run alone**. It must execute an `init` process.

“No distro” means:
- No package manager
- No system services
- Minimal user space

The minimal working setup is:
- Linux kernel
- BusyBox (static)
- Tiny initramfs with `/init`

This is commonly tested using **QEMU**.

---

## 6. Kernel Module vs Kernel Driver

### Kernel Module
- A loadable object (`.ko`)
- Inserted and removed at runtime
- Uses `module_init()` and `module_exit()`
- May or may not interact with hardware

### Kernel Driver
- Kernel code that controls hardware or kernel subsystems
- Often delivered as a module, but can be built-in
- Lives under `drivers/`, `fs/`, `net/`, etc.

Key relationships:
- Most drivers are modules
- Not all modules are drivers
- Drivers are functionality
- Modules are a delivery mechanism

---

## 7. Very Small Module Development Project

A good first module project:
- Demonstrates module lifecycle
- Uses kernel logging
- Accepts module parameters
- Loads and unloads cleanly
- No hardware required

This proves understanding of kernel module fundamentals.

---

## 8. Very Small Driver Development Project

Recommended first driver:
**Character device driver (virtual sensor)**

Features:
- Creates `/dev/vsensor0`
- Supports read/write
- Maintains kernel-side state
- Demonstrates real driver concepts

This is true driver development, not just a demo module.

---

## 9. Ubuntu vs Gentoo Kernel Config (High-Level)

| Aspect | Ubuntu Generic Kernel | Gentoo Custom Kernel |
|------|-----------------------|----------------------|
| Portability | Very high | Hardware-specific |
| Modules | Many | Fewer |
| Built-in drivers | Minimal | Common |
| Boots without initramfs | Rare | Common |
| Kernel size | Larger | Smaller |
| Optimization | Generic | Tuned |

Ubuntu prioritizes compatibility and support.
Gentoo prioritizes control and minimalism.

---

## 10. Simple Testable Kernel Module

### hello.c
```c
#include <linux/module.h>
#include <linux/init.h>
#include <linux/moduleparam.h>

static char *name = "world";
module_param(name, charp, 0644);
MODULE_PARM_DESC(name, "Name to greet");

static int __init hello_init(void)
{
    pr_info("hello: loaded. Hello, %s!\n", name);
    return 0;
}

static void __exit hello_exit(void)
{
    pr_info("hello: unloaded. Goodbye, %s!\n", name);
}

module_init(hello_init);
module_exit(hello_exit);

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Author");
MODULE_DESCRIPTION("Simple hello kernel module");
MODULE_VERSION("0.1");

Makefile

obj-m += hello.o

all:
	$(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	$(MAKE) -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

Build and Test

make
sudo insmod hello.ko name="Kernel"
dmesg | tail
sudo rmmod hello


## 11. Why This Knowledge Matters

This material demonstrates:

Correct kernel vs user-space understanding

Proper boot flow mental model

Safe kernel coding practices

Ability to write, load, debug, and unload kernel code

These are core expectations for entry-level kernel engineers.

## 12. Recommended Entry-Level Books

Linux Kernel Development — Robert Love

Linux Device Drivers — Corbet, Rubini, Kroah-Hartman

Understanding the Linux Kernel — Bovet & Cesati

Linux Kernel Programming — Kaiwan N Billimoria

Always combine reading with real kernel code and small projects.

## 13. Final Note

Kernel development is learned by:

Reading

Building

Breaking

Fixing

Repeating
