# Experiment 001 — iPadOS USB-C / ESP32-S3 Access

## Purpose

Document a real-world attempt to identify and interact with an ESP32-S3 development device directly from an iPad using USB-C.

This experiment is the first reference case for Open Device Bridge (ODB).

## Test environment

- Host: iPad Pro 13-inch (M4)
- Operating system: iPadOS 26.3.1
- Browser: Safari
- Connection: USB-C
- Target hardware: Heltec WiFi LoRa 32 V4 / ESP32-S3
- Application: Meshtastic Web Flasher
- Physical connection: Direct USB-C data connection
- LoRa antenna attached during testing

## Procedure

1. Connect the ESP32-S3 device to the iPad through USB-C.
2. Confirm that the device receives power.
3. Attempt normal device operation.
4. Place the ESP32-S3 into its ROM bootloader/recovery state.
5. Open the Meshtastic Web Flasher in Safari.
6. Attempt browser-based device detection and communication.
7. Record the behavior of the hardware, browser, and operating system.

## Observations

The ESP32-S3 receives power from the iPad.

The device can be placed into its bootloader/recovery state.

Safari successfully loads the Meshtastic Web Flasher.

The web application reports:

> Your browser does not support the WebSerial API.

The physical USB connection therefore exists, but the browser application does not receive the serial interface required to communicate with and flash the ESP32-S3.

## Result

**Physical connection: successful.**

**Device power: successful.**

**ESP32-S3 bootloader access: successful.**

**Web application loading: successful.**

**Browser-to-device serial communication: unavailable.**

The experiment demonstrates a boundary between physical USB connectivity and application-level access to the connected hardware.

## ODB relevance

This is the capability gap that Open Device Bridge is intended to investigate.

ODB does not propose unrestricted USB access.

The proposed security model is:

**Connect → Identify → Explain → Authorize → Interact → Revoke**

The operating system remains the security authority while allowing the user to explicitly grant narrowly scoped capabilities to an application.

Possible capabilities include:

- Device identification
- Diagnostics
- Serial read
- Serial write
- Configuration
- Firmware operations with elevated authorization
- Explicit revocation of access

## Experiment 002 hypothesis

A controlled intermediary may be able to expose device identity and limited communication to an iPad application without granting unrestricted hardware access.

The next experiment should determine what device information iPadOS can expose through currently permitted interfaces before attempting firmware operations.

## Status

Experiment 001 documents the initial failure boundary.

**Result: useful failure.**

The hardware is connected and operational. The missing layer is authorized application access to the USB serial transport.
