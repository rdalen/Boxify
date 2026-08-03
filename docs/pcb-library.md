# 📄 PCB Library

## 🧠 Purpose

The PCB Library defines the electronic assembly that will be used throughout the Boxify framework.

Each PCB is represented by a reusable Part Studio that serves as the **single source of truth** for all PCB-related geometry and reference information.

A PCB Library entry may be:

- A standard prototype PCB generated parametrically
- A custom PCB imported from an ECAD tool such as KiCad

Regardless of its origin, every PCB is presented to the rest of Boxify through the same interface.

---

# 📦 Responsibilities

The PCB Library is responsible for defining:

- PCB geometry
- PCB thickness
- Mounting hole locations
- PCB dimensions
- Reference Mate Connector
- Reference geometry used by downstream Part Studios

The PCB Library intentionally contains **no enclosure-specific features**.

---

# ⚙️ Configuration

Depending on the PCB type, configurable options may include:

- PCB selection
- PCB rotation
- Component side
- Mounting side

For standard prototype PCBs, available sizes include:

- 20 × 80 mm
- 30 × 70 mm
- 40 × 60 mm
- 50 × 70 mm
- 60 × 80 mm
- 70 × 90 mm

---

# 📤 Outputs

Each PCB Library entry provides:

- PCB solid model
- Mounting holes
- Reference geometry
- Reference Mate Connector

These outputs are consumed by the Adapter Plate and ultimately by the enclosure.

---

# 🎯 Design Principles

The PCB Library follows several important principles:

- Every PCB has a consistent interface.
- PCB definitions are independent of the enclosure.
- Downstream Part Studios never modify PCB geometry.
- PCB changes automatically propagate through the Boxify framework.

This allows a PCB to be replaced or updated without redesigning the enclosure.
