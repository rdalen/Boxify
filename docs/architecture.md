# 🏗 Architecture

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** built for Onshape.

The architecture separates the electronics from the mechanical enclosure by introducing an intermediate **Adapter Plate**.

This separation allows:

- The same PCB to be used with different enclosure configurations.
- The same enclosure architecture to support different PCB designs.
- PCB-specific geometry to remain independent from enclosure-specific geometry.
- Enclosure configuration to be managed from a single user-facing location.

The result is a modular, reusable and configuration-driven enclosure framework.

---

# 🔄 Workflow Overview

```text
PCB / KiCad
     │
     │ detailed PCB STEP
     ▼
PS KiCadStep  <--- PCB Library
     │
     │ PCB + reusable PCB type
     ▼
Adapter Plate
     │
     │ mechanical interface
     ▼
PS Enclosure <--- Threaded Insert Library
     │
     │ user configuration
     ▼
Box Base + Box Lid
     │
     ▼
Assembly
```

The libraries support the stages where their reusable definitions are consumed:

- **PCB Library** provides the reusable PCB type used by `PS KiCadStep`.
- **Threaded Insert Library** provides the insert definitions used by `PS Enclosure`.

The main mechanical design flow remains one-directional:

> **PCB → Adapter Plate → Enclosure → Assembly**

Changes to upstream geometry or configuration propagate downstream through the parametric architecture.

---

<img width="59%" alt="image" src="https://github.com/user-attachments/assets/e1c10873-f9fd-4df8-9d88-fa98777c8af0" />  

<img width="39%" alt="image" src="https://github.com/user-attachments/assets/79765599-6296-46cc-861c-e51c30c22aa4" />
<p align="center"><i>From PCB design to Boxify Enclosure design</i></p>
---

# 🧩 Core Components

## PCB Library

The **PCB Library** contains reusable definitions of supported PCB types.

In the Boxify architecture, the PCB Library provides the **Carrier** for the imported KiCad STEP model, which acts as the **Passenger**.

Each PCB definition can provide:

- PCB geometry
- Mounting hole locations
- PCB dimensions
- Reference geometry
- PCB-specific measured variables

The PCB Library contains information about the PCB itself and does not contain enclosure-specific configuration.

This separation allows the same PCB definition to be reused by different enclosure designs.

---

<img width="29%" alt="image" src="https://github.com/user-attachments/assets/f4776759-e6ac-4e63-8ff4-193a4fcfcd65" />
<img width="69%" alt="image" src="https://github.com/user-attachments/assets/16429a6e-03f9-459d-8346-272a7b01088f" />
<p align="center"><i>Boxify PCB Library</i></p>

---

## PS KiCadStep

`PS KiCadStep` integrates a detailed PCB STEP model exported from KiCad with a reusable PCB type from the PCB Library.

The architecture uses a **Carrier / Passenger** relationship:

```text
PCB Library
    │
    │ reusable PCB type
    ▼
  Carrier
    ▲
    │
  Passenger
    │
    │ detailed KiCad STEP
    │
PS KiCadStep
```
*In Onshape terms; The KiCad STEP is mated to the PCB Type from the PCB library*

The PCB Library provides the stable, reusable reference geometry, while the imported STEP model provides the detailed representation of the actual PCB assembly.

This allows the detailed KiCad model to be used without making the enclosure dependent directly on the imported STEP geometry.

---

# 🔧 Adapter Plate

The **Adapter Plate** is the mechanical heart of Boxify.

It provides the interface between the PCB and the enclosure.

Its responsibilities include:

- Mounting the PCB.
- Positioning the PCB relative to the enclosure.
- Providing stable mechanical reference geometry.
- Defining mounting locations for enclosure features.
- Providing measured and derived geometry required by the enclosure.

The Adapter Plate deliberately separates the electronics from the enclosure.

The enclosure therefore does not need to understand the details of the PCB itself. It only needs the mechanical interface provided by the Adapter Plate.

This loose coupling is one of the fundamental principles of Boxify.

---

<p align="center">
  <img width="75%" alt="image" src="https://github.com/user-attachments/assets/e3d67d1b-cafe-44a0-b046-9d6984c00c5e" />
<p>
<p align="center"><i>Boxify Adapter Plate</i></p>

---

# 📦 PS Enclosure

`PS Enclosure` is the **main user-facing configuration point** of Boxify.

Enclosure-related configuration is centralized here so that the user normally does not need to switch between Part Studios to configure the enclosure.

Typical configuration options include:

- PCB type selection
- PCB rotation
- Component side up or down
- Mounting side above or below the adapter plate
- Adapter Plate height
- Box height
- Box-wall, box-bottom and box-lid  thickness
- The PCB distance from the box-walls
- Lid fastening type
- Threaded insert configuration
- Interface cut-outs
- Text labels

Configuration variables and features can be shown, hidden or dynamically suppressed depending on the selected options.

This allows a single enclosure framework to generate different enclosure variants without manually editing downstream features.

---

<p align="center">
  <img width="50%" alt="image" src="https://github.com/user-attachments/assets/79765599-6296-46cc-861c-e51c30c22aa4" />
