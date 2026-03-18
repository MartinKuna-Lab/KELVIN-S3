# KELVIN S3 v1.5.0 — Industrial Cooling Controller Platform (ESP32-S3)

**KELVIN S3** is an ESP32-S3 based industrial cooling controller platform for compressor radiators, multi-fan cooling systems, and service-oriented machine cooling automation.

It is designed for real industrial applications where simple thermostat logic is not enough.  
The goal is clear: **stable thermal control, deterministic output behavior, robust safety handling, and transparent diagnostics** in real service conditions.

---

## What KELVIN S3 is

KELVIN S3 is not a hobby cooling thermostat.  
It is a machine-oriented control platform intended for installations such as:

- compressor cooling skids
- multi-fan radiator assemblies
- air coolers and industrial heat exchangers
- systems with staged fan control
- installations with reverse airflow / cleaning cycle
- systems with alarm, unload, flap, pump, and auxiliary logic
- service-friendly cooling systems with Web UI and diagnostics

Typical deployment example:

> **industrial radiator with 6 to 12 fans, staged cooling, reverse handling, pump/flap coordination, diagnostics, and expandable auxiliary logic**

---

## Version focus — v1.5.0

**KELVIN S3 v1.5.0** reflects the current architecture direction of the project:

- target platform is based on **ESP32-S3**
- fan and output handling are organized around a **32-channel relay architecture**
- **relays 1–24** are reserved for core internal system functions
- **relays 25–32** are exposed as **AUX1–AUX8**
- **adiabatic / spray cooling belongs to the main control logic**, not AUX
- **AUX is a read-only reactive engine** based on exported internal values
- Service UI includes a dedicated **AUX tab**

This version is focused on maintaining a clear separation between:
- **core machine cooling logic**
- **safety and interlocks**
- **service diagnostics**
- **reactive auxiliary behavior**

---

## Typical application

KELVIN S3 is intended for systems such as:

- compressor cooling radiators
- multi-fan dry coolers
- oil cooler and aftercooler assemblies
- industrial cooling loops with pump support
- systems requiring reverse cleaning cycle
- machines with alarm/interlock behavior
- installations requiring service visibility and remote diagnostics

---

## Current architecture

### Master controller
- **ESP32-S3** based control unit
- built-in **Web UI**
- JSON backend for diagnostics and state synchronization
- non-volatile configuration storage
- modular firmware architecture focused on control logic and serviceability

---

### Relay architecture
KELVIN S3 v1.5.0 uses a **32-channel output concept**.

#### Internal system outputs
- **Relay 1 to Relay 24** are reserved for internal system functions

These outputs may be used by the firmware for:
- staged fan control
- alarm logic
- unload/interlock handling
- flap / water valve logic
- reverse logic
- spray / adiabatic support
- pump outputs
- future internal machine functions

Exact assignment may depend on the hardware build and firmware configuration.

#### Auxiliary outputs
- **Relay 25 = AUX1**
- **Relay 26 = AUX2**
- **Relay 27 = AUX3**
- **Relay 28 = AUX4**
- **Relay 29 = AUX5**
- **Relay 30 = AUX6**
- **Relay 31 = AUX7**
- **Relay 32 = AUX8**

AUX outputs are designed as **reactive outputs** driven by logic conditions derived from exported system values.

---

## Inputs and sensing

Depending on firmware build and hardware variant, KELVIN S3 may use:

- multiple temperature channels
- PT100-based sensing architecture
- ambient temperature input
- digital status / interlock inputs
- optional pressure, voltage, or current-related monitoring extensions
- service-oriented internal status exports for logic and diagnostics

The sensing architecture is designed to support real machine cooling decisions rather than single-threshold thermostat behavior.

---

## Core functions

### Multi-stage fan control
KELVIN S3 controls radiator fans in multiple stages according to thermal demand and selected operating mode.

Features include:
- configurable fan count
- configurable stage logic
- deterministic stage transitions
- AUTO and MANUAL operation
- support for larger fan arrays
- separation of control logic from UI rendering

The system is intended for radiator layouts where predictable staging matters more than simple ON/OFF switching.

---

### Fan rotation logic
To reduce uneven wear, KELVIN S3 supports controlled fan rotation.

Goals:
- distribute runtime more evenly
- reduce wear on individual fans, relays, and contactors
- keep thermal behavior balanced over long-term operation

Rotation is handled in backend control logic, not as a dashboard-side effect.

---

### Spray / adiabatic cooling
KELVIN S3 supports **SPRAY** as a controlled high-stage cooling function.

Important in v1.5.0:
- **SPRAY is part of the core cooling logic**
- it is **not** treated as AUX
- it is activated only under defined operating conditions
- it follows thermal and safety rules of the main system

This keeps adiabatic support inside the deterministic cooling path.

---

