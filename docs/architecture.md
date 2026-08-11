# 🏗 Architecture

## 🧠 Overview

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** built for Onshape.

Unlike traditional enclosure designs, Boxify separates the **electronics** from the **mechanical enclosure** by introducing an intermediate **Adapter Plate**.

This modular architecture allows the same PCB to be reused with different enclosure styles while keeping every stage fully parametric.

<img width="582" height="630" alt="image" src="https://github.com/user-attachments/assets/79765599-6296-46cc-861c-e51c30c22aa4" />
<p align="center"><i>The boxify Enclosure design</i></p>

<img width="912" height="796" alt="image" src="https://github.com/user-attachments/assets/2a43b5b7-7397-4ead-834f-749adac874b5" />
<p align="center"><i>The 3D printed result</i></p>


---

# 🧩 System Architecture

```text
                 PCB Design
                     │
                     ▼
              Imported STEP Model
                     │
                     ▼
               PCB Library Entry
                     │
                     ▼
               Adapter Plate
          (Mechanical Interface)
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
      Box Base             Box Lid
          │                     │
          └──────────┬──────────┘
                     ▼
                  Assembly
```

Each stage has a single responsibility and can evolve independently.

---

# 📦 Core Components

## PCB Library

The PCB Library defines the electronics.

Each PCB is represented by:

- PCB geometry
- Mounting hole locations
- PCB dimensions
- Reference geometry

The PCB Library never contains enclosure-specific information.

Its only purpose is to describe the PCB itself.

---

## Adapter Plate

The Adapter Plate is the heart of Boxify.

It provides the mechanical interface between the PCB and the enclosure.

Responsibilities include:

- Component orientation
- Mounting orientation
- Defining mounting locations
- Providing reference geometry
- Measuring required dimensions
- Passing information to the enclosure

Because the enclosure depends only on the Adapter Plate, the PCB and enclosure remain loosely coupled.

---

## Enclosure

The enclosure consists of two parts:

- Box Base
- Box Lid

These are generated from the Adapter Plate rather than directly from the PCB.

Typical enclosure features include:

- Walls
- Bottom
- Lid
- Lip
- Mounting lugs
- Screw pockets
- Threaded insert pockets

---

## Threaded Insert Library

Threaded inserts are managed independently from the enclosure.

Each insert definition contains:

- Insert dimensions
- Pocket geometry
- Clearance
- Counterbore dimensions

The enclosure simply references the selected insert type.

---

## Assembly

The Assembly combines all generated components.

Its purpose is verification rather than design.

Typical checks include:

- PCB fit
- Component clearance
- Screw alignment
- Insert alignment
- Overall dimensions

---

# 🔄 Data Flow

Information flows in one direction.

```text
PCB
 ↓
PCB Library
 ↓
Adapter Plate
 ↓
Enclosure
 ↓
Assembly
```

Each stage builds upon the previous stage.

Changes made upstream automatically propagate downstream.

---

# 📏 Parametric Design

Every important dimension in Boxify is controlled by variables.

Examples include:

- Wall thickness
- Box height
- Adapter Plate height
- PCB offset
- Corner radius
- Insert type

This allows an enclosure to adapt automatically when the PCB changes.

---

# 📐 Measured Variables

Whenever possible, Boxify measures geometry instead of relying on manually entered values.

Examples include:

- PCB size
- Adapter Plate thickness
- Lug height
- Internal enclosure dimensions

Measured variables reduce duplicate information and improve robustness.

---

# 🔗 Reference Geometry

Rather than copying geometry between Part Studios, Boxify uses derived geometry and reference features.

This provides:

- Consistent positioning
- Reduced maintenance
- Better model stability

Reference geometry includes:

- Mate Connectors
- Derived Parts
- Composite Parts
- Construction Geometry

---

# ⚙ Configurations

Configurations allow a single model to support multiple hardware variants.

Typical configurable options include:

- PCB selection
- PCB rotation
- Mount side
- Component side
- Insert type
- Enclosure dimensions

This avoids maintaining multiple versions of the same design.

---

# 🎯 Design Principles

Boxify follows several guiding principles.

## Separation of Responsibilities

Each Part Studio has one clearly defined purpose.

---

## Reusable Components

Components should be reusable across multiple projects whenever possible.

---

## Parametric First

Geometry should adapt through parameters rather than manual editing.

---

## Measure Rather Than Duplicate

Whenever geometry already contains the required information, measure it instead of entering it again.

---

## Stable References

Use reference geometry and derived parts instead of recreating geometry.

---

## Modular Growth

New functionality should extend the framework without requiring existing designs to change.

---

# 📚 Related Documentation

For implementation details, see:

- Getting Started
- PCB Library
- Adapter Plate
- Enclosure
- Threaded Insert Library
- KiCad Integration
- Configurations
