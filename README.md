# Open Device Bridge

### Connect. Authorize. Interact.

Open Device Bridge (ODB) is a project exploring secure, user-authorized access to physical hardware from modern mobile computers.

## The problem

Modern tablets such as the iPad have USB-C, substantial computing power, networking, and sophisticated web applications.

Yet many development devices still require a traditional desktop computer simply to identify, configure, recover, or program them.

Our initial reference case exposed this gap:

**iPad → USB-C → Heltec WiFi LoRa 32 V4 / ESP32-S3 → Meshtastic Web Flasher**

The hardware connects and the ESP32-S3 can enter its ROM bootloader, but Safari on iPadOS does not provide the Web Serial transport required by the browser-based flashing workflow.

## The idea

ODB explores a secure architecture:

Connect → Identify → Explain → Authorize → Interact → Revoke

The goal is not unrestricted USB access.

The operating system remains the security authority. Access should be explicit, scoped, visible, and revocable.

Potential capabilities could include:

- Device identification
- Diagnostics
- Serial read/write
- Configuration
- Firmware operations with elevated authorization

## First reference implementation

The first target is deliberately narrow:

**iPadOS + USB-C + ESP32-S3**

An initial proof of concept should only:

1. Detect the connected device.
2. Establish authorized communication.
3. Synchronize with the ESP32-S3 ROM bootloader.
4. Identify the chip.
5. Report diagnostic information.

No flash erase, firmware writing, eFuse modification, or other irreversible operation is required for the first proof of concept.

## Why this matters

ODB could eventually support development and provisioning workflows involving:

ESP32 • Arduino-class hardware • Meshtastic • IoT • robotics • STEM education • amateur radio • field equipment • sensors • laboratory hardware

The long-term objective is simple:

> A secure bridge between the mobile computer people already own and the physical hardware they want to use.

## Project status

**Early concept / architecture stage.**

The project is currently exploring technical feasibility, security architecture, platform APIs, and an ESP32-S3 reference implementation.

Contributions, technical discussion, prior-art references, and platform expertise are welcome.

---

**Open Device Bridge**

*Connect. Authorize. Interact.*

*ESP32 first. Open hardware next.*

---

## Broader Architecture

The ESP32-S3/iPad experiment is the initial reference case for a broader interoperability problem.

Modern computing platforms can communicate with sophisticated external hardware, but access remains fragmented across device classes, proprietary applications, transport-specific workarounds, platform-specific drivers, and vendor ecosystems.

Comparable device categories include:

- Arduino and other microcontroller platforms
- ESP32 and Meshtastic/LoRa hardware
- Thermal imaging systems such as FLIR
- OBD-II automotive diagnostic hardware
- Software-defined radio (SDR)
- Oscilloscopes and digital multimeters
- USB microscopes and imaging instruments
- GNSS/GPS receivers
- Amateur-radio equipment
- 3D printers and CNC controllers
- Robotics and flight controllers
- Environmental sensors
- Scientific and laboratory instrumentation
- Industrial diagnostic and configuration equipment

ODB is therefore being developed not as a workaround for one ESP32 board, but as a general model for secure interaction between computing platforms and user-owned external hardware.

## Proposed ODB Model

Open Device Bridge is a proposed operating-system-mediated framework for secure, user-authorized, capability-scoped interaction between computing platforms and user-owned external hardware.

The proposed lifecycle is:

> **Connect → Identify → Explain → Authorize → Interact → Verify → Revoke**

Physical connection alone does not grant unrestricted access.

Instead, applications request specific capabilities and the operating system remains the enforcement authority.

Example capabilities may include:

- `device.identify`
- `sensor.read`
- `serial.read`
- `serial.write`
- `configuration.read`
- `configuration.write`
- `firmware.flash`
- `recovery`
- `debug`
- `radio.control`
- `diagnostics.read`
- `machine.control`

Higher-risk capabilities can require stronger authorization.

## Architectural Principles

ODB currently rests on four primary principles:

**Open** — Hardware access should not automatically depend on a proprietary vendor application.

**User Authorized** — Physical connection establishes an opportunity to request access, not permission by itself.

**Capability Scoped** — Applications receive only the hardware operations required for the authorized task.

**OS Enforced** — The operating system remains the ultimate authority over access, isolation, revocation, and security policy.

## Transport Independence

ODB is intended to describe hardware interaction rather than a single connection technology.

Potential transports include:

- USB / USB-C
- Thunderbolt
- Bluetooth / BLE
- Wi-Fi
- Ethernet
- NFC
- Future transports

The intended abstraction is:

> **Intent → Capability → Authorization → Session → Protocol → Transport**

A user may want to configure, diagnose, program, or recover a device regardless of whether that interaction occurs over USB, Bluetooth, or a network connection.

## Core and Profiles

The architecture is expected to separate a small **ODB Core** from composable **ODB Profiles**.

The ODB Core would define concepts such as:

- device identity
- discovery
- capability negotiation
- authorization
- trust
- sessions
- verification
- revocation
- auditing
- risk classification

Profiles could define domain-specific behavior, including:

- Serial Profile
- Firmware Profile
- Sensor Profile
- Imaging Profile
- Radio Profile
- Diagnostic Profile
- Machine-Control Profile
- Educational Hardware Profile

A single device may expose several profiles.

For example, an ESP32-based LoRa device could expose identity, serial, firmware, radio, and sensor capabilities without requiring those concepts to be hard-coded into the core specification.

## Initial Reference Implementation

The first ODB reference implementation will remain deliberately small.

Initial target:

**ESP32-S3 / Heltec hardware**

Initial capabilities:

- `device.identify`
- `serial.read`
- `serial.write`
- `firmware.flash`

The existing experiments in this repository document the boundary conditions that motivated this work.

The goal is to determine whether the same security and authorization architecture can later extend to additional hardware classes without weakening platform security.

## Educational and Long-Term Significance

ODB may have implications beyond developer convenience.

Potential areas include:

- STEM education
- maker and Arduino ecosystems
- field science
- accessibility hardware
- vocational and technical education
- amateur radio
- repairability
- device longevity
- electronic-waste reduction
- open hardware
- preservation of locally functional devices after vendor software or cloud services disappear

A student, technician, researcher, maker, or device owner should not necessarily require a second class of general-purpose computer solely to identify or safely interact with hardware when their existing computing platform is technically capable of performing the operation.

ODB does not propose unrestricted hardware access.

It proposes a structured security model for hardware access.

> **Connection should initiate negotiation — not assumption, unrestricted access, or a dead end.**


