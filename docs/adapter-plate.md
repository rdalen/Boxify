# 📄 Adapter Plate

## 🧠 Purpose

The Adapter Plate is the mechanical heart of the Boxify framework.

It forms the interface between the PCB and the enclosure, allowing both to evolve independently while remaining fully parametric.

By separating the electronics from the enclosure, the Adapter Plate makes it possible to reuse the same PCB with different enclosure designs or reuse the same enclosure architecture with different PCBs.

---

<img width="1046" height="892" alt="image" src="https://github.com/user-attachments/assets/f35adabe-419d-4af0-94da-1c42cc0a5404" />
<p align="center"><i>Adapter Plate with the PCB Library Entry</i></p>

---

# 🏗 Responsibilities

The Adapter Plate is responsible for:

- Mounting the PCB
- Positioning the PCB inside the enclosure
- Defining the mechanical reference for the enclosure
- Providing mounting locations for enclosure features
- Passing configuration variables to downstream Part Studios

Because the enclosure depends only on the Adapter Plate, the PCB and enclosure remain loosely coupled.

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
Create Composite Part
Create a PCB Library Entry
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

# ⚙️ Configuration options

Typical configuration options include:

- PCB selection
- Component side
- Mounting side
- PCB offset
- Mounting clearances

These parameters allow the Adapter Plate to adapt automatically to different PCB designs.

These configuration options can be selected in the downstream Part Studio PS Enclosure

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

---

### Mechanical Separation

The PCB and enclosure remain independent.

Changes to one should have minimal impact on the other.

---

### Single Source of Truth

The Adapter Plate is the authoritative source for the mechanical interface between the electronics and the enclosure.

---

### Parametric by Design

All important dimensions are controlled by variables or measured geometry.

Manual editing should rarely be required.

---

### Stable References

Reference geometry is preferred over recreated geometry wherever possible.

This improves model stability and simplifies future changes.

---

# 📚 Related Documentation

- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)
