# Linux Engineering

This repository contains **learning-focused Linux engineering projects** related to
Linux kernel fundamentals, kernel module programming, and low-level system building
using **Linux From Scratch (LFS)** and **Gentoo Linux**.

The purpose of this repository is to demonstrate hands-on understanding of:
- Linux kernel concepts
- Kernel module lifecycle
- Linux boot process
- Kernel ↔ user-space boundaries
- UNIX-like operating system internals

This repository is intended as a **junior/entry-level Linux kernel engineering portfolio**.

---

## Repository Overview

The contents of this repository fall into the following categories:

### 1. Kernel Module Programming

This section contains **small, testable Linux kernel modules** written in C.

These modules are designed to:
- Demonstrate correct use of `module_init()` and `module_exit()`
- Show safe module load and unload behavior
- Use kernel logging (`pr_info`, `dmesg`)
- Illustrate interaction between user space and kernel space
- Follow basic kernel coding conventions

These modules are **not production drivers**, but educational implementations intended
to build a solid foundation for Linux kernel development.

---

### 2. Linux From Scratch (LFS) Bootstrap Files

This repository includes **bootstrap scripts and configuration notes** related to
building Linux systems from source using **Linux From Scratch (LFS)**.

Topics covered include:
- Toolchain bootstrap
- Kernel configuration and compilation
- Early system setup
- Understanding init, initramfs, and early boot
- Root filesystem preparation

These files reflect practical experience with **manual Linux system construction**
and deep exposure to system internals.

---

### 3. Gentoo Linux Configuration & Build Files

This section contains configuration files and notes used when working with **Gentoo Linux**.

Focus areas include:
- Custom kernel configuration
- Minimal vs generic kernel setups
- Boot configuration and troubleshooting
- Understanding modular vs built-in kernel components
- Performance and system specialization trade-offs

---

## Supported / Tested Environments

The code and configuration files in this repository have been tested or used with:

### Linux Distributions
- Linux From Scratch (LFS)
- Gentoo Linux
- Ubuntu
- Debian
- Fedora
- Arch Linux

### BSD Systems (for comparison & learning)
- FreeBSD
- DragonFly BSD
- OpenBSD
- NetBSD

---

## Build & Test (Kernel Modules)

> **Note:** Kernel modules must be built against the running kernel headers.

Typical build workflow:
```bash
make
sudo insmod module_name.ko
dmesg | tail
sudo rmmod module_name
```
## Learning Goals

This repository focuses on:

- Understanding Linux kernel fundamentals
- Developing confidence reading kernel source code
- Practicing safe kernel programming habits
- Preparing for entry-level Linux kernel engineering roles
- Bridging the gap between Linux system usage and kernel development

## Disclaimer

The projects in this repository are educational.

They are not intended for production use.

The code prioritizes clarity and correctness over performance or feature completeness.

## Author

Sayed Mohammad Badiezadegan
Linux & Kernel Engineering Enthusiast

GitHub: https://github.com/mbzadegan

## Motivation

This repository was created as part of a structured effort to transition from
advanced Linux system usage (LFS, Gentoo, BSDs) into Linux kernel development,
with a focus on correctness, learning, and open-source best practices.
