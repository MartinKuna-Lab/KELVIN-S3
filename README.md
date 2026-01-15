# KELVIN-
ESP32-S3 industrial cooling controller: multi-fan staging, Web UI, SafetyLock, PID valve + shut-off damper.

# KELVIN S3 — Industrial Cooling Management Controller (ESP32-S3)

KELVIN S3 is an industrial-ready controller for multi-fan compressor cooling systems and air-to-water heat exchanger radiators. It is designed as a robust, PLC-like solution built around the ESP32-S3 platform with a clean Web UI, modular firmware architecture, safety-first logic, and service-friendly diagnostics.

The goal is simple: **stable temperatures, predictable behavior, and clear service visibility** — even in harsh compressor-room environments where “DIY style” controllers fail.

---

## What it controls

Typical deployment: large compressor cooling skid / radiator section with multiple fans, optional water spray, reverse/defrost mode, and optional coolant circuit control.

**Outputs (typical):**
- Multi-stage fan control (up to 6+ fans, grouped or individual)
- Spray / mist cooling output (optional)
- Reverse / cleaning / defrost output (optional)
- Unload / interlock output (compressor load reduction / protective interlock)
- Alarm relay output (hard fault / warning patterns)
- **Shut-off closing damper / closing flap** (airflow isolation / emergency closing / anti-backflow)

**Inputs (typical):**
- Temperature sensors (PT100 / digital sensors depending on build)
- Ambient temperature
- Optional flow / pressure / digital interlocks
- Optional voltage monitoring (VMON) per phase (L1/L2/L3) for diagnostics and protection logic

---

## Key features

### 1) Industrial control profiles
KELVIN S3 supports multiple operating profiles (e.g. POWER / OPTIMAL / ECO / CUSTOM / AUTO), allowing the same controller to be tuned for different radiators, fan layouts, or energy policies. Profiles define:
- fan staging thresholds (ΔT based)
- rotation strategy and balancing
- spray logic (priority rules, delays, locks)
- reverse constraints (e.g., never during full load)
- safety reactions and alarm handling

### 2) Fan rotation & balancing (anti-wear logic)
Instead of running the same fan(s) forever, KELVIN S3 can rotate active fan groups in controlled patterns to:
- equalize runtime hours
- reduce mechanical wear
- keep airflow performance more consistent over time

This is a practical “industrial reality” feature: it extends fan lifetime and reduces service calls.

### 3) SafetyLock and protective modes
A dedicated safety layer supervises sensor plausibility and system states. Depending on the fault severity, KELVIN S3 can:
- trigger warning patterns (non-critical)
- reduce load (Unload relay)
- enter lockout (hard faults)
- keep minimum cooling if needed (config dependent)

Designed so that faults are **observable and deterministic** — not random resets.

### 4) PID mixing valve control (coolant / water temperature stabilization)
KELVIN S3 includes an optional **PID controller for a mixing valve** (PWM controlled actuator).  
Use cases:
- stabilize water/coolant temperature entering a heat exchanger
- prevent thermal shock
- improve efficiency by holding a target temperature despite variable ambient and compressor load

Highlights:
- PID parameters stored in non-volatile memory (service-friendly)
- live values available in JSON status
- remote tuning via REST endpoint (when enabled)

### 5) Shut-off damper / closing flap support
The controller supports a **closing damper (shut-off flap)** used to isolate airflow when required, for example:
- preventing unwanted airflow / backflow when fans are off
- isolating the radiator section during fault / emergency
- improving thermal control in specific installations (ducting, outdoor units)

The flap logic is integrated with system state (RUN / STOP / FAULT / LOCKOUT) so it behaves predictably.

### 6) Web UI + JSON API (service visibility)
KELVIN S3 provides a fast, clear Web UI for:
- live temperatures and status
- active profile + current stage/step
- relays / outputs overview
- alarms / warnings and reason codes
- tuning and configuration (depending on build)

For integration and diagnostics, the firmware exposes:
- `/json` status output (single source of truth for UI)
- REST endpoints for configuration blocks (profile/pid/etc., depending on enabled modules)

---

## Hardware platform

KELVIN S3 is built on **ESP32-S3**, designed for “industrial DIY” hardware like relay boards (DIN-rail enclosures, shielded wiring, real terminals). Typical builds use:
- relay outputs for fans and auxiliary functions
- sensor front ends (PT100 via MAX31865 etc.)
- optional 3-phase voltage monitoring modules (VMON)

Exact pinout depends on the target board and wiring harness.

---

## Typical state logic (high level)

- **IDLE / STANDBY**: monitoring, ready state
- **RUN**: staged cooling with rotation strategy
- **BOOST / POWER stages**: maximum cooling behavior with constraints
- **REVERSE**: controlled reverse cycle when conditions allow
- **LOCKOUT**: hard fault state, outputs set to safe positions, clear alarm behavior
- **SERVICE / MANUAL** (optional): controlled output testing

---

## Why KELVIN S3 (vs common HVAC controllers)

Many off-the-shelf controllers are excellent for simple thermostatic regulation, but they usually lack:
- advanced fan rotation logic
- predictable safety state machine with service diagnostics
- multi-profile behavior tuned to industrial radiators
- integrated PID mixing valve control
- system-level interlocks (Unload/Alarm) designed for compressor skids

KELVIN S3 is built for real compressor rooms: **clear logic, reliability, and service-first design.**

---

## Project status

This repository contains firmware for the KELVIN S3 controller:
- modular architecture (clean separation of logic/UI/hardware)
- Web dashboard + JSON backend
- profile engine, safety logic, PID mixing valve module, damper control (build dependent)

> ⚠️ Note: This is industrial control firmware. Always validate wiring, relays, interlocks, and safety reactions in a controlled environment before deploying on production equipment.



## License
License: Proprietary (All rights reserved).
