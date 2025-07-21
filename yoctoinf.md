Poky Source Directory Overview (Yocto Project)

The Poky source tree includes several important directories and files. Here's what each does:

Ὄ1 bitbake/

Contains all the Python scripts used by BitBake (the build engine).

bitbake/bin is added to the system PATH so that the bitbake command can be executed.

Includes:

Bitbake parser

Task scheduler

Fetchers and UI handlers

Ὄ1 documentation/

Contains all source files used to generate official Yocto Project documentation.

Can be built into PDF or HTML manuals.

Written in reStructuredText (.rst) format using Sphinx.

Ὄ1 meta/

Contains the OpenEmbedded-Core (OE-Core) metadata.

Includes core recipes, classes, and configuration files.

Provides:

Compiler tools (gcc, binutils)

Base libraries and packages

Core image definitions

Ὄ1 meta-poky/

Holds configuration for the Poky reference distribution.

Defines distro-level settings like poky.conf.

Adds on top of meta/ to form a working reference distro.

Ὄ1 meta-skeleton/

A template layer for creating new recipes, BSPs, and kernel configurations.

Good for developers starting their own layers.

Ὄ1 meta-yocto-bsp/

Maintains Board Support Packages (BSPs) for:

BeagleBone

EdgeRouter

Generic IA (x86) 32-bit and 64-bit machines

Contains machine configuration, device trees, and kernel settings.

Ὄ1 scripts/

Holds helper scripts for:

Setting up the build environment (oe-init-build-env)

Creating layers and BSPs

Flashing images to target boards

Ὄ4 LICENSE

The license under which Poky is distributed (usually MIT).

Also includes licenses for components from other projects.

