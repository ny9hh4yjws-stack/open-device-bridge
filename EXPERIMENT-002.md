# Experiment 002 — iPadOS Permitted Device Discovery

## Purpose

Determine what information iPadOS can expose about a physically connected ESP32-S3 device through currently permitted interfaces without requiring unrestricted USB serial access.

This experiment builds directly on Experiment 001, which demonstrated that physical USB-C connectivity can exist while application-level access to the USB serial transport remains unavailable.

The objective is to identify capabilities that could form the discovery and authorization layer of Open Device Bridge (ODB).

## Research Question

What can an iPad determine about a connected ESP32-S3 before privileged serial or firmware access is granted?

## Hypothesis

iPadOS may expose limited information about a connected USB device while withholding direct serial communication from ordinary applications or browser-based tools.

If useful device information can be obtained through permitted interfaces, Open Device Bridge may be able to use that information to create a controlled authorization process rather than requiring unrestricted USB access.

## Test Environment

- Host: iPad Pro 13-inch (M4)
- Operating system: iPadOS 26.3.1
- Browser: Safari
- Connection: USB-C
- Target hardware: Heltec WiFi LoRa 32 V4 / ESP32-S3
- Reference application: Meshtastic Web Flasher
- LoRa antenna attached during testing

## Baseline

Experiment 001 established the following:

1. The ESP32-S3 can be physically connected to the iPad through USB-C.
2. The connected device receives power.
3. The hardware remains operational.
4. The Meshtastic Web Flasher cannot obtain the USB serial access required for normal flashing.
5. Repeated clearing, cancellation, page refreshes, and reconnection do not remove this access boundary.

Experiment 001 therefore identified a distinction between physical USB connectivity and authorized application-level hardware access.

## Procedure

1. Connect the ESP32-S3 directly to the iPad using USB-C.
2. Confirm that the device receives power.
3. Observe any response from iPadOS when the device is connected.
4. Examine currently available iPadOS interfaces for evidence of the connected device.
5. Record any device identity, accessory information, connection state, or other metadata exposed by the operating system.
6. Attempt only non-destructive discovery operations.
7. Do not attempt firmware modification during this experiment.
8. Document successful and unsuccessful discovery methods.
9. Capture screenshots or photographs when useful.
10. Compare the results with the failure boundary documented in Experiment 001.

## Information of Interest

Where available, attempt to determine whether iPadOS exposes any of the following:

- Device presence
- USB device class
- Manufacturer
- Product name
- Vendor ID
- Product ID
- Serial number
- Interface information
- Accessory status
- Connection state
- Power state
- Available communication capabilities

Not all of this information is expected to be accessible.

## Security Principle

Open Device Bridge should not depend on unrestricted hardware access.

The intended authorization model is:

**Connect → Identify → Explain → Authorize → Interact → Revoke**

Discovery and identification should require the minimum access necessary.

More powerful operations should require progressively stronger and explicit authorization.

Potential capability levels include:

- Device identification
- Diagnostics
- Serial read
- Serial write
- Device configuration
- Firmware operations

The operating system should remain the enforcement authority.

## Success Criteria

Experiment 002 will be considered successful if at least one useful characteristic of the physically connected ESP32-S3 can be discovered through an interface currently permitted by iPadOS.

A negative result is also useful if it establishes that iPadOS exposes no practical device identity to the tested application layer.

## Expected Outcome

This experiment should define the next boundary for Open Device Bridge:

**What information is available before privileged hardware access begins?**

The result will help determine whether ODB can build a controlled discovery and authorization layer using existing iPadOS capabilities or whether an additional trusted intermediary is required.

## Status

**Experiment in progress.**

Results will be recorded after testing.
