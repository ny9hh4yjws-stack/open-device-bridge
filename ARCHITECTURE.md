# Open Device Bridge Architecture

## Status

This document describes the evolving architecture of Open Device Bridge (ODB).

ODB is currently an early-stage architectural proposal and reference implementation effort. The ESP32-S3/iPad case is the initial reference case, not the boundary of the project.

## 1. Purpose

Open Device Bridge is a proposed operating-system-mediated framework for secure, user-authorized, capability-scoped interaction between computing platforms and user-owned external hardware.

The central principle is:

> **Connection should initiate negotiation — not assumption, unrestricted access, or a dead end.**

ODB proposes the lifecycle:

> **Connect → Identify → Explain → Authorize → Interact → Verify → Revoke**

The objective is not unrestricted USB or hardware access.

The objective is to give operating systems a structured way to determine:

1. What device is connected?
2. What capabilities does it expose?
3. What does an application want to do?
4. What risks are associated with that operation?
5. Has the user authorized it?
6. Can the OS enforce that authorization?
7. Did the requested operation succeed?
8. Can access subsequently be revoked?

---

## 2. Origin of the Reference Case

ODB originated from a simple hardware interoperability problem.

A user had:

- an iPad with USB-C
- an ESP32-S3-based Heltec LoRa device
- a standards-based physical connection
- sufficient computing resources
- internet access
- browser-based firmware tools

Yet the complete firmware workflow could not be performed directly through the available iPadOS browser environment.

The immediate workaround was to use another class of computer.

That exposed a broader question:

> Why should physically connected, user-owned hardware require a second general-purpose computer when the first computer is technically capable of participating in the operation?

The Heltec/ESP32-S3 case remains the initial reference implementation because it gives ODB a concrete and testable boundary.

---

## 3. The Broader Interoperability Problem

The same general problem appears across many hardware ecosystems.

Relevant device classes include:

- Arduino and other microcontrollers
- ESP32 development hardware
- Meshtastic and LoRa devices
- thermal imaging equipment such as FLIR
- OBD-II automotive diagnostic interfaces
- software-defined radios
- oscilloscopes
- digital multimeters
- USB microscopes
- GNSS/GPS receivers
- amateur-radio equipment
- 3D printers
- CNC controllers
- robotics systems
- flight controllers
- environmental sensors
- scientific instrumentation
- laboratory equipment
- industrial diagnostic hardware
- accessibility and assistive devices

These systems may use USB, Bluetooth, Wi-Fi, Ethernet, proprietary applications, cloud services, platform-specific drivers, or combinations of them.

ODB treats this fragmentation as an architectural problem rather than a collection of unrelated device problems.

---

## 4. Existing Precedent

ODB does not assume that modern mobile operating systems are incapable of interacting with sophisticated hardware.

Existing ecosystems already demonstrate combinations of:

- external sensors
- professional instrumentation
- firmware management
- diagnostic equipment
- imaging hardware
- Bluetooth peripherals
- USB accessories
- network-connected instruments

Examples such as FLIR thermal imaging equipment and mobile-compatible OBD-II systems demonstrate that sophisticated external hardware can participate in mobile computing workflows when a suitable platform-specific integration exists.

Arduino, ESP32, SDR, laboratory equipment, and similar ecosystems demonstrate the other side of the problem: generic development, diagnostic, serial, recovery, and firmware workflows often remain dependent on particular operating systems, drivers, browsers, or desktop software.

ODB asks whether part of this integration burden can be represented by a common security and capability architecture.

---

## 5. Architectural Separation

ODB separates concepts that are often treated as one problem.

### Transport

How data reaches the device.

Examples:

- USB / USB-C
- Thunderbolt
- Bluetooth / BLE
- Wi-Fi
- Ethernet
- NFC
- future transports

### Protocol or Driver

How the host interprets and exchanges device-specific information.

Some hardware will always require specialized protocols or drivers.

ODB does not attempt to eliminate that requirement.

### Authorization

Whether a particular application or user is permitted to perform a particular operation.

ODB primarily standardizes this layer while providing common mechanisms for discovery, identity, capability negotiation, trust, and session management.

The intended abstraction is:

> **Intent → Capability → Authorization → Session → Protocol → Transport**

---

## 6. ODB Core

ODB Core defines concepts that should remain independent of individual device types.

Proposed core functions include:

- device discovery
- device identity
- identity provenance
- capability declaration
- capability requests
- user authorization
- application authorization
- session establishment
- privilege boundaries
- verification
- revocation
- auditing
- risk classification
- error reporting
- recovery state

The operating system remains the enforcement authority.

---

## 7. Capability Model

ODB avoids treating hardware access as one binary permission.

Instead, applications request specific capabilities.

Examples include:

