# 📦 Enclosure

## 🧠 Purpose

The **Enclosure** is the final mechanical stage of the Boxify framework.

It converts the mechanical interface provided by the **Adapter Plate** and the user-selected configuration in **PS Enclosure** into a complete parametric enclosure consisting of:

- **Box Base**
- **Box Lid**

The Base and Lid are designed together because they always belong to the same enclosure configuration.

The enclosure adapts automatically to the selected PCB, Adapter Plate and configuration.

---


<img width="850" height="896" alt="image" src="https://github.com/user-attachments/assets/1331d56a-d396-409b-99f9-39e9f524ce26" />
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

- PCB selection
- PCB rotation
- Component side
- Mounting side
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
- Optional cut-outs
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
       ├── No fastening
       │       └── Screw-related features suppressed
       │
       ├── Counterbore
       │       └── Counterbore geometry enabled
       │
       └── Countersink
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
<p align="center"><i>Section view of Enclosure with Treaded Inserts</i></p>

---

# 🔌 Interface Cut-outs

Boxify can generate interface cut-outs based on the geometry and configuration available to the enclosure.

Cut-outs are positioned relative to the enclosure and Adapter Plate rather than being manually placed against the final enclosure walls.

This makes them more robust when enclosure dimensions change.

Where the required geometry is available from the PCB / KiCad integration, the cut-out can follow the corresponding interface geometry.

The goal is to keep the cut-out associated with the **interface it represents**, rather than with a fixed location in the enclosure.

---

# 🏷️ Text Labels

Optional text labels can be included in the enclosure configuration.

Labels can be used for:

- Connector identification
- Button identification
- Orientation markings
- Project information
- Other enclosure-specific information

Labels belong to the enclosure configuration rather than the PCB Library because they describe the physical enclosure.

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
     ├── measured height
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
     ├── Inserts = No
     │       └── Insert features suppressed
     │
     ├── Lid fastening = Countersink
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
- 📄 [Configurations](configurations.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)

---

**Happy Boxifying! 🚀**