</p>
<p align="center"><i>Boxify Enclosure</i></p>

---

### 🧱 Box Base and Box Lid

The enclosure consists of two main components:

- **Box Base**
- **Box Lid**

Both are generated from the enclosure configuration and the mechanical references provided by the Adapter Plate.

Typical enclosure features include:

- Walls
- Bottom
- Lid
- Mounting lugs
- Screw pockets
- Threaded insert pockets
- Interface cut-outs
- Text labels

The Box Base and Box Lid are designed as a coordinated system because they always belong to the same enclosure configuration.

The selected configuration determines which features are required. Features that are not applicable can be dynamically suppressed.

---

# 🔩 Threaded Insert Library

The **Threaded Insert Library** contains reusable definitions for threaded inserts.

Each insert definition can contain:

- Insert dimensions
- Pocket geometry
- Installation requirements
- Configuration information

The library is consumed by **PS Enclosure**.

This keeps insert-specific geometry separate from the enclosure itself and allows different insert types to be supported without redesigning the enclosure features.

Threaded inserts can also be disabled when an enclosure does not require them.

---

<img width="39%" alt="image" src="https://github.com/user-attachments/assets/7439bafd-b270-442a-9c83-bebcb6d6b8c8" />
<img width="59%" alt="image" src="https://github.com/user-attachments/assets/8a0189ef-4f5e-4c37-b752-3c603636a3fd" />
<p align="center"><i>Boxify Treaded inserts Library</i></p>

---

# ⚙️ Configuration Architecture

A key principle introduced in Boxify is **centralized user configuration**.

The user-facing configuration belongs to **PS Enclosure**.

The other components provide the information required to generate the enclosure:

```text
                     User Configuration
                            │
                            ▼
                    ┌───────────────┐
                    │ PS Enclosure  │
                    └───────┬───────┘
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
        PCB geometry   Adapter Plate   Insert Library
             │              │              │
             └──────────────┼──────────────┘
                            ▼
                    Box Base + Box Lid
```

This distinction is important:

### User configuration

Controls **what the user wants**, for example:

- Box height
- Adapter height
- Wall thickness
- The PCB distance from the box-walls
- Lid fastening
- Insert option
- PCB orientation

### Engineering and measured variables

Describe **what the geometry provides**, for example:

- PCB dimensions
- Mounting-hole positions
- Derived reference locations
- Measured component clearance

Whenever possible, engineering information is measured or derived from existing geometry rather than entered manually.

---

# 🔁 Parametric Propagation

Boxify is designed so that information flows through the architecture rather than being duplicated.

```text
PCB / KiCad
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
Box Base + Box Lid
```

For example:

1. A PCB STEP model provides the actual PCB assembly.
2. The PCB Library provides the reusable PCB geometry and reference.
3. The Adapter Plate establishes the mechanical interface.
4. PS Enclosure uses that interface together with the selected configuration.
5. Box Base and Box Lid are generated from those references.

When upstream geometry changes, downstream geometry can adapt automatically.

This reduces duplicated dimensions and minimizes manual updates.

---

# 🎯 Design Principles

Boxify follows several guiding principles.

## Separation of Responsibilities

Each Part Studio and library has a clearly defined purpose.

PCB-related information belongs to the PCB architecture.

Mechanical interface information belongs to the Adapter Plate.

Enclosure configuration and generation belong to PS Enclosure.

Reusable insert definitions belong to the Threaded Insert Library.

---

## Centralized User Configuration

User-facing enclosure configuration should be available from **PS Enclosure**.

The user should not need to edit downstream Part Studios or manually modify Derive features during normal configuration.

---

## Reusable Components

Components should be reusable across multiple projects whenever possible.

Libraries are used to separate reusable definitions from project-specific geometry.

---

## Parametric First

Geometry should adapt through parameters and configuration rather than manual editing.

---

## Measure Rather Than Duplicate

Whenever geometry already contains the required information, measure or derive it instead of entering the same information again.

This reduces inconsistencies and makes the model more robust to changes.

---

## Stable References

Use stable reference geometry and derived parts instead of recreating geometry.

This keeps relationships between the PCB, Adapter Plate and enclosure predictable.

---

## Loose Coupling

The enclosure should depend on the **mechanical interface** rather than directly on PCB implementation details.

The Adapter Plate provides this abstraction layer.

---

## Dynamic Configuration

Features that are not required by a selected configuration should be automatically suppressed where practical.

This keeps the generated model clean while allowing one framework to support many enclosure variants.

---

## Modular Growth

New functionality should extend the framework without requiring existing designs to be fundamentally redesigned.

Libraries and configuration-driven features should be preferred over project-specific hard-coded geometry.

---

# 🧠 The Boxify Architecture in One Sentence

> **Boxify separates the PCB from the enclosure through an Adapter Plate, while PS Enclosure provides centralized configuration to generate a complete parametric enclosure from reusable libraries and derived mechanical references.**

---

# 📚 Related Documentation

For implementation details, see:

- 📄 [Getting Started](getting-started.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)

---
