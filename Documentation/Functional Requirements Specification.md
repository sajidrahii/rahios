# RahiOS — Functional Requirements Specification

**Document ID:** RahiOS-FRS-001  
**Version:** 0.1  
**Status:** Initial Draft — Pending Review  
**Project:** RahiOS  
**Document Type:** Functional Requirements Specification  
**Target Architecture:** x86-64  
**Kernel Architecture:** Modular Monolithic  
**Kernel Languages:** C + Assembly  
**Initial Boot Firmware:** UEFI  
**Development Host:** Windows + WSL2

---

# 1. Introduction

## 1.1 Purpose

This document defines the functional requirements of RahiOS, an independently developed, portable emergency operating system.

It specifies the expected capabilities, behavior, interfaces, and operational constraints of the system.

It will serve as the functional reference for subsequent architecture, subsystem design, implementation, and testing documentation.

## 1.2 Product Description

RahiOS is a lightweight, independent operating system designed to boot from removable USB storage and provide an environment for computer diagnostics, data recovery, storage management, and emergency system maintenance.

The operating system will feature:

- A custom kernel.
- An independent bootloader.
- A custom graphical environment.
- An independent command-line interface.
- Hardware discovery and device management.
- Storage and filesystem support.
- Recovery and diagnostic utilities.

RahiOS must operate independently of the computer's installed operating system.

## 1.3 Product Objectives

The primary objectives are:

1. Boot independently from removable storage.
2. Detect and identify computer hardware.
3. Access supported internal and external storage devices.
4. Browse and recover important user files.
5. Create disk images and backups.
6. Inspect and manage partitions.
7. Diagnose storage and hardware problems.
8. Troubleshoot supported operating system boot configurations.
9. Provide a graphical recovery environment.
10. Provide an optional command-line environment.
11. Protect user data against accidental modification.

## 1.4 Target Users

- Ordinary computer users.
- Computer technicians.
- IT support professionals.
- Data recovery specialists.
- System administrators.
- Operating system developers.

## 1.5 Definitions

| Term | Definition |
|---|---|
| Host OS | Operating system installed on the computer |
| Live OS | OS running independently from removable media |
| Bootloader | Software responsible for loading the kernel |
| Kernel | Core of the operating system |
| VFS | Virtual Filesystem |
| GPT | GUID Partition Table |
| MBR | Master Boot Record |
| UEFI | Unified Extensible Firmware Interface |
| SMART | Self-Monitoring, Analysis and Reporting Technology |
| Source Disk | Storage device from which data is recovered |
| Destination Disk | Storage device receiving recovered data |
| Disk Image | Sector-level representation of a storage device |
| Read-only | Storage access that prohibits intentional write operations |

---

# 2. Product Scope

## 2.1 In-Scope Features

RahiOS will provide the following capabilities:

- Independent USB boot.
- Hardware detection.
- Storage device discovery.
- Partition identification.
- Filesystem browsing.
- File recovery and copying.
- Disk imaging.
- Backup management.
- Storage diagnostics.
- Partition management.
- Filesystem inspection and repair.
- Bootloader troubleshooting.
- Hardware information.
- Graphical desktop.
- Command-line interface.
- Network connectivity.
- System logging.
- Safe shutdown and restart.

## 2.2 Out-of-Scope Features for Initial Development

The following are not initial requirements:

- General-purpose daily computing.
- Gaming platform.
- Multimedia production environment.
- Full replacement for Windows or Linux.
- Support for every processor architecture.
- Universal support for every proprietary hardware device.
- Guaranteed recovery of physically destroyed storage.
- Bypassing encryption or authentication.
- Automatic repair of every operating system installation.

These capabilities may be reconsidered in future versions.

---

# 3. Functional Requirements

## Requirement classification
We'll use four priorities:  
| Priority | Meaning |  
|---|---|
P0 — Critical | Essential for the core operating system
P1 — High | Required for a usable recovery environment
P2 — Medium | Important additional functionality  
P3 — Future | Planned enhancement  

## 3.1 Boot and Startup Management

**Module ID: BOOT**

### BOOT-FR-001 — Independent Boot

Priority: P0

RahiOS shall boot independently from a supported USB storage device without requiring the installed operating system.

Acceptance criteria:
- The computer can start RahiOS from USB.
- The installed operating system is not required.
- The boot process does not require an installation to internal storage.

### BOOT-FR-002 — UEFI Compatibility