### Reverse and SafetyLock
The system includes controlled **REVERSE** behavior together with **SafetyLock** protection logic.

This part of the firmware is designed so that reverse operation is:

- state-dependent
- protected against unsafe transitions
- deterministic after stop or mode changes
- clearly visible in diagnostics
- compatible with controlled restart behavior

This is one of the most important safety-oriented sections of the project.

---

### Pump and flap logic
KELVIN S3 supports coordinated coolant-side logic including:

- pump outputs
- flap / water valve logic
- afterrun behavior
- interlock consistency between hydraulic outputs

Design rules include:
- pumps must not run against a closed flap
- flap must be open whenever pump operation is active
- afterrun must remain logically consistent
- output behavior must stay deterministic during transitions and fault states

---

### Safety and fault handling
KELVIN S3 is designed around explicit and deterministic fault behavior.

Depending on detected condition and configuration, the controller can:

- activate alarm signaling
- trigger unload or machine interlock behavior
- block unsafe actions
- restrict output combinations
- enter protected state
- enter lockout / alarm condition
- expose diagnostic state through JSON and service UI

The design goal is not “hidden fallback behavior”, but **clear service visibility and predictable reaction**.

---

## Operating modes and profiles

KELVIN S3 supports operating logic such as:

- **AUTO**
- **MANUAL**
- profile-based cooling behavior
- configurable thermal response strategies

Depending on firmware build, available profiles may include:

- **POWER**
- **OPTIMAL**
- **ECO**
- **CUSTOM**
- **AUTO**

These profiles may influence:
- fan stage thresholds
- cooling aggressiveness
- rotation behavior
- spray permission logic
- automatic profile selection behavior

Exact profile implementation may evolve between firmware revisions.

---

## AUX engine

### AUX philosophy in v1.5.0
KELVIN S3 v1.5.0 introduces a clearer definition of AUX behavior.

AUX is **not** intended to replace core machine logic.  
Instead, AUX acts as a **read-only reactive engine** based on exported internal values.

This means AUX logic can react to system states such as:
- temperatures
- cooling stage
- reverse state
- alarm state
- mode/profile state
- service conditions
- other exported variables

But AUX does **not** own the core cooling process.

This keeps the architecture clean:

- **main firmware** = machine cooling and safety logic
- **AUX engine** = reactive side behavior and extension logic

---

### AUX outputs
KELVIN S3 provides:

- **AUX1**
- **AUX2**
- **AUX3**
- **AUX4**
- **AUX5**
- **AUX6**
- **AUX7**
- **AUX8**

These outputs are intended for controlled auxiliary scenarios such as:
- signaling
- external enable chains
- helper outputs
- extension logic
- installation-specific reactions

Exact use depends on the machine and project configuration.

---

## Web UI and diagnostics

KELVIN S3 includes a built-in web dashboard used for:

- live temperature monitoring
- mode and profile overview
- fan and output state visualization
- warning and alarm visibility
- service/testing functions
- configuration pages
- diagnostic state inspection
- trend/history features (build dependent)
- AUX status and logic visibility

The dashboard is driven by a **JSON backend**, which acts as the primary source of truth for UI state.

### Service UI
The service-oriented part of the UI is important to the project philosophy.

In current architecture, service features may include:
- I/O state visibility
- diagnostics and quick checks
- output testing tools
- status explanation
- internal exported values
- dedicated **AUX tab**

The goal is practical real-world service usability, not decorative UI.

---

## Firmware design goals

Current KELVIN S3 development is focused on:

- deterministic control behavior
- robust backend logic
- clear separation of UI and control state
- stable JSON-driven dashboard operation
- reliable interlocks and safety handling
- maintainable industrial-style firmware structure
- modular growth without breaking core logic
- service visibility during commissioning and diagnostics

---

## Typical high-level states

Depending on firmware build, the system may operate through states such as:

- **IDLE / STANDBY**
- **AUTO RUN**
- **MANUAL**
- **STAGED COOLING**
- **SPRAY ACTIVE**
- **REVERSE**
- **SAFETYLOCK**
- **ALARM / LOCKOUT**
- **SERVICE**
- **AUX REACTIVE STATES**

Exact behavior depends on enabled modules and current firmware revision.

---

## Project status

This repository contains the active firmware platform for **KELVIN S3 v1.5.0**.

Current development focus includes:

- control stability
- fan/output correctness
- safety and interlocks
- service-oriented diagnostics
- Web UI consistency
- AUX integration and clarity
- maintainable industrial firmware architecture

> **Warning**  
> This is industrial control firmware.  
> Always validate wiring, relay mapping, interlocks, sensor readings, state transitions, and fault reactions on a controlled test setup before deployment on live equipment.

---

## License

This project is released under the **KELVIN-NC (Non-Commercial)** license.

Commercial use requires written permission from the author.