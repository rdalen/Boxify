# 🔧 Adapter Plate

## 🧠 Purpose

The **Adapter Plate** is the mechanical interface between the PCB and the enclosure.

It is one of the fundamental concepts of the Boxify architecture.

Instead of connecting the enclosure directly to a specific PCB design, Boxify connects the enclosure to an **Adapter Plate**.

This creates a stable mechanical abstraction layer between the electronics and the enclosure.

```text id="6m7n4r"
PCB
 │
 │ PCB-specific geometry
 ▼
Adapter Plate
 │
 │ standardized mechanical interface
 ▼
Enclosure
```

This separation allows the PCB and enclosure to evolve independently.

---
<img width="1030" height="850" alt="image" src="https://github.com/user-attachments/assets/c482e30a-4df9-435a-a16c-d9599ea6d15e" />
<p align="center"><i>Adapter Plate with the PCB Library Entry</i></p>

---

# 🏗 Position in the Boxify Architecture

```text id="r0g2h5"
PCB / KiCad
     │
     │ detailed STEP
     ▼
PS KiCadStep  <--- PCB Library
     │
     │ PCB definition
     ▼
Adapter Plate
     │
     │ mechanical interface
     ▼
PS Enclosure  <--- Threaded Insert Library
     │
     │ user configuration
     ▼
Box Base + Box Lid
     │
     ▼
Assembly
```

The Adapter Plate is therefore the **bridge between the PCB definition and the enclosure generator**.

---

# 🔗 The Mechanical Abstraction Layer

The Adapter Plate hides PCB-specific implementation details from the enclosure.

The enclosure does not need to know:

- Which CAD system was used to design the PCB.
- How the PCB STEP model is structured.
- Which components are present.
- How the PCB Type in the PCB Library was created.

It only needs the mechanical information provided by the Adapter Plate.

This is a key Boxify design principle:

> **The enclosure depends on the mechanical interface, not directly on the electronics.**

---

# 🧩 Adapter Plate Responsibilities

The Adapter Plate is responsible for establishing the mechanical relationship between the PCB and the enclosure.

Typical responsibilities include:

- PCB positioning
- PCB mounting
- Mounting-hole locations
- PCB reference geometry
- Enclosure mounting references
- Adapter Plate dimensions
- Reference Mate Connectors
- Mechanical interface geometry

The exact implementation can evolve, but the responsibility remains the same:

> **Translate PCB-specific geometry into a stable enclosure interface.**

---

# 📐 PCB Interface

The PCB side of the Adapter Plate is driven by the PCB definition provided by **PS KiCadStep**.

The Adapter Plate uses the PCB references to establish the correct position and orientation of the PCB.

Typical references include:

- PCB outline
- Mounting holes
- PCB origin
- Mounting plane
- Reference Mate Connector
  
The KiCad STEP is positioned as part of the Derive operation.

Therefore, `pcbThickness` and `pcbMountAdjust` are resolved before the KiCad STEP is derived into the Adapter Plate.

The Adapter Plate therefore adapts automatically to the selected PCB and its Passenger.

---

# 🧱 Enclosure Interface

The enclosure side of the Adapter Plate provides stable references for the Box Base and Box Lid.

These references can include:

- Adapter Plate outline
- Mounting locations
- Thickness
- Lug positions
- Enclosure reference planes
- Other mechanical reference geometry

The enclosure uses these references to generate the surrounding walls and other enclosure features.

---

# 📏 Measured and Derived Geometry

Where possible, Adapter Plate information should be **measured or derived from existing geometry** rather than manually duplicated.

Measured from geometry
- PCB length
- PCB width
- mounting-hole positions
- hole diameter

Defined PCB information
- PCB thickness

For example:

```text id="v7t5a3"
PCB geometry
     │
     ├── PCB dimensions
     ├── Mounting holes
     └── Reference position
             │
             ▼
       Adapter Plate
             │
             ├── interface geometry
             ├── mounting references
             └── measured dimensions
                     │
                     ▼
                PS Enclosure
```

This reduces the number of independent dimensions that need to be maintained.

---

# ⚙️ Configuration

The Adapter Plate should contain **mechanical information**, not general enclosure configuration.

Some Adapter Plate-related values may be exposed as user configuration through **PS Enclosure**, for example:

- Distance between the PCB and the edge of the Adapter Plate
- PCB orientation
- Component side up or down
- Mounting side above or below the adapter plate

The important distinction is that the user makes these choices from **PS Enclosure**, while the Adapter Plate provides the geometry required to implement them.

The user should not normally need to edit the Adapter Plate directly.

---

# 🔄 Configuration Flow

The overall relationship is:

```text id="5brz4p"
                  User
                   │
                   │ configuration
                   ▼
             PS Enclosure
                   │
                   │ selected PCB
                   ▼
              PS KiCadStep
                   │
                   │ PCB definition
                   ▼
             Adapter Plate
                   │
                   │ mechanical interface
                   ▼
          Box Base + Box Lid
```

This keeps user configuration centralized while allowing the Adapter Plate to remain a reusable mechanical component.

---

# 🧭 Reference Strategy

Stable references are important because the Adapter Plate is used by downstream enclosure geometry.

References should therefore be:

- Predictable
- Clearly named
- Based on stable geometry
- Independent of unnecessary implementation details

Where possible, downstream features should reference the Adapter Plate rather than directly referencing imported PCB geometry.

This reduces the risk of broken references when the PCB or KiCad STEP model changes.

---

# 🔄 PCB Updates

One of the main benefits of the Adapter Plate architecture is that PCB updates can propagate through the system.

A typical update looks like:

```text id="6b3n2q"
KiCad PCB
    │
    ▼
New STEP
    │
    ▼
PS KiCadStep
    │
    ▼
Adapter Plate
    │
    ▼
PS Enclosure
    │
    ▼
Updated Base + Lid
```

If the PCB remains compatible with the existing mechanical interface, the enclosure architecture does not need to be redesigned.

If PCB dimensions or mounting locations change, the Adapter Plate adapts the new PCB definition to the enclosure.

---

# 🧠 Design Principles

## Mechanical Abstraction

The Adapter Plate hides PCB-specific details from the enclosure.

## Stable Interface

The Adapter Plate provides predictable references for downstream enclosure geometry.

## Reusable

The same enclosure architecture can be used with different PCB definitions as long as they can be represented through the Adapter Plate interface.

## Derived Where Possible

Mechanical information should be measured or derived rather than duplicated.

## Minimal Coupling

The Adapter Plate should depend on the PCB definition, while the enclosure should depend primarily on the Adapter Plate.

## Centralized Configuration

User-facing configuration belongs in PS Enclosure rather than being distributed across Part Studios.

---

# 🎯 Result

The Adapter Plate allows Boxify to treat the electronics and enclosure as two separate design domains connected by a well-defined mechanical interface.

```text id="h7q4xm"
       ELECTRONICS DOMAIN
              │
              ▼
       PCB / KiCad
              │
              ▼
       PS KiCadStep
              │
              ▼
       ┌──────────────┐
       │   Adapter    │
       │    Plate     │
       └──────┬───────┘
              │
              ▼
       MECHANICAL DOMAIN
              │
              ▼
        PS Enclosure
              │
              ▼
       Box Base + Lid
```

The key principle is:

> **The Adapter Plate is the contract between the PCB and the enclosure.**

The PCB side defines the electronics and their mechanical requirements.

The enclosure side consumes a stable mechanical interface.

This separation is what allows Boxify to remain parametric, reusable and adaptable.

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)

---

**Happy Boxifying! 🚀**