Priority: P0

RahiOS shall support booting on compatible x86-64 UEFI systems.

### BOOT-FR-003 — Bootloader Operation

Priority: P0

RahiOS shall provide its own bootloader responsible for loading and transferring control to the kernel.

### BOOT-FR-004 — Boot Configuration

Priority: P1

The bootloader shall provide a configurable boot menu.

Planned options:
- Start RahiOS.
- Start in diagnostic mode.
- Start with basic display compatibility options.
- Restart or power off.

### BOOT-FR-005 — Boot Failure Reporting

Priority: P1

RahiOS shall provide meaningful diagnostic information when boot initialization fails.

### BOOT-FR-006 — Firmware Information

Priority: P2

RahiOS shall identify available firmware information, including firmware type and supported boot environment details.

### BOOT-FR-007 — Legacy BIOS Compatibility

Priority: P2

RahiOS shall provide a separate Legacy BIOS boot path in a future compatibility milestone.

---

## 3.2 Hardware Detection and Management

**Module ID: HW**

### HW-FR-001 — Hardware Discovery

Priority: P0

RahiOS shall identify supported hardware devices during startup and subsequent device discovery.

### HW-FR-002 — CPU Information

Priority: P1

RahiOS shall display:
- Processor vendor.
- Processor model information where available.
- Architecture.
- Logical processor count.
- Supported CPU features.

### HW-FR-003 — Memory Information

Priority: P1

RahiOS shall display:
- Detected physical memory.
- Available memory.
- Memory currently allocated.
- Memory usage information.

### HW-FR-004 — Storage Controller Detection

Priority: P0

RahiOS shall identify supported storage controllers.

Initial targets:
- SATA/AHCI.
- NVMe.

### HW-FR-005 — Input Device Detection

Priority: P0

RahiOS shall identify and support compatible keyboards and pointing devices.

### HW-FR-006 — Display Detection

Priority: P0

RahiOS shall initialize a supported display interface and provide a usable visual output.

### HW-FR-007 — Device Status

Priority: P1

RahiOS shall display device identification, availability, and driver status.

### HW-FR-008 — Hardware Inventory

Priority: P2

RahiOS shall provide a consolidated hardware inventory.

---

## 3.3 Storage Device Management

**Module ID: STORAGE**

### STORAGE-FR-001 — Storage Discovery

Priority: P0

RahiOS shall detect supported internal and external storage devices.

### STORAGE-FR-002 — Storage Identification

Priority: P0

RahiOS shall display:
- Device name and model where available.
- Device capacity.
- Device type.
- Interface type.
- Device status.

### STORAGE-FR-003 — Storage Classification

Priority: P1

RahiOS shall distinguish between:
- Internal storage.
- External storage.
- Boot media.
- Removable storage.
- Destination storage.

### STORAGE-FR-004 — Partition Discovery

Priority: P0

RahiOS shall identify supported partition layouts.

Initial targets:
- MBR.
- GPT.

### STORAGE-FR-005 — Partition Information

Priority: P0

RahiOS shall display partition information, including:
- Partition number.
- Starting sector.
- Partition size.
- Partition type.
- Filesystem identification where available.
- Partition status.

### STORAGE-FR-006 — Device Refresh

Priority: P1

RahiOS shall allow users to refresh device discovery after attaching or removing supported storage devices.

### STORAGE-FR-007 — Storage Status

Priority: P1

RahiOS shall communicate unavailable, disconnected, unsupported, or potentially failing devices to the user.

### STORAGE-FR-008 — Safe Device Removal

Priority: P1

RahiOS shall provide a mechanism for safely removing supported external storage devices.

---

## 3.4 Filesystem Management

**Module ID: FS**

### FS-FR-001 — Filesystem Identification

Priority: P0

RahiOS shall identify supported filesystems on detected partitions.

### FS-FR-002 — Initial Filesystem Support

Priority: P0

The system shall target the following filesystems:

- FAT32.
- NTFS.
- exFAT.
- ext2.
- ext3.
- ext4.

Actual support shall be documented by filesystem and operation, rather than treating identification, reading, and writing as equivalent capabilities.

### FS-FR-003 — Read-only Filesystem Access

Priority: P0

RahiOS shall support read-only access to supported filesystems.

### FS-FR-004 — Filesystem Mounting

Priority: P0

RahiOS shall allow users or authorized system services to mount supported filesystems.

