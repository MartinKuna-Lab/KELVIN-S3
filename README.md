KELVIN S3 is an ESP32-S3 based industrial cooling controller for compressor radiators and multi-fan cooling systems.  
It combines staged fan control, rotation logic, spray support, reverse handling, pump/flap coordination, safety interlocks, and a Web UI with JSON diagnostics into a service-friendly machine control platform.




# KELVIN S3 — Industrial Cooling Controller (ESP32-S3)

**KELVIN S3** is an industrial-oriented control system for compressor cooling and multi-fan radiator management, built on the **ESP32-S3** platform.

It is designed for real machine cooling applications where simple thermostat logic is not enough.  
The goal is straightforward: **stable thermal control, predictable output behavior, clear diagnostics, and robust safety logic** for compressor rooms and similar industrial installations.

---

## Typical application

KELVIN S3 is intended for systems such as:

- compressor cooling skids
- multi-fan radiator sections
- air coolers and heat exchanger radiator assemblies
- installations with optional spray / adiabatic cooling
- systems requiring reverse airflow / cleaning cycle
- cooling systems with alarm, unload, flap, and pump outputs

Typical deployment:  
**a compressor cooling radiator with 6 to 12 fans, staged cooling logic, auxiliary outputs, and service-oriented diagnostics**

---

## Current architecture

### Master controller
- **ESP32-S3** based control unit
- built-in Web UI and JSON backend
- non-volatile configuration storage
- modular firmware focused on control logic, diagnostics, and serviceability

### Master relay outputs
Current KELVIN S3 architecture uses the following relay mapping on the master module:

- **RO1 = ALARM**
- **RO2 = UNLOAD**
- **RO3 = WATER VALVE / FLAP**
- **RO4 = SPRAY**
- **RO5 = REVERSE**
- **RO6 = PUMP1**
- **RO7 = PUMP2**
- **RO8 = PUMP3**

### Fan outputs
Fans are controlled through an external **Modbus RTU relay slave**:

- **FAN1 to FAN12** on slave relay outputs
- remaining outputs reserved for future expansion

### Inputs and sensing
Depending on build and hardware variant, KELVIN S3 uses:

- multiple temperature channels
- PT100-based sensing architecture
- ambient temperature input
- optional digital interlocks and fault signals
- optional monitoring extensions

---

## Core functions

### Multi-stage fan control
KELVIN S3 controls radiator fans in multiple stages according to thermal conditions and selected operating profile.

Features include:
- configurable fan count and stage behavior
- staged cooling response
- AUTO and MANUAL operation
- support for larger fan arrays through Modbus expansion

---

### Fan rotation logic
To reduce uneven wear, the controller supports controlled fan rotation.

This helps to:
- distribute runtime more evenly
- reduce wear on individual fans and relays/contactors
- keep long-term operation more balanced

Rotation is handled in control logic, not as a UI-side effect.

---

### Spray cooling
KELVIN S3 supports an auxiliary **SPRAY** output for adiabatic / evaporative cooling.

Spray is treated as a controlled high-stage cooling function with operating constraints, not as an always-free parallel output.

---

### Reverse and SafetyLock
The system includes controlled **REVERSE** behavior together with **SafetyLock** protection logic.

This part of the firmware is designed so that reverse operation is:

- state-dependent
- protected against unsafe transitions
- deterministic after fan stop or mode changes
- clearly visible in diagnostics

This is one of the key safety-oriented parts of the project.

---

### Pump and flap logic
KELVIN S3 supports up to **3 cooling pumps** including **afterrun** behavior.

Pump operation is tied to the **flap / water valve** logic:

- pumps must not run against a closed flap
- flap must be open whenever pump operation is active
- afterrun must remain logically consistent with flap state

This is essential for safe coolant-side operation.

---

### Safety and fault handling
KELVIN S3 is designed around deterministic fault behavior.

Depending on the detected condition and firmware configuration, the controller can:

- activate the ALARM output
- trigger UNLOAD / interlock behavior
- block unsafe actions
- keep only explicitly allowed outputs active
- enter protected or lockout state

The design goal is clear service visibility and predictable reactions — not hidden fallback behavior.

---

## Operating profiles

KELVIN S3 supports multiple operating profiles such as:

- **POWER**
- **OPTIMAL**
- **ECO**
- **CUSTOM**
- **AUTO**

These profiles allow the same platform to be adapted to different radiator layouts, machine behavior, and cooling priorities.

Depending on firmware version, profiles may influence:
- fan staging thresholds
- rotation behavior
- spray permissions
- cooling aggressiveness
- automatic profile selection logic

---

## Web UI and diagnostics

KELVIN S3 includes a built-in web dashboard for:

- live temperatures
- mode and profile overview
- fan and relay/output state visualization
- alarm and warning display
- service and testing functions
- configuration pages
- trend graphs and history functions (build dependent)

The dashboard is driven by a **JSON backend**, which serves as the primary source of truth for UI state and diagnostics.

---

## Firmware design goals

The current KELVIN S3 development is focused on:

- deterministic control behavior
- separation of backend logic and UI rendering
- stable JSON-driven dashboard operation
- robust safety and interlock handling
- service-friendly diagnostics
- modular growth without breaking core control logic

KELVIN S3 is not intended to be a hobby thermostat project.  
It is a machine-oriented control platform for real service conditions.

---

## Typical high-level states

The system operates through states such as:

- **IDLE / STANDBY**
- **AUTO RUN**
- **MANUAL**
- **STAGED COOLING**
- **SPRAY ACTIVE**
- **REVERSE**
- **SAFETYLOCK**
- **ALARM / LOCKOUT**
- **SERVICE**

Exact state behavior depends on firmware version and enabled modules.

---

## Project status

This repository contains the active firmware platform for **KELVIN S3**.

Current development is focused mainly on:

- control stability
- correctness of fan and output logic
- safety behavior and interlocks
- Web UI consistency
- diagnostics, graphs, and history improvements
- maintainable industrial-style firmware architecture

> **Warning**  
> This is industrial control firmware.  
> Always validate wiring, relay mapping, interlocks, sensor readings, and fault reactions on a controlled test setup before deploying it on live equipment.

---

## License

This project is released under the **KELVIN-NC (Non-Commercial)** license.

Commercial use requires written permission from the author.