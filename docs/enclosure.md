# 📦 Enclosure

## 🧠 Purpose

The **Enclosure** is the final mechanical stage of the Boxify framework.

It converts the mechanical interface provided by the **Adapter Plate** and the user-selected configuration in **PS Enclosure** into a complete parametric enclosure consisting of:

- **Box Base**
- **Box Lid**

The Base and Lid are designed together because they always belong to the same enclosure configuration.

The enclosure adapts automatically to the PCB Type provided by the upstream PCB definition, the Adapter Plate and the enclosure configuration.

---

<img width="75%" alt="image" src="https://github.com/user-attachments/assets/452abd34-6212-4b03-86d6-fdb95021c9f2" />
<p align="center"><i>Enclosure for the PCB with cut-outs and textlabels</i></p>

---

# 🏗 Enclosure Architecture

The enclosure is generated from the information provided by the preceding stages of the Boxify architecture.

```text
PS KiCadStep  <--- PCB Library
     │
     │ PCB geometry
     ▼
Adapter Plate
     │
     │ mechanical interface
     ▼
PS Enclosure  <--- Threaded Insert Library
     │
     │ user configuration
     ▼
┌─────────────────────────┐
│                         │
│     Box Base + Lid      │
│                         │
└─────────────────────────┘
```

The enclosure does not need to understand the detailed implementation of the PCB.

It uses the **Adapter Plate as the mechanical abstraction layer**.

This keeps the enclosure independent from the PCB implementation while allowing it to adapt to different PCB designs.

---

# ⚙️ PS Enclosure

**PS Enclosure** is the main user-facing Part Studio for configuring and generating the enclosure.

Enclosure-related configuration is centralized here.

Typical configuration options include:

- PCB rotation
- Component side up or down
- Mounting side above or below the adapter plate
- Adapter Plate height
- Box height
- Wall thickness
- Lid fastening type
- Threaded insert configuration
- Interface cut-outs
- Text labels

The available configuration options can change dynamically depending on other selections.

This prevents irrelevant settings from being presented to the user.

---

# 🧱 Box Base

The **Box Base** contains the main body of the enclosure.

It is generated relative to the Adapter Plate and the selected enclosure configuration.

Typical Base features include:

- Bottom
- Side walls
- Mounting structure
- PCB / Adapter Plate support
- Enclosure lugs
- Threaded insert pockets
- Interface cut-outs
- Text labels

The Base dimensions are derived from the configured PCB and Adapter Plate geometry wherever possible.

This allows the Base to adapt automatically when the upstream geometry changes.

---

# 🛡️ Box Lid

The **Box Lid** closes the enclosure and is generated as part of the same enclosure configuration as the Base.

The Lid can contain:

- Lid plate
- Fastening features
- Screw pockets
- Countersinks or counterbores
- Cut-outs
- Text labels

The Lid is designed together with the Base so that fastening features, wall geometry and clearances remain coordinated.

---

# 🔩 Lid Fastening

The Lid fastening system is configuration-driven.

The user selects a **Lid Fastening Type**, and Boxify generates the corresponding geometry.

Possible configurations can include:

- **None**
- **Counterbore screws**
- **Countersunk screws**
- **Selftapping screws**

The exact options can evolve as additional fastening methods are added.

The important principle is that the user selects the **fastening method**, not the individual CAD features required to implement it.

---

## Dynamic Fastening Features

Fastening-related features are dynamically suppressed when they are not required.

For example:

```text
Lid Fastening Type
       │
       ├── None
       │       └── Screw-related features suppressed
       │
       ├── Counterbore screws
       │       └── Counterbore geometry enabled
       │
       └── Countersunk screws
               └── Countersink geometry enabled
```

This allows several lid variants to be generated from the same parametric feature structure.

---

# 🔩 Threaded Inserts

Threaded inserts are optional.

The **Threaded Insert Library** provides reusable insert definitions that can be selected from the enclosure configuration.

The selected insert definition determines the geometry required to accommodate the insert.

```text
Threaded Insert Library
          │
          │ reusable insert definition
          ▼
     PS Enclosure
          │
          ▼
 Insert pockets / mounting geometry
```

Threaded inserts can also be disabled completely.

When inserts are disabled, insert-related features are dynamically suppressed.

This makes it possible to use the same enclosure model for:

- Enclosures with threaded inserts
- Enclosures without threaded inserts

---

<img width="1156" height="494" alt="image" src="https://github.com/user-attachments/assets/cdcd8059-6a76-43c5-95dc-5889efbc2660" />
<p align="center"><i>Section view of Enclosure with Threaded Inserts</i></p>

---

# 🔌 Interface Cut-outs

Boxify allows the user to define interface cut-outs based on a **projection of the relevant PCB / KiCad geometry**.

The required interface geometry is projected into Boxify, where the user defines the corresponding cut-out. The cut-out is then positioned relative to the **Adapter Plate and enclosure**, rather than being manually placed against the final enclosure walls.

This makes the cut-outs more robust when enclosure dimensions or configurations change.

Because the cut-out is derived from the PCB geometry, it **propagates with the PCB** when the PCB position or configuration changes.

Where the required geometry is available from the PCB / KiCad integration, the cut-out can therefore remain associated with the **interface it represents**, rather than with a fixed location in the enclosure.

> **PCB geometry → projection → user-defined cut-out → parametric enclosure**
---

# 🏷️ Text Labels

Boxify allows the user to **define optional text labels** as part of the enclosure configuration.

Labels can be used for:

- Connector identification
- Button identification
- Orientation markings
- Project information
- Other enclosure-specific information

Labels belong to the **enclosure configuration** rather than the PCB Library because they describe the physical enclosure, not the PCB itself.

This keeps enclosure-specific labelling independent from the PCB definition and allows labels to be configured together with the enclosure.

---

# 📐 Parametric Dimensions

The enclosure is driven by a combination of configuration variables and derived/measured geometry.

Typical configurable dimensions include:

- Box height
- Wall thickness
- Bottom thickness
- Lid thickness
- Adapter Plate height

Other dimensions should preferably be derived from existing geometry.

For example:

```text
PCB geometry
     │
     ▼
Adapter Plate geometry
     │
     ├── measured PCB Thickness
     ├── mounting positions
     └── reference geometry
             │
             ▼
       Enclosure geometry
```

This avoids duplicating dimensions throughout the model.

---

# 🔄 Base and Lid Relationship

The Box Base and Box Lid are intentionally kept together in the same Part Studio.

This provides a shared design context for features that must remain coordinated.

Examples include:

- Overall enclosure dimensions
- Wall thickness
- Lug position
- Fastening locations
- Screw clearance
- Insert positions
- Lid fit
- Cut-out locations

A change to the enclosure configuration can therefore update both Base and Lid together.

---

# 🧩 Feature Organization

Related enclosure features should remain grouped logically.

A typical feature structure can contain groups such as:

```text
Enclosure
│
├── References
│
├── Box Base
│   ├── Bottom
│   ├── Walls
│   ├── Lugs
│   └── Insert Features
│
├── Box Lid
│   ├── Lid
│   ├── Fastening
│   └── Optional Features
│
├── Cut-outs
│
└── Labels
```

The exact feature structure can evolve as the enclosure develops.

The important principle is that related features remain together and their order remains understandable.

---

# ⚙️ Dynamic Feature Suppression

Boxify uses **dynamic suppression** to keep optional features under control.

A configuration can determine whether a feature or feature group is relevant.

For example:

```text
Configuration
     │
     ├── Inserts = None
     │       └── Insert features suppressed
     │
     ├── Lid fastening = Countersunk Screws
     │       ├── Counterbore suppressed
     │       └── Countersink enabled
     │
     └── Labels = No
             └── Label features suppressed
```

This means one enclosure model can support multiple valid configurations without requiring separate versions of the feature tree.

---

# 🧠 Design Principles

The enclosure follows several important principles.

### Configuration-driven

The enclosure is generated from user-selected configuration rather than manually edited geometry.

### Adapter Plate driven

The enclosure uses the Adapter Plate as its mechanical reference.

### Base and Lid together

The Base and Lid share the same design context and configuration.

### Optional features

Features that are not required are dynamically suppressed.

### Reusable libraries

Reusable components such as threaded inserts are provided through libraries rather than hard-coded into the enclosure.

### Derived geometry

Where possible, geometry is derived or measured rather than duplicated as independent variables.

### Minimal manual intervention

Normal enclosure configuration should not require editing Derive features, sketches or internal implementation details.

---

# 🎯 Result

The result is a configurable enclosure that can adapt to different electronics without rebuilding the enclosure architecture from scratch.

The same Boxify framework can therefore produce variants such as:

```text
                    Boxify Enclosure
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
     No Inserts       With Inserts      Different PCB
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                  Configured Base + Lid
```

The geometry changes according to the selected configuration while the underlying enclosure architecture remains reusable.

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)

---