### FS-FR-005 — Filesystem Unmounting

Priority: P1

RahiOS shall allow mounted filesystems to be safely unmounted.

### FS-FR-006 — Filesystem Status

Priority: P1

RahiOS shall display filesystem information and identify unsupported or potentially inconsistent filesystems.

### FS-FR-007 — Write Support

Priority: P2

RahiOS may introduce controlled write support for selected filesystems after filesystem-specific validation and recovery safety testing.

---

## 3.5 File Management and Recovery

**Module ID: FILE**

### FILE-FR-001 — File Browser

Priority: P0

RahiOS shall provide a graphical file manager.

### FILE-FR-002 — Directory Navigation

Priority: P0

Users shall be able to browse supported mounted filesystems.

### FILE-FR-003 — File Information

Priority: P1

The file manager shall display available information, including:
- Filename.
- File type.
- File size.
- Directory location.
- Available timestamps.

### FILE-FR-004 — File Copying

Priority: P0

Users shall be able to copy files from source storage to destination storage.

### FILE-FR-005 — Directory Copying

Priority: P0

Users shall be able to copy complete directories and their contents.

### FILE-FR-006 — Copy Progress

Priority: P1

RahiOS shall display:
- Current operation.
- Files processed.
- Total progress where determinable.
- Transfer status.
- Errors and skipped files.

### FILE-FR-007 — Copy Verification

Priority: P1

RahiOS shall offer file verification using checksums after copying.

### FILE-FR-008 — File Search

Priority: P2

Users shall be able to search for files by name and file type.

### FILE-FR-009 — File Recovery

Priority: P1

RahiOS shall provide tools for recovering accessible files from supported storage devices.

### FILE-FR-010 — Deleted File Recovery

Priority: P2

RahiOS shall investigate and support deleted-file recovery for selected filesystems where metadata and overwritten data permit.

### FILE-FR-011 — Recovery Destination

Priority: P0

The system shall require an explicit destination for recovered data.

The recovery interface shall clearly distinguish source and destination devices.

### FILE-FR-012 — Recovery Reporting

Priority: P1

RahiOS shall generate a report of completed, failed, skipped, and partially recovered files.

---

## 3.6 Disk Imaging and Backup

**Module ID: BACKUP**

### BACKUP-FR-001 — Disk Imaging

Priority: P1

RahiOS shall provide the ability to create a sector-level image of a supported storage device.

### BACKUP-FR-002 — Image Destination

Priority: P1

Users shall select a destination with sufficient available capacity.

### BACKUP-FR-003 — Imaging Progress

Priority: P1

RahiOS shall display imaging progress, processed data, errors, and operation status.

### BACKUP-FR-004 — Interrupted Imaging

Priority: P2

The system shall support documented handling of interrupted imaging operations, including identification of incomplete images.

### BACKUP-FR-005 — Image Verification

Priority: P1

RahiOS shall provide image verification using suitable checksums.

### BACKUP-FR-006 — Image Restoration

Priority: P2

RahiOS shall support restoring a disk image to a selected destination device after explicit confirmation.

### BACKUP-FR-007 — Source Protection

Priority: P0

Disk imaging operations shall prevent accidental selection of the same device as both source and destination.

---

## 3.7 Disk Diagnostics and Health Monitoring

**Module ID: DIAG**

### DIAG-FR-001 — SMART Information

Priority: P1

RahiOS shall retrieve and display SMART information from supported storage devices.

### DIAG-FR-002 — Disk Health

Priority: P1

RahiOS shall communicate available storage health information and warnings.

### DIAG-FR-003 — Hardware Diagnostics

Priority: P2

RahiOS shall provide basic hardware diagnostic information.

### DIAG-FR-004 — Filesystem Inspection

Priority: P1

RahiOS shall inspect supported filesystems for detectable inconsistencies.

### DIAG-FR-005 — Diagnostic Reports

Priority: P1

RahiOS shall generate reports containing diagnostic findings and relevant device information.

### DIAG-FR-006 — Error Logging

Priority: P0

RahiOS shall maintain system and operation logs during the current session.

### DIAG-FR-007 — Failing Storage Warning

Priority: P0

RahiOS shall warn users when available diagnostic information indicates possible storage failure.

---

## 3.8 Partition Management

**Module ID: PART**

### PART-FR-001 — Partition Inspection

Priority: P0

Users shall be able to inspect partition layouts.

### PART-FR-002 — Partition Creation

Priority: P2

RahiOS shall eventually support creating partitions on selected storage devices.

### PART-FR-003 — Partition Deletion

Priority: P2

RahiOS shall eventually support deleting selected partitions after explicit confirmation.

### PART-FR-004 — Partition Resizing

Priority: P3

Partition resizing may be supported in a future release.

### PART-FR-005 — Partition Formatting

Priority: P2

RahiOS shall eventually support formatting selected partitions using supported filesystems.

### PART-FR-006 — Partition Operation Confirmation

Priority: P0

All destructive partition operations shall require explicit confirmation.

### PART-FR-007 — Partition Operation Reporting

Priority: P1

RahiOS shall report the result of partition operations and identify failures.

---

## 3.9 Filesystem Repair

**Module ID: REPAIR**

### REPAIR-FR-001 — Filesystem Assessment

Priority: P1

RahiOS shall provide filesystem assessment tools for supported filesystems.

### REPAIR-FR-002 — Repair Eligibility

Priority: P1

RahiOS shall identify whether a supported filesystem is eligible for a repair operation.

### REPAIR-FR-003 — Repair Authorization

Priority: P0

Filesystem repair shall never be initiated automatically.

### REPAIR-FR-004 — Repair Warning

Priority: P0

Before repair, RahiOS shall explain potential data-loss risks and recommend imaging the source device when appropriate.

### REPAIR-FR-005 — Repair Reporting

Priority: P1

RahiOS shall report repair results and detected inconsistencies.

### REPAIR-FR-006 — Repair Restrictions

Priority: P0

RahiOS shall not claim that an unsupported filesystem can be repaired.

---

## 3.10 Bootloader Troubleshooting

**Module ID: BOOTREC**

### BOOTREC-FR-001 — Boot Partition Discovery

Priority: P1

RahiOS shall identify supported operating system boot partitions.

### BOOTREC-FR-002 — EFI System Partition Inspection

Priority: P1

RahiOS shall inspect supported EFI System Partitions.

### BOOTREC-FR-003 — Bootloader File Inspection

Priority: P1

RahiOS shall identify supported bootloader files and report their availability.

### BOOTREC-FR-004 — Boot Configuration Inspection

Priority: P2

RahiOS shall provide tools to inspect supported boot configuration data.

### BOOTREC-FR-005 — Boot Repair Assistance

Priority: P2

RahiOS shall provide guided boot repair procedures for explicitly supported configurations.

### BOOTREC-FR-006 — Boot Repair Confirmation

Priority: P0

Any operation that modifies boot partitions or boot configuration shall require explicit authorization.

### BOOTREC-FR-007 — Boot Repair Backup

Priority: P1

RahiOS shall recommend or create appropriate backups of affected boot configuration data before modifications.

---

## 3.11 Graphical User Interface

**Module ID: GUI**

### GUI-FR-001 — Graphical Desktop

Priority: P0

RahiOS shall provide an independently developed graphical desktop environment.

### GUI-FR-002 — Recovery Dashboard

Priority: P0

The desktop shall provide a central recovery dashboard.

### GUI-FR-003 — Navigation

Priority: P0

Users shall be able to navigate the graphical environment using supported input devices.

### GUI-FR-004 — Window Management

Priority: P1

RahiOS shall support multiple application windows.

### GUI-FR-005 — Application Controls

Priority: P1

Users shall be able to open, close, minimize, maximize, and move supported application windows.

### GUI-FR-006 — Notifications

Priority: P1

The graphical environment shall provide notifications for important system events.

### GUI-FR-007 — Error Dialogs

Priority: P0

Critical operations and failures shall be communicated through understandable messages.

### GUI-FR-008 — Low-Resource Operation

Priority: P1

The GUI shall be designed to operate within the system's documented minimum hardware requirements.

### GUI-FR-009 — Terminal Access

Priority: P0

Users shall be able to open the command-line environment from the graphical desktop.

### GUI-FR-010 — Accessibility

Priority: P2

The GUI should support configurable display scaling, readable fonts, and keyboard navigation.

---

## 3.12 Command-Line Interface

**Module ID: CLI**

### CLI-FR-001 — Terminal

Priority: P0

RahiOS shall provide an independent command-line interface.

### CLI-FR-002 — Command Interpreter

Priority: P0

The terminal shall interpret and execute supported commands.

### CLI-FR-003 — File Navigation

Priority: P1

