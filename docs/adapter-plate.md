# 📄 Adapter Plate

## 🧠 Purpose

The Adapter Plate is the mechanical heart of the Boxify framework.

It forms the interface between the PCB and the enclosure, allowing both to evolve independently while remaining fully parametric.

By separating the electronics from the enclosure, the Adapter Plate makes it possible to reuse the same PCB with different enclosure designs or reuse the same enclosure architecture with different PCBs.

---

# 🏗 Responsibilities

The Adapter Plate is responsible for:

- Mounting the PCB
- Positioning the PCB inside the enclosure
- Defining the mechanical reference for the enclosure
- Providing mounting locations for enclosure features
- Passing measured dimensions to downstream Part Studios
- Passing configuration variables to downstream Part Studios

The Adapter Plate intentionally contains no enclosure walls or lid geometry.

---

# 🔄 Position in the Workflow

```text
 KiCad PCB
      │
      ▼
 Export STEP Model
      │
      ▼
 Import into Onshape
      │
      ▼
 Create Composite Part
      │
      ▼
 Derive into PCB Library
      │
      ▼
-> Configure Adapter Plate
      │
      ▼
Configure & Generate Enclosure
      │
      ▼
Assembly
```

All enclosure geometry is derived from the Adapter Plate rather than directly from the PCB.

---

# ⚙️ Configuration

Typical configuration options include:

- PCB selection
- Component side
- Mounting side
- Adapter Plate height
- PCB offset
- Mounting clearances

These parameters allow the Adapter Plate to adapt automatically to different PCB designs.

---

# 📐 Reference Geometry

The Adapter Plate provides the reference geometry used throughout the Boxify framework.

Examples include:

- Reference planes
- Reference Mate Connector
- Mounting locations
- Measured dimensions

Downstream Part Studios should reference these features instead of creating their own.

---

# 📏 Measured Variables

Where possible, Boxify measures geometry instead of requiring manual input.

Examples include:

- Adapter Plate thickness
- Mounting height
- PCB position
- Internal reference dimensions

Measured variables improve robustness and reduce duplicated information.

---

# 📤 Outputs

The Adapter Plate provides:

- Adapter Plate solid model
- PCB mounting locations
- Reference geometry
- Measured variables
- Mounting references for the enclosure

These outputs are consumed by the Box Base, Box Lid and Assembly.

---

# 🎯 Design Principles

The Adapter Plate follows several core principles:

## Mechanical Separation

The PCB and enclosure remain independent.

Changes to one should have minimal impact on the other.

---

## Single Source of Truth

The Adapter Plate is the authoritative source for the mechanical interface between the electronics and the enclosure.

---

## Parametric by Design

All important dimensions are controlled by variables or measured geometry.

Manual editing should rarely be required.

---

## Stable References

Reference geometry is preferred over recreated geometry wherever possible.

This improves model stability and simplifies future changes.

---

# 📚 Related Documentation

- PCB Library
- Enclosure
- Threaded Insert Library
- KiCad Integration
- Architecture
