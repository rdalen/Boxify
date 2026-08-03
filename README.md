# 🏗 Boxify

## 🧠 Purpose

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** for designing reusable, printable electronics enclosures.

The framework separates the **electronics** from the **mechanical enclosure** through the use of an **Adapter Plate**.

This allows a single generic enclosure to support many different electronic projects by simply replacing the Adapter Plate.

> **Van engineeren naar configureren.**
>
> *(Dutch for: "From engineering to configuration.")*

Boxify captures engineering knowledge in reusable building blocks, allowing future enclosure designs to be created primarily through **configuration rather than redesign**.

The framework currently supports a library of standard prototype PCBs and is designed to evolve towards direct integration with **KiCad STEP models** and reusable mechanical component libraries.

---

# 🏛 Architecture

```text
Electronics
     │
     ▼
PCB Library
     │
     ▼
Adapter Plate
     │
     ▼
Box Base
     │
     ├──────────────┐
     ▼              │
Threaded Insert     │
Library             │
     │              │
     └──────► Box Base
                    │
                    ▼
                 Box Lid
                    │
                    ▼
                 Assembly
```

Each building block has a single responsibility and depends only on the previous building block.

Reusable libraries encapsulate engineering knowledge and are instantiated where required.

---

# 🎯 Design Philosophy

The enclosure itself never knows which PCB is installed.

The **Adapter Plate** forms the interface between the electronics and the enclosure.

The Adapter Plate owns the complete installation of the electronics:

- PCB selection
- PCB position
- PCB orientation
- Mounting side
- PCB height

The enclosure only knows the Adapter Plate.

Mechanical details such as threaded inserts are encapsulated inside reusable libraries and are instantiated where needed.

This separation keeps both the electronics and the enclosure independent while maximizing reuse.

---

# 📚 Building Blocks

| Building Block | Responsibility |
|----------------|----------------|
| PCB Library | Defines the electronics |
| Adapter Plate | Adapts the electronics to the enclosure |
| Threaded Insert Library | Defines reusable fastening components |
| Box Base | Provides the reusable enclosure |
| Box Lid | Closes the enclosure |
| Assembly | Combines all parts |

---

# 📄 PCB Library

## 🧠 Purpose

Generates a configurable PCB.

Initially this library contains standard prototype PCBs.

Later it will support custom PCBs imported directly from KiCad as STEP models.

The PCB Library is the **single source of truth** for all PCB geometry.

## ⚙️ Configuration

Select the desired PCB type.

Examples:

- w2x8cm
- w3x7cm
- w4x6cm
- w5x7cm
- w6x8cm
- w7x9cm

## Outputs

- PCB outline
- PCB thickness
- Mounting holes
- Reference geometry
- Reference Mate Connector

---

# 📄 Adapter Plate

## 🧠 Purpose

The Adapter Plate forms the mechanical interface between the PCB Library ("the electronics") and the generic enclosure.

It determines:

- PCB position
- PCB orientation
- PCB mounting height
- Mounting side
- Mechanical interface to the enclosure

The enclosure itself remains completely independent of the electronics.

## ⚙️ Configuration

The PCB is instantiated using a **Derived Part**.

Changing the PCB configuration automatically rebuilds:

- Adapter Plate
- Box Base
- Box Lid

Additional configuration options include:

- Component Side
- Mount Side
- Rotation

## Outputs

- PCB support
- PCB mounting points
- Mechanical interface to the Box Base
- Structural reinforcement

---

# 📄 Threaded Insert Library

## 🧠 Purpose

Provides configurable heat-set threaded inserts as reusable engineering components.

The library contains both the reference model and the manufacturing geometry required by the enclosure.

## Outputs

- Threaded Insert
- Insert Pocket
- Wall Thickness Guide
- Mate Connector

The Insert Pocket is used directly as a Boolean tool inside the Box Base.

Only one insert instance needs to be positioned; additional inserts can be mirrored or patterned.

Supported insert types:

- M3×L5.7-OD4.6
- M4×L5.0-OD6.0

---

# 📄 Box Base

## 🧠 Purpose

Provides the reusable mechanical enclosure.

The Box Base contains:

- Outer shell
- Adapter Plate mounting lugs
- Heat-set insert pockets
- Reinforcing features
- Mounting interface to the Adapter Plate

The Box Base does **not** know which PCB is installed.

---

# 📄 Box Lid

## 🧠 Purpose

Closes the enclosure.

The Box Lid attaches to the threaded inserts located in the Box Base.

Future versions may include:

- Display windows
- Connector openings
- Labels
- Ventilation
- Snap fits

---

# 🔗 Derived Parts

Boxify builds the enclosure using **Derived Parts**.

```text
PCB Library
      │
      ▼
Adapter Plate
      │
      ▼
Box Base
      ▲
      │
Threaded Insert Library
      │
      ▼
Box Lid
```

Derived geometry is used for:

- visualization
- interference checking
- measured variables
- Boolean tooling
- future KiCad integration

---

# 📏 Variable Ownership

Every parameter has a single owner.

| Owner | Examples |
|--------|----------|
| Variable Studio | Printing standards, clearances and design constants |
| PCB Library | PCB geometry and mounting-hole locations |
| Adapter Plate | PCB placement and support geometry |
| Threaded Insert Library | Insert dimensions and insert pocket geometry |
| Box Base | Box construction dimensions |
| Box Lid | Lid construction dimensions |

This prevents duplicated information and keeps responsibilities clearly separated.

---

# 📐 Design Rules

1. Every building block has exactly one responsibility.
2. Every dimension has a single owner.
3. Never type the same dimension twice.
4. Derive geometry instead of recreating it.
5. Convert derived geometry into **Measured Variables** before using it to drive sketches.
6. Reuse engineering knowledge through configurable libraries.
7. Variable Studios contain engineering standards, not duplicated geometry.
8. The Adapter Plate is the only part that knows about the electronics.
9. The enclosure remains reusable for future electronics projects.
10. Prefer reusable Boolean tooling over duplicated sketches.
11. A valid configuration should gracefully support boundary conditions.

---

# 🚀 Future Development

Planned extensions include:

- KiCad STEP model library
- Automatic connector cut-outs
- Display windows
- Push button and LED openings
- Ventilation patterns
- Cable glands
- Snap-fit systems
- DIN rail mounting
- Wall mounting options
- Multiple enclosure families

The long-term goal remains unchanged:

> **Design a new enclosure primarily through configuration, while the engineering knowledge is captured once and reused through configurable libraries.**