Users shall be able to navigate supported filesystems through terminal commands.

### CLI-FR-004 — Storage Commands

Priority: P1

The terminal shall provide commands for inspecting storage devices and partitions.

### CLI-FR-005 — Diagnostic Commands

Priority: P1

The terminal shall provide access to supported diagnostic functions.

### CLI-FR-006 — Recovery Commands

Priority: P1

Authorized users shall be able to execute supported recovery operations through the CLI.

### CLI-FR-007 — Help System

Priority: P1

The terminal shall provide command descriptions and usage information.

### CLI-FR-008 — Error Reporting

Priority: P0

Invalid commands and failed operations shall return understandable error information.

---

## 3.13 Networking

**Module ID: NET**

### NET-FR-001 — Network Device Discovery

Priority: P2

RahiOS shall identify supported network adapters.

### NET-FR-002 — Ethernet Connectivity

Priority: P2

RahiOS shall provide Ethernet connectivity through supported drivers.

### NET-FR-003 — IP Configuration

Priority: P2

RahiOS shall support basic network configuration.

### NET-FR-004 — Network File Transfer

Priority: P2

RahiOS shall support file transfer over supported network protocols.

### NET-FR-005 — Wi-Fi

Priority: P3

RahiOS may support wireless networking through compatible hardware and firmware.

### NET-FR-006 — Network Security

Priority: P1

Network operations shall not expose recovered files or sensitive information without user authorization.

---

## 3.14 System Management

**Module ID: SYS**

### SYS-FR-001 — System Information

Priority: P1

RahiOS shall provide system information, including:
- OS version.
- Kernel version.
- Boot mode.
- System uptime.
- Memory information.
- Hardware inventory.

### SYS-FR-002 — System Logging

Priority: P0

RahiOS shall record relevant startup, hardware, and recovery events.

### SYS-FR-003 — Session Management

Priority: P1

RahiOS shall provide a managed system session.

### SYS-FR-004 — Shutdown

Priority: P0

Users shall be able to shut down the system safely.

### SYS-FR-005 — Restart

Priority: P0

Users shall be able to restart the system safely.

### SYS-FR-006 — Live Session

Priority: P0

The default operating mode shall run independently from the internal storage devices.

### SYS-FR-007 — Persistence

Priority: P2

RahiOS may support optional persistence for user settings and selected session data in future releases.

### SYS-FR-008 — Session Cleanup

Priority: P1

The system shall provide a mechanism to close applications and release resources before shutdown.

---

# 4. Non-Functional Requirements

The following requirements define system quality rather than individual features.

## 4.1 Performance

**NFR-001:** The operating system shall be optimized for low memory consumption.

**NFR-002:** The system shall minimize unnecessary background services.

**NFR-003:** File operations shall provide measurable progress information.

**NFR-004:** Boot performance shall be measured and optimized.

**NFR-005:** Performance shall be evaluated on both virtual and physical hardware.

Specific performance targets will be established after initial architectural feasibility testing.

## 4.2 Compatibility

**NFR-006:** Initial releases shall target x86-64 systems.

**NFR-007:** Initial boot support shall target UEFI.

**NFR-008:** Legacy BIOS support shall be considered in a subsequent compatibility milestone.

**NFR-009:** Hardware support shall be explicitly documented.

**NFR-010:** Unsupported hardware shall be reported without causing system instability where reasonably preventable.

## 4.3 Reliability

**NFR-011:** Kernel failures shall provide diagnostic information wherever possible.

**NFR-012:** Failed recovery operations shall not be reported as successful.

**NFR-013:** File-copy operations shall identify incomplete transfers.

**NFR-014:** Storage operations shall handle unexpected device disconnection safely.

**NFR-015:** Critical operations shall maintain operation status and error information.

## 4.4 Usability

**NFR-016:** The recovery dashboard shall be understandable to non-technical users.

**NFR-017:** Destructive operations shall display clear warnings.

**NFR-018:** GUI and CLI shall expose consistent system functionality.

**NFR-019:** Error messages shall identify the problem and provide useful next steps.

## 4.5 Maintainability

**NFR-020:** Kernel subsystems shall have documented interfaces.

**NFR-021:** Functional requirements shall have corresponding test cases.

**NFR-022:** Architectural decisions shall be documented.

**NFR-023:** Builds shall be reproducible.

**NFR-024:** Source code shall follow documented coding standards.

---

