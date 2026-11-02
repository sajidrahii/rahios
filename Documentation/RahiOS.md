# RahiOS — System Architecture Specification

**Document ID:** RahiOS-ARCH-001  
**Version:** 0.2  
**Status:** Architecture Baseline — Proposed for Approval  
**Project:** RahiOS  
**Target Architecture:** x86-64  
**Kernel Language:** C + Assembly  
**Kernel Architecture:** Modular Monolithic  
**Initial Firmware:** UEFI  
**Development Host:** Windows + WSL2  

---

# 1. Architectural Vision

RahiOS will be an independently developed, portable emergency operating system.

Its kernel, bootloader, device drivers, memory management, filesystem interfaces, graphical environment, command-line interface, and recovery utilities will be independently developed.

The operating system will not use an existing Linux or Windows kernel.

It will follow established processor, firmware, hardware, and filesystem specifications.

# 2. Architecture Decisions

## ADR-001: Programming Languages

**Decision:** C and Assembly.

C will be the primary kernel development language.

Assembly will be used for:

- Initial processor entry.
- Low-level CPU operations.
- Interrupt entry and exit.
- Context switching.
- Processor-specific operations.
- Bootloader handoff.

The kernel will maintain its own minimal runtime rather than depending on a hosted C standard library.

A consistent ABI, calling convention, memory model, and coding standard will be established.

## ADR-002: Kernel Architecture

**Decision:** Modular Monolithic Kernel.

The kernel will operate in a privileged address space.

Its internal components will be separated into well-defined subsystems.

Major components:

- Kernel Core
- Memory Manager
- Process Manager
- Scheduler
- Interrupt Manager
- System Call Interface
- Device Driver Framework
- Storage Manager
- Virtual Filesystem
- Networking Stack

Initially, most kernel components will be statically linked.

Dynamic kernel module loading will be considered for a later release.

## ADR-003: Boot Architecture

**Decision:** UEFI First.

Initial target:

- x86-64 UEFI firmware.
- 64-bit kernel.
- UEFI Graphics Output Protocol.
- UEFI memory map.
- Independent RahiOS bootloader.

The bootloader will initialize the required execution environment, load the kernel, provide boot information, and transfer control.

The kernel will take responsibility for hardware and memory management after the UEFI boot-services handoff.

Legacy BIOS compatibility will be developed as a separate boot path in a later milestone.

## ADR-004: Development Environment

**Decision:** Windows Host with WSL2.

Windows will be the primary host operating system.

WSL2 will provide the Linux-based development environment.

Development capabilities will include:

- C compilation.
- Assembly.
- Linking.
- Debugging.
- Version control.
- Build automation.
- ISO generation.
- Virtual hardware testing.

QEMU will be the primary virtual machine testing platform.

Physical hardware testing will complement virtual testing.

## ADR-005: Graphical Environment

**Decision:** Fully Custom GUI Stack.

RahiOS will have its own graphical subsystem.

It will not depend on an existing desktop environment or window manager.

The GUI will include:

- Display management.
- Framebuffer management.
- Graphics rendering.
- Font rendering.
- Input handling.
- Window manager.
- Widget toolkit.
- Desktop environment.
- Application framework.

The initial graphics implementation will use a firmware-provided framebuffer interface.

The long-term GUI architecture will run in user space, with controlled communication through kernel interfaces.

## ADR-006: Initial Processor Architecture

**Decision:** x86-64.

The first architecture target will be 64-bit x86 processors.

Other processor architectures may be considered in future releases.

## ADR-007: GUI Execution Model

**Decision:** User-space GUI.

The graphical environment will run outside the privileged kernel address space.

This requires memory protection, process management, system calls, and inter-process communication to be sufficiently mature.

A temporary early-development diagnostic interface may operate before the full user-space environment exists.

## ADR-008: Kernel Component Loading

**Decision:** Static kernel components initially.

Kernel subsystems will be integrated into the initial kernel image.

Dynamic module loading may be introduced after the driver framework and security model mature.

---

# 3. High-Level System Architecture

The architecture consists of five major layers.

### Layer 1 — Firmware and Hardware

- UEFI firmware.
- CPU.
- RAM.
- Storage controllers.
- USB controllers.
- Display hardware.
- Input devices.
- Network hardware.

### Layer 2 — Boot Infrastructure

- RahiOS UEFI bootloader.
- Kernel loader.
- Boot information structures.
- Firmware memory-map handoff.

### Layer 3 — Kernel

- Kernel initialization.
- Memory management.
- Interrupt handling.
- Scheduling.
- Process management.
- System calls.
- Driver framework.
- Storage subsystem.
- VFS.
- Networking.

### Layer 4 — System Services

- Device management.
- Storage discovery.
- Filesystem management.
- Logging.
- Application services.
- Recovery services.
- Network services.

### Layer 5 — User Interface

- Graphical desktop.
- File manager.
- Recovery dashboard.
- Terminal.
- System diagnostics.
- Disk utilities.
- Recovery applications.

---

# 4. Boot Architecture Specification

The initial boot process will follow this sequence:

