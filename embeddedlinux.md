
# Embedded Linux Architecture Overview

## 📦 System Images (Loaded into Embedded System)
These are essential components required to boot and run Linux on a target board (e.g., Raspberry Pi):

### ◼ Bootloader Image (e.g., U-Boot)
- First component to run after power-on/reset.
- Initializes hardware (CPU, RAM).
- Loads the kernel image and device tree into memory.
- Passes control to the kernel.

### ◼ Devicetree
- Describes the hardware layout to the kernel (CPU, memory, peripherals).
- Example: tells the kernel what devices exist and how to access them.

### ◼ Linux Kernel Image
- Core of the OS that manages hardware, processes, memory, etc.
- Communicates with hardware through drivers.
- Provides system calls for applications.

### ◼ File System Image (rootfs + partitions)
- Contains user-space software: C library, BusyBox, applications, config files.
- This is the root filesystem mounted by the kernel after boot.

---

## 🧠 Logical Architecture Breakdown
Explains how system components are layered in memory and execution:

### 🔴 Bootloader
- Initializes system and jumps to the kernel.

### 🔷 Kernel Space
- **System Call Interface**: Gateway for user-space programs to access kernel features (e.g., open, read, write).
- **Core Kernel Components**:
  - **Network Stack**: Manages TCP/IP, routing, Wi-Fi/Ethernet.
  - **Virtual Filesystem**: Supports filesystems like ext4, FAT via common API.
  - **Process Management**: Handles creation, scheduling, termination.
  - **Memory Management Unit (MMU)**: Manages memory, virtual-to-physical address mapping.
  - **IPC**: Mechanisms like pipes, message queues, shared memory.
  - **Architecture Dependent Code**: Code for specific CPU architectures (ARM, x86).
  - **Device Drivers**: Interface between kernel and hardware (USB, Wi-Fi, etc.).

### 🟡 User Space
Where your applications and tools run:

- **C Library (libc)**: Provides standard functions (e.g., printf, malloc, open). Bridges apps to system calls.
- **Other Libraries**: GUI, networking, and more.
- **BusyBox**: Collection of Unix utilities in a single small executable.
- **Services & Desktop Manager**: Background services (networking, logging), GUI components.
- **Applications**: Custom or user-installed software.

---

## 🔄 Boot Flow
```
Power On
  ↓
Bootloader (e.g., U-Boot)
  ↓
Loads Kernel Image + Devicetree + File System
  ↓
Kernel Initializes Drivers, Memory, Filesystem
  ↓
Mounts Root Filesystem
  ↓
Starts init process
  ↓
Starts C library, Services
  ↓
Runs Applications
```

---

## ✅ Summary Table
| Component     | Purpose                             |
|--------------|-------------------------------------|
| Bootloader   | Start system, load kernel           |
| Devicetree   | Tell kernel about hardware          |
| Kernel       | Manage hardware, memory, processes  |
| Filesystem   | Store apps, configs, libraries      |
| User Space   | Run applications and services       |