- `device.identify`
- `telemetry.read`
- `sensor.read`
- `serial.read`
- `serial.write`
- `configuration.read`
- `configuration.write`
- `firmware.read`
- `firmware.flash`
- `recovery`
- `debug.attach`
- `sensor.stream`
- `radio.control`
- `diagnostics.read`
- `machine.control`

The OS may apply different authorization requirements to different capabilities.

Reading device identity is not equivalent to replacing firmware.

Reading temperature is not equivalent to commanding a motor.

ODB should preserve those distinctions.

---

## 8. Risk Classes

ODB should allow capabilities to be associated with risk.

A preliminary model might include:

### Class A — Observation

Examples:

- identity
- telemetry
- sensor measurements

Primarily read-only.

### Class B — Configuration

Examples:

- serial communication
- device configuration
- firmware installation
- recovery

Operations may alter device state.

### Class C — External-System Interaction

Examples:

- vehicle diagnostics
- radio configuration
- network equipment

Operations may affect systems beyond the connected peripheral.

### Class D — Physical Actuation

Examples:

- CNC machinery
- robotics
- motors
- flight controllers

Commands may cause physical movement or create safety hazards.

Higher-risk capabilities may require stronger authorization, additional policy enforcement, or active-session confirmation.

---

## 9. Device Identity and Trust

Authorization requires meaningful device identity.

Potential identity information includes:

- manufacturer
- product
- model
- hardware revision
- processor architecture
- USB VID/PID
- interface classes
- bootloader
- firmware version
- exposed capabilities

ODB should also communicate where identity information came from.

A possible trust model:

### Level 0 — Unknown

The OS detects a device but cannot establish meaningful identity.

### Level 1 — Observed

Identity is derived from descriptors and observable device behavior.

### Level 2 — Community Identified

The device matches metadata from an open registry.

### Level 3 — Manufacturer Declared

Metadata is supplied by the manufacturer.

### Level 4 — Cryptographically Verified

Device identity or its manifest can be cryptographically validated.

Trust level should be visible rather than implied.

---

## 10. Device Manifests

ODB may use machine-readable device manifests.

A manifest could describe:

- device identity
- processor architecture
- hardware revision
- interfaces
- capabilities
- supported protocols
- firmware formats
- bootloader behavior
- recovery procedures
- documentation
- risk information
- cryptographic identity information

Possible manifest sources include:

1. the device itself
2. the manufacturer
3. the operating system
4. an open device registry
5. a trusted community definition
6. a local user definition

The provenance of the manifest should remain visible.

---

## 11. ODB Profiles

ODB Core should not attempt to understand every possible hardware domain.

Device-specific semantics can instead be represented through composable profiles.

Possible profiles include:

- ODB Serial Profile
- ODB Firmware Profile
- ODB Sensor Profile
- ODB Imaging Profile
- ODB Radio Profile
- ODB Diagnostic Profile
- ODB Machine-Control Profile
- ODB Educational Hardware Profile

A device can expose multiple profiles.

For example, an ESP32-based LoRa board might expose:

- Identity
- Serial
- Firmware
- Radio
- Sensor

A thermal camera might expose:

- Identity
- Imaging
- Sensor

An OBD-II adapter might expose:

- Identity
- Diagnostics
- Firmware

Profiles allow the architecture to expand without placing every device-specific concept into ODB Core.

---

## 12. Firmware as a Transaction

Firmware installation should not be treated simply as sending bytes to a device.

ODB can model firmware installation as a transaction:

1. Identify device.
2. Determine hardware revision.
3. Validate firmware target.
4. Validate firmware source or signature when available.
5. Explain the operation.
6. Request authorization.
7. Preserve recoverable state when possible.
8. Enter bootloader or update mode.
9. Transfer firmware.
10. Verify the written image.
11. Restart the device.
12. Confirm expected device response.
13. Record the result.

Failure should produce a recoverable diagnostic state whenever the hardware permits it.

---

## 13. Recovery as a First-Class Capability

Recovery should not be an afterthought.

ODB may describe:

- bootloader entry
- DFU modes
- button sequences
- reset behavior
- expected recovery interfaces
- firmware restoration
- configuration backup
- safe retry procedures

A user interface can then translate implementation details into guided recovery.

For example:

> Hold BOOT.

> Press RESET.

> Recovery mode detected.

> Release BOOT.

This does not remove technical complexity.

It represents that complexity through a structured interface.

---

## 14. Progressive Disclosure

ODB should serve both inexperienced users and technical professionals.

A user interface could provide multiple levels.

### Normal

- Identify
- Configure
- Update
- Diagnose

### Advanced

- serial console
- interfaces
- firmware partitions
- bootloader state
- device manifest

### Developer

- descriptors
- endpoints
- protocol diagnostics
- debug capabilities
- low-level interface information

ODB should simplify routine interaction without preventing legitimate advanced access.

---

## 15. Education and STEM

ODB may be especially relevant to education.

A student may possess:

- a school-issued tablet
- an Arduino or ESP32 board
- sensors
- robotics hardware
- LoRa equipment
- a USB-C cable

