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