# 5. Data Safety and Security Requirements

These requirements have special importance because RahiOS handles potentially irreplaceable user data.

## 5.1 Storage Safety

**SAFE-FR-001:** Internal storage shall not be modified automatically during startup.

**SAFE-FR-002:** Supported source filesystems shall default to read-only access wherever technically possible.

**SAFE-FR-003:** The user shall explicitly select a destination before recovering files.

**SAFE-FR-004:** The system shall clearly identify source and destination devices.

**SAFE-FR-005:** Destructive operations shall require explicit confirmation.

**SAFE-FR-006:** The system shall warn users when a destination device may contain existing data.

**SAFE-FR-007:** The system shall prevent accidental source/destination selection conflicts.

**SAFE-FR-008:** A failing storage device shall trigger a recommendation to create a disk image before repair attempts, where practical.

**SAFE-FR-009:** The system shall not automatically execute filesystem repairs.

**SAFE-FR-010:** The system shall not automatically format partitions.

## 5.2 Encryption

**SEC-FR-001:** RahiOS shall identify supported encrypted storage formats where possible.

**SEC-FR-002:** RahiOS shall not bypass encryption or authentication.

**SEC-FR-003:** Access to encrypted filesystems shall require appropriate user credentials or recovery keys.

**SEC-FR-004:** Sensitive credentials shall not be written to ordinary diagnostic logs.

## 5.3 Access Control

**SEC-FR-005:** The kernel shall enforce user-space and kernel-space separation.

**SEC-FR-006:** Privileged storage operations shall require appropriate authorization.

**SEC-FR-007:** Applications shall not receive unrestricted hardware access by default.

---

# 6. User Interaction and Operational Workflows

## 6.1 Standard Emergency Recovery Workflow

1. User inserts the RahiOS USB device.
2. User selects USB boot from firmware.
3. RahiOS starts.
4. Hardware discovery occurs.
5. Recovery dashboard appears.
6. User selects Recover Files.
7. RahiOS displays available storage devices.
8. User selects the source device.
9. User selects the destination device.
10. User browses and selects files.
11. RahiOS performs the copy.
12. RahiOS reports the result.
13. User safely removes destination storage.
14. User shuts down RahiOS.

## 6.2 Disk Imaging Workflow

1. User selects Disk Imaging.
2. RahiOS lists detected storage devices.
3. User selects the source device.
4. RahiOS displays source details.
5. User selects an image destination.
6. RahiOS validates destination capacity.
7. User reviews and confirms the operation.
8. RahiOS creates the image.
9. RahiOS reports errors and progress.
10. RahiOS offers image verification.

## 6.3 Filesystem Repair Workflow

1. User selects Filesystem Diagnostics.
2. User selects a partition.
3. RahiOS inspects filesystem information.
4. RahiOS reports detected inconsistencies.
5. RahiOS recommends a backup or disk image when appropriate.
6. User explicitly selects a repair operation.
7. RahiOS displays the potential consequences.
8. User confirms.
9. RahiOS performs the supported repair.
10. RahiOS generates a report.

## 6.4 Bootloader Troubleshooting Workflow

1. User selects Boot Recovery.
2. RahiOS identifies supported boot partitions.
3. User selects an operating system installation.
4. RahiOS inspects bootloader files and configuration.
5. RahiOS reports detected issues.
6. RahiOS presents supported recovery actions.
7. User selects an action.
8. RahiOS creates or recommends a configuration backup.
9. User confirms the modification.
10. RahiOS performs the operation and reports its outcome.

---

# 7. User Interface Requirements

## 7.1 Main Dashboard

The dashboard shall provide access to:

- File Recovery.
- Storage Devices.
- Disk Imaging.
- Partition Management.
- System Diagnostics.
- Filesystem Tools.
- Boot Recovery.
- Terminal.
- Network Tools.
- System Information.
- Shutdown and Restart.

## 7.2 User Experience Principles

- Simple navigation.
- Clear device identification.
- Visible operation progress.
- Understandable error messages.
- Consistent confirmation dialogs.
- Minimal unnecessary interface elements.
- Keyboard accessibility.
- Optional advanced controls.

## 7.3 GUI and CLI Consistency

The GUI and CLI shall use common system functionality.

An operation available through the GUI should use the same underlying system service as its CLI equivalent.

---

# 8. Release Scope and Feature Prioritization

The following table establishes the intended release boundaries.