The learning objective may be programming, electronics, measurement, radio, robotics, or experimentation.

Platform-specific driver installation and access limitations are often incidental complexity rather than the educational objective.

ODB seeks to reduce incidental interoperability barriers while preserving the meaningful complexity of engineering.

A guiding educational principle is:

> A student possessing a standards-compliant computing device and physically accessible educational hardware should not require ownership of a second class of general-purpose computer solely to identify, inspect, configure, or program that hardware when those operations can be performed safely by the existing device.

---

## 16. Field Science and Instrumentation

A field researcher may carry a tablet alongside:

- GNSS equipment
- environmental probes
- thermal cameras
- microscopes
- LoRa nodes
- measurement instruments

These devices may currently require separate proprietary applications or a laptop.

ODB could allow applications to request authorized capabilities from multiple instruments through a common security framework.

The tablet could then function as a field instrumentation platform rather than merely a display for individual vendor applications.

---

## 17. Accessibility

Assistive hardware may include:

- switches
- alternative input devices
- environmental controls
- specialized sensors
- communication hardware

A capability model could reduce the need for every assistive-device manufacturer and application developer to create bespoke integrations with one another.

Accessibility use cases require additional research and should be developed with affected users and accessibility specialists.

---

## 18. Repairability and Right-to-Interface

Modern repair increasingly requires software operations.

A physically repaired device may still require:

- diagnostics
- calibration
- pairing
- configuration
- commissioning
- reset
- firmware installation

ODB introduces the broader concept of a **right-to-interface**:

The ability for an owner or authorized technician to request secure, documented access to legitimate diagnostic and maintenance capabilities of user-owned hardware.

ODB does not imply unrestricted access or circumvention of legitimate safety controls.

It proposes a technical framework through which authorized interaction can occur.

---

## 19. Device Longevity

Hardware may remain functional after:

- a manufacturer closes
- a cloud service disappears
- an application is removed
- vendor support ends

When essential device functions depend entirely on a proprietary application or remote service, otherwise functional hardware may become unusable.

Open capability descriptions, local interfaces, and documented device manifests could improve:

- device longevity
- repairability
- preservation
- sustainability
- electronic-waste reduction

---

## 20. Security Model

ODB is not intended to weaken operating-system security.

Physical possession does not automatically grant applications unrestricted access.

The OS may evaluate:

- device identity
- device trust
- application identity
- requested capability
- risk class
- user authorization
- device state
- enterprise policy
- platform policy

before creating a session.

Applications receive only the capabilities authorized for that session.

Access can subsequently be revoked.

---

## 21. Negotiated Trust

Some hardware may require authorization from both sides.

A high-security interaction could involve:

1. OS verifies application.
2. Application requests capability.
3. User authorizes request.
4. Device verifies host or session.
5. Device approves requested capability.
6. Secure session begins.

This may be relevant to industrial systems, secure storage, vehicles, or other sensitive equipment.

---

## 22. Benefits to Hardware Manufacturers

ODB must provide value to manufacturers as well as users.

Hardware manufacturers currently may need to maintain combinations of:

- Windows drivers
- macOS applications
- Android applications
- iOS applications
- Bluetooth implementations
- Wi-Fi implementations
- cloud infrastructure
- firmware update utilities

A standardized capability and manifest system could allow manufacturers to publish device definitions and supported operations while multiple authorized applications provide user interfaces.

This could reduce duplicated integration work.

---

## 23. Benefits to Open-Source Projects

Open-source hardware and firmware projects could publish ODB profiles describing their capabilities.

For example, a radio project might define:

- `node.info`
- `radio.configure`
- `message.send`
- `telemetry.read`
- `firmware.flash`
- `recovery`

Different applications and operating systems could implement those conceptual capabilities over different transports.

This creates a path toward cross-platform hardware interoperability without requiring identical low-level implementations.

---

## 24. Initial ODB 0.1 Scope

The first reference implementation should remain deliberately narrow.

Reference hardware:

**ESP32-S3 / Heltec development hardware**

Initial capabilities:

- `device.identify`
- `serial.read`
- `serial.write`
- `firmware.flash`

Initial objectives:

1. Detect the connected device.
2. Establish authorized communication.
3. Identify the processor/device.
4. Expose useful diagnostic information.
5. Demonstrate capability-scoped authorization.
6. Perform only explicitly authorized operations.
7. Verify the result.

Additional hardware families can be introduced only after the core model is demonstrated.

---

## 25. Architectural Principle

ODB begins with a simple observation:

A physical connection, compatible hardware, and sufficient computing power do not necessarily produce usable interoperability.

The missing layer is often not electrical.

It is the relationship between:

- identity
- capability
- authorization
- trust
- protocol
- transport
- user intent

Open Device Bridge proposes making that relationship explicit.

> **Connect → Identify → Explain → Authorize → Interact → Verify → Revoke**
