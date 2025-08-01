
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



# 📦 Embedded Linux: System Images & Custom Software Development

## 🧩 Overview

Embedded Linux development involves creating several system images (binaries) that work together to boot and run Linux on target hardware like the Raspberry Pi.

---

## 🔧 Development Flow

```
Source Code → Cross-Compilation → Image Generation → Flash to Board
```

### 🔨 Toolchain

- Compiler + Tools for target architecture (e.g., `arm-linux-gcc`)
- Converts source code into binaries for the target system.

### 🏗️ Build System

- Examples: **Yocto**, **Buildroot**
- Automates image creation:
  - Bootloader
  - Kernel
  - Root filesystem
  - Firmware

---

## 📦 Types of Images

### 1. Bootloader Image

- **Example**: U-Boot
- **Function**:
  - Initializes CPU, RAM, clocks.
  - Loads the kernel and device tree into memory.
  - Passes control to the Linux kernel.
- **Note**: Some boards (e.g., Raspberry Pi 5) include a primary bootloader in EEPROM or ROM.

### 2. Linux Kernel Image

- **Format**: `zImage`, `uImage`, or `Image`
- **Function**:
  - Manages hardware, memory, and processes.
  - Offers system calls to user space.
  - Loads device drivers.

### 3. Root Filesystem (rootfs) Image

- **Contents**:
  - BusyBox, shell, utilities
  - `/bin`, `/lib`, `/etc`, `/usr`, etc.
  - Custom applications and services
- **Format**: `ext4`, `squashfs`, `cpio`

### 4. Firmware Binaries

- **Purpose**: Provide support for peripherals (e.g., Bluetooth, Wi-Fi)
- **Files**: Typically `.bin` (binary-only)
- **Example**: Bluetooth firmware blob for Raspberry Pi

---

## 🔁 Boot Sequence

```
Power ON
  ↓
Primary Bootloader (ROM or EEPROM)
  ↓
Secondary Bootloader (e.g., U-Boot)
  ↓
Load Kernel Image + Devicetree
  ↓
Mount Root Filesystem
  ↓
Start Init Process → Start Services & Apps
```

---

## 🗂️ Source Code Breakdown

| Component               | Source Path                      |
|------------------------|----------------------------------|
| Bootloader (U-Boot)    | `u-boot/`                        |
| Kernel Source Code     | `linux/`                         |
| User Apps & Libraries  | `project/app`, `3rdparty/libs`  |
| Firmware Binaries      | Provided externally (`*.bin`)   |

---

## ✅ Summary Table

| Image Type      | Purpose                                  |
|-----------------|-------------------------------------------|
| Bootloader      | Initialize and load kernel               |
| Linux Kernel    | Manage hardware, provide system calls    |
| Root Filesystem | Provide userspace environment            |
| Firmware        | Support specific peripherals             |

---

## 💡 Notes

- Flashing is done to **SD card**, **eMMC**, or other storage.
- All images are generated using **cross-compilation** on a host PC (e.g., Ubuntu).
- **Devicetree** is typically loaded by the bootloader along with the kernel.

# 🧠 Central Component: Processor

The processor (typically ARM in embedded systems like Raspberry Pi) is the brain of the system.

It directly communicates with all hardware components through buses and controllers.

---

# 🧩 Connected Hardware Components

## 1. Memory (RAM)
- Volatile storage used by the processor to run the operating system and applications.
- Connected directly to the processor with a high-speed memory bus.

## 2. Storage (Non-Volatile)
- Types: **eMMC**, **Flash**, **SD Card**
- Used to store:
  - Bootloader
  - Kernel image
  - Root filesystem
- Content persists after power-off.

## 3. Power Regulators & PMIC
- Power Management IC (PMIC) controls power distribution.
- Ensures proper voltage levels to all parts of the board.

## 4. I2C Bus Controller
- Used to communicate with low-speed peripherals (sensors, RTC, etc.)
- I2C: Two-wire protocol (SCL, SDA)

## 5. SPI Controller
- Serial Peripheral Interface for fast communication with devices like flash memory, display drivers.
- SPI: Four-wire protocol (MOSI, MISO, SCLK, SS)

## 6. GPIO Controller & Mux
- General Purpose Input/Output
- Used for buttons, LEDs, and custom I/O
- GPIO pins can be multiplexed for different functions.

## 7. Ethernet Controller
- Handles network data.
- Connected to the Ethernet port for LAN/WAN connectivity.

## 8. HDMI Controller
- Sends video signals to HDMI port (for display/monitor).

## 9. Sound Codec Chip
- Converts digital audio signals to analog (for headphones, speakers)
- Often works with I2S interface.

## 10. USB Controller
- Controls USB communication for:
  - USB keyboards, mice
  - Flash drives
  - Cameras

## 11. PCIe Controller
- Connects high-speed peripherals like Wi-Fi cards, NVMe SSDs.

## 12. Wi-Fi / Bluetooth Module
- May be connected via PCIe, SDIO, or USB.
- Enables wireless communication.

---

# 🔁 Summary Flow

```
[Power ON]
   ↓
[Processor initializes via Bootloader]
   ↓
[Processor loads Kernel from Storage (via Bus Controller)]
   ↓
[Kernel accesses Memory, Devices via I2C, SPI, GPIO, etc.]
   ↓
[User-space apps run using hardware interfaces]
```
# 🐧 How Linux Boots – Simplified Explanation

---

## ⚡️ 1. Power Up

- Electrical power is supplied to the system.
- CPU and other components (RAM, storage, peripherals) receive voltage.
- The CPU starts executing code from **Boot ROM** (typically stored in SoC).
- This code usually initializes the **Bootloader**.

---

## 🧰 2. Bootloader Stage

Bootloader is typically divided into two parts:

### 🔹 Primary Bootloader (Stage 1)

- Resides in ROM or the first sector of storage.
- Initializes core components:
  - CPU
  - Minimal RAM setup
  - Basic clock, etc.

### 🔸 Secondary Bootloader (Stage 2)

- Loaded by the primary bootloader into RAM.
- More capable — initializes:
  - Full RAM
  - Storage devices (eMMC, SD card, NAND/NOR Flash)
  - Optional peripherals (PCIe, Ethernet, GPU)
- Loads the **Linux kernel** into RAM
- Loads **Device Tree Blob (DTB)** — describes hardware layout
- Optionally loads:
  - **GPU firmware**
  - **initrd/initramfs** (RAM disk)
- Finally, jumps to the **kernel's entry point**

---

## 🧠 3. Linux Kernel Stage

- Starts running in protected mode (ARM or x86 depending on architecture).
- Key tasks:
  - Mount `initrd` (optional temporary root filesystem)
  - Initialize kernel subsystems:
    - Memory management
    - Scheduler
    - VFS (Virtual File System)
    - IPC (Inter-Process Communication)
  - Load drivers using information from **Device Tree**
  - Starts the `init` process (PID 1)

---

## 🧱 4. Linux Userspace

- `init` system (e.g., **systemd**, **SysVinit**, or **upstart**) takes over.
- Reads system configuration files (`/etc/`)
- Launches:
  - Services (network, display manager, logging)
  - Shell or desktop environment
  - User applications

---

## 🟢 5. Application Ready

- After services are initialized, applications can run.
- System is fully operational.
- Total boot time varies depending on:
  - Optimizations
  - Bootloader/kernel size
  - Hardware speed

---

## ⏱️ Example Boot Time Flow

| Stage           | Time    | Notes                          |
|------------------|---------|--------------------------------|
| Power Up         | ~1s     | Depends on hardware startup    |
| Bootloader       | ~2-3s   | Includes kernel + DTB loading  |
| Kernel Init      | ~1-2s   | Mount, driver init, etc.       |
| Userspace Init   | ~5-10s  | Depends on init system         |
| **Total**        | ~8-15s  | Can be reduced with tuning     |

---

## 🔧 Notes for Optimization

- Disable unused kernel drivers.
- Use minimal `initramfs`.
- Parallelize service startup (e.g., with `systemd`).
- Use fast storage (eMMC over SD).
- Optimize bootloader configuration (e.g., U-Boot script).

## 🧠 How Raspberry Pi Boots from SD Card

### 🔹 1. First Stage – EEPROM (ROM Bootloader)
Raspberry Pi has a small ROM in EEPROM.

This stage initializes hardware and loads the second-stage bootloader from the SD card (`bootcode.bin`).

### 🔹 2. Second Stage – `bootcode.bin`
Found in the boot partition (FAT32).

Loads a GPU firmware binary called `start.elf`.

### 🔹 3. Third Stage – `start*.elf`
Initializes the GPU firmware.

Reads `config.txt` (user-defined settings).

Then optionally launches **U-Boot** (if configured).

### 🔹 (Optional) U-Boot
A flexible bootloader used in embedded systems.

Loads:
- `boot.scr` (boot script file)
- `kernel8.img` (ARM64 kernel image)

### 🔹 4. Kernel Boot
After U-Boot (or directly from `start.elf`), the kernel image is loaded.

Kernel then mounts the root filesystem (`rootfs`).

### 🔹 Final: Dom0 (Linux OS User Space)
Kernel passes control to user space.

Loads `init` process → full OS boots.

---

### 🔧 Files You’ll See on the Boot Partition
- `bootcode.bin` → second stage
- `start.elf`, `start4.elf`, etc. → third stage GPU firmware
- `config.txt` → boot configuration
- `cmdline.txt` → kernel boot parameters
- `kernel8.img` → ARM64 kernel
- `boot.scr` → optional U-Boot script