1. Computer firmware initializes hardware.
2. UEFI identifies the RahiOS boot entry.
3. RescueOS bootloader is loaded.
4. Bootloader validates and loads the kernel.
5. Required boot information is collected.
6. Memory map information is obtained.
7. Graphics information is obtained through GOP.
8. Boot services are exited.
9. Control transfers to the RahiOS kernel.
10. Kernel initialization begins.
11. Hardware discovery and subsystem initialization occur.
12. The system starts its initial user-space environment.

The boot architecture must support a clear separation between firmware responsibilities and kernel responsibilities.

---

# 5. Kernel Subsystem Specification

## 5.1 Kernel Core

Responsible for:

- Kernel initialization.
- Kernel configuration.
- Core interfaces.
- Panic handling.
- Logging.
- Subsystem coordination.

## 5.2 Memory Manager

Responsible for:

- Physical memory management.
- Virtual memory management.
- Page allocation.
- Kernel heap.
- Memory protection.
- Address-space management.

## 5.3 Process Manager

Responsible for:

- Process creation.
- Process termination.
- Thread management.
- Scheduling.
- Context switching.
- Process isolation.

## 5.4 Interrupt Manager

Responsible for:

- CPU exceptions.
- Hardware interrupts.
- Interrupt routing.
- Interrupt service routines.
- Timer interrupts.

## 5.5 Device Driver Framework

Responsible for:

- Hardware discovery.
- Device registration.
- Driver interfaces.
- Device communication.
- Device lifecycle management.

## 5.6 Storage Manager

Responsible for:

- Block devices.
- Storage controllers.
- Disk discovery.
- Partition management.
- Storage I/O.

## 5.7 Virtual Filesystem

Responsible for:

- Common filesystem interfaces.
- File operations.
- Directory operations.
- Mount management.
- Filesystem driver integration.

## 5.8 Networking

Responsible for:

- Network device interfaces.
- Ethernet.
- IP networking.
- Network configuration.
- Network file transfer.

---

# 6. Storage and Recovery Architecture

The storage subsystem is a critical part of RahiOS.

It must support a safe and extensible recovery architecture.

Initial planned filesystem targets:

- FAT32.
- exFAT.
- NTFS.
- ext2/ext3/ext4.

Initial partition formats:

- MBR.
- GPT.

Recovery capabilities:

- Disk discovery.
- Partition inspection.
- Filesystem inspection.
- Read-only file access.
- File copying.
- Disk imaging.
- Backup creation.
- Deleted-file recovery research.
- Bootloader inspection.
- Filesystem repair.
- Partition repair.

All destructive recovery operations must require explicit authorization.

Read-only access will be the default where supported.

Failing storage devices should be imaged before attempting repairs whenever practical.

---

# 7. Graphical Architecture

The GUI will be independently developed.

Its logical components will include:

1. Framebuffer Interface.
2. Graphics Engine.
3. Rendering Engine.
4. Font Engine.
5. Input Manager.
6. Window Manager.
7. Widget Toolkit.
8. Desktop Environment.
9. Application Framework.

The interface should prioritize usability, responsiveness, accessibility, and low resource consumption.

The recovery dashboard will be the primary user experience.

---

# 8. Command-Line Architecture

RahiOS will include an independent command-line environment.

It will support:

- Command interpretation.
- Built-in commands.
- External utilities.
- Filesystem navigation.
- Storage inspection.
- Hardware diagnostics.
- Recovery operations.
- System information.
- Logging.

The terminal and GUI will access common system services.

They must not implement conflicting storage or recovery logic.

---

# 9. Security and Data Integrity

Security is a core architectural requirement.

The system must provide:

- Kernel and user-space separation.
- Memory protection.
- Controlled system calls.
- Process isolation.
- Explicit storage permissions.
- Safe handling of encrypted storage.
- Destructive-operation confirmation.
- Reliable logging.
- Source/destination identification.

The system must never assume that an attached storage device is safe to modify.

---

# 10. Development Environment Architecture

Host:

- Windows.

Development subsystem:

- WSL2 with a Linux distribution.

Tool categories:

- C compiler.
- Assembler.
- Linker.
- Debugger.
- Build automation.
- Version control.
- QEMU.
- Disk image and ISO utilities.

Testing environments:

- UEFI virtual machine.
- Physical UEFI computer.
- Legacy BIOS virtual machine during compatibility development.
- Physical storage devices.

All builds should eventually be reproducible from documented source and build configurations.

---

# 11. Architectural Constraints

1. No existing general-purpose OS kernel.
2. No existing desktop environment.
3. No existing window manager.
4. No automatic modification of internal storage.
5. No dependence on the installed operating system.
6. Initial CPU architecture limited to x86-64.
7. Initial boot priority given to UEFI.
8. Hardware compatibility developed incrementally.
9. Kernel and user-space interfaces must be documented.
10. Architecture changes require documented review.

---

# 12. Architecture Acceptance Criteria

The architecture will be considered approved when:

- All major subsystem boundaries are documented.
- Bootloader and kernel responsibilities are defined.
- Kernel execution and privilege model are specified.
- Memory and process management responsibilities are defined.
- Device-driver interfaces are planned.
- Storage and filesystem interfaces are documented.
- GUI and terminal responsibilities are defined.
- Recovery safety requirements are established.
- Testing and compatibility strategies are documented.

---

**Document Control**

Owner: Sajid Rahi
Document: RahiOS-ARCH-001
Version: 0.2
Next Review: Before implementation planning is finalized.
