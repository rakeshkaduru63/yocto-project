# BitBake and Yocto Project Concepts

## What is BitBake?
BitBake is a build engine that performs tasks such as:
- Fetching
- Unpacking
- Compiling
- Packaging
- Generating final images

It reads **recipes** and generates the final system image.

Key Configuration Files:
- `local.conf`
- `bblayers.conf`
- `bin/` → Library part

---

## What is Metadata?
Metadata refers to build instructions and includes:
- Configuration files
- Recipes (`.bb`, `.bbappend`)
- Classes (`.bbclass`)
- Includes (`.inc`)

---

## What are Recipes?
A **recipe** is a set of instructions that BitBake reads and processes.

It defines how to fetch, configure, compile, install, and package software.

---

## What are Configuration Files?
Files that hold:
- Global variable definitions
- User-defined variables
- Hardware configuration information

---

## What are Layers?
A **layer** is a collection of related recipes.

Types of Layers:
- Predefined layers
- User-defined layers

### Why Layers?
Layers provide a mechanism to isolate metadata based on functionality.
Examples:
- Distro layer → `meta-poky`
- BSP layer → `meta-raspberrypi`
- Middleware layer → `meta-oe`
- UI layer → `meta-vt5`
- Custom layers

**Dropbear**: Directly connects the board over Wi-Fi.

---

## Which Layers are Used by the Poky Build System?
`BBLAYERS` variable in `build/conf/bblayers.conf` lists the layers used by BitBake.

---

## Yocto Project Compatible Layers
These layers are tested and fully compatible with the Yocto Project.

---

## What is a Class?
Class files (`.bbclass`) abstract common functionality for reuse across multiple recipes.

To use a class, the recipe must **inherit** it.

### Examples:
- `cmake.bbclass`: Handles CMake in recipes
- `kernel.bbclass`: Handles kernel building and tree management
- `module.bbclass`: Supports building out-of-tree Linux kernel modules

---

## What is a Package?
A **package** is a binary file (e.g., `.rpm`, `.deb`, `.ipk`).

A single recipe can produce many packages.

All packages from a recipe are listed in the `PACKAGES` variable.

# 📁 Poky Source Directory Overview

The Poky source tree includes several important directories and files. Here's what each does:

## 📁 bitbake/
Contains all the Python scripts used by BitBake (the build engine).

- `bitbake/bin` is added to the system PATH so that the `bitbake` command can be executed.

Includes:
- BitBake parser
- Task scheduler
- Fetchers and UI handlers

## 📁 documentation/
Contains all source files used to generate official Yocto Project documentation.

- Can be built into PDF or HTML manuals.
- Written in reStructuredText (`.rst`) format using Sphinx.

## 📁 meta/
Contains the OpenEmbedded-Core (OE-Core) metadata.

Includes core recipes, classes, and configuration files.

Provides:
- Compiler tools (`gcc`, `binutils`)
- Base libraries and packages
- Core image definitions

## 📁 meta-poky/
Holds configuration for the Poky reference distribution.

- Defines distro-level settings like `poky.conf`.
- Adds on top of `meta/` to form a working reference distro.

## 📁 meta-skeleton/
A template layer for creating new recipes, BSPs, and kernel configurations.

- Good for developers starting their own layers.

## 📁 meta-yocto-bsp/
Maintains Board Support Packages (BSPs) for:
- BeagleBone
- EdgeRouter
- Generic IA (x86) 32-bit and 64-bit machines

Contains machine configuration, device trees, and kernel settings.

## 📁 scripts/
Holds helper scripts for:
- Setting up the build environment (`oe-init-build-env`)
- Creating layers and BSPs
- Flashing images to target boards

## 📄 LICENSE
The license under which Poky is distributed (usually MIT).

- Also includes licenses for components from other projects.