| Capability | Prototype | Alpha | Beta | v1.0 |
|---|---|---|---|---|
| UEFI boot | Yes | Yes | Yes | Yes |
| Kernel initialization | Yes | Yes | Yes | Yes |
| Hardware discovery | Basic | Expanded | Expanded | Documented |
| Memory management | Basic | Expanded | Complete target | Stable |
| Process management | No | Basic | Expanded | Stable |
| Terminal | Diagnostic | Basic | Expanded | Yes |
| Storage detection | No | Basic | Expanded | Yes |
| Partition inspection | No | Basic | Yes | Yes |
| Filesystem reading | No | Limited | Expanded | Supported targets |
| File manager | No | Basic | Yes | Yes |
| File recovery | No | Basic copying | Expanded | Yes |
| Disk imaging | No | No | Basic | Yes |
| GUI | No | Early | Expanded | Yes |
| SMART diagnostics | No | No | Basic | Supported devices |
| Filesystem repair | No | No | Experimental | Limited supported targets |
| Bootloader recovery | No | No | Experimental | Limited supported configurations |
| Networking | No | No | Basic Ethernet | Supported Ethernet |
| Legacy BIOS | No | No | Experimental | Compatibility target |

The release table represents planned maturity, not a guarantee of universal hardware support.

---

# 9. Requirements Traceability

Each requirement shall have:

- A unique identifier.
- A priority.
- A responsible subsystem.
- A design reference.
- One or more test cases.
- An implementation status.
- An acceptance status.

Example:

| Requirement | Subsystem | Validation |
|---|---|---|
| BOOT-FR-001 | Bootloader | USB boot test |
| HW-FR-001 | Hardware | Device discovery test |
| STORAGE-FR-001 | Storage | Storage detection test |
| FS-FR-003 | Filesystem | Read-only access test |
| FILE-FR-004 | File manager | File copy test |
| BACKUP-FR-001 | Backup | Disk imaging test |
| GUI-FR-001 | GUI | Desktop startup test |
| CLI-FR-001 | CLI | Terminal startup test |
| SAFE-FR-005 | Safety | Destructive operation confirmation test |

A requirement cannot be marked complete solely because its code exists. Its documented acceptance criteria must pass.

---

# 10. Assumptions and Constraints

1. RahiOS will initially target x86-64 computers.
2. UEFI is the first supported firmware environment.
3. Legacy BIOS compatibility is a subsequent objective.
4. Development will take place on Windows with WSL2.
5. The kernel will be written in C and Assembly.
6. The kernel architecture will be modular monolithic.
7. The graphical subsystem will be independently developed.
8. The system will run as a live operating system.
9. Internal storage protection is a fundamental requirement.
10. Hardware and filesystem compatibility will expand incrementally.
11. Encrypted data requires legitimate authorization credentials.
12. No recovery operation can guarantee recovery of physically destroyed or overwritten data.

---

# 11. Open Requirements and Future Decisions

The following subjects require separate technical specifications:

- Minimum RAM requirement.
- Minimum USB capacity.
- Supported screen resolutions.
- Supported display modes.
- Detailed hardware compatibility matrix.
- Filesystem-specific read/write capabilities.
- Filesystem repair coverage.
- Bootloader repair coverage.
- Disk imaging formats.
- Recovery report formats.
- Network protocols.
- User-space permission model.
- Persistence configuration.
- Localization and language support.
- Performance targets.

These values shall not be considered finalized until their technical feasibility has been evaluated.

---

# 12. Acceptance Criteria for the FRS

This specification will be approved when:

1. All core system capabilities are documented.
2. Each functional requirement has a unique identifier.
3. Every requirement has an assigned priority.
4. Requirements are distinguishable from implementation decisions.
5. Recovery workflows are documented.
6. Data safety requirements are established.
7. Initial release boundaries are defined.
8. Testing and acceptance criteria are traceable.
9. Open questions are documented.
10. The project owner approves the specification.

---

# 13. Document Control

**Project:** RahiOS  
**Document ID:** RahiOS-FRS-001  
**Version:** 0.1  
**Status:** Initial Draft — Pending Review  
**Owner:** Sajid Ahmed Rahi   
**Related Document:** RahiOS-ARCH-001  
**Next Document:** RahiOS-BDS-001 — Bootloader Design Specification

**Change History**

| Version | Description |
|---|---|
| 0.1 | Initial functional requirements specification |

**End of Document**