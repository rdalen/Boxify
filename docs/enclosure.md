# 📄 Enclosure

## 🧠 Purpose

The Enclosure provides the protective housing for the electronics.

Within Boxify, the enclosure is generated from the Adapter Plate rather than directly from the PCB. This separation allows the enclosure to adapt automatically to different PCB designs while keeping the mechanical architecture consistent.

The enclosure consists of two closely related parts:

- **Box Base**
- **Box Lid**

Both are designed within the same Part Studio to ensure they always remain compatible.

<img width="850" height="896" alt="image" src="https://github.com/user-attachments/assets/1331d56a-d396-409b-99f9-39e9f524ce26" />
<p align="center"><i>Enclosure for the PCB with cut-outs and textlabels</i></p>

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
Configure Adapter Plate
      │
      ▼
-> Configure & Generate Enclosure
      │
      ▼
Assembly
```

All enclosure geometry is derived from the Adapter Plate rather than directly from the PCB.

---

# 🏗 Architecture

```text
PCB Library
      │
      ▼
Adapter Plate
      │
      ▼
┌─────────────────────┐
│     Enclosure       │
│                     │
│   ┌─────────────┐   │
│   │  Box Base   │   │
│   ├─────────────┤   │
│   │   Box Lid   │   │
│   ├─────────────┤   │
│   │   Cut-outs  │   │
│   │ Text labels │   │
│   └─────────────┘   │
└─────────────────────┘
      │
      ▼
Assembly
```

The enclosure relies on the Adapter Plate for positioning, dimensions and reference geometry.

---

# 📦 Responsibilities

The enclosure is responsible for:

- Protecting the electronics
- Providing structural rigidity
- Defining the external dimensions
- Housing the Adapter Plate
- Supporting threaded inserts and fasteners
- Providing a removable lid for assembly and maintenance

The enclosure does **not** define PCB geometry or mounting locations. Those responsibilities belong to the Adapter Plate.

---

# 🧩 Box Base

The Box Base forms the structural foundation of the enclosure.

Typical features include:

- Bottom surface
- Side walls
- Internal mounting lugs
- Adapter Plate supports
- Threaded insert pockets
- Cable openings (optional)
- Ventilation features (optional)
- Cut-outs and text labels

---

# 🧩 Box Lid

The Box Lid closes the enclosure and protects the electronics.

Typical features include:

- Lid surface
- Alignment lip
- Screw clearance holes
- Counterbores and countersinks (where applicable)
- Optional openings for displays, buttons or connectors

---

# ⚙️ Configuration

Typical configurable parameters include:

- all the upstream Part Studio Configuration Variables
- Box height
- Wall thickness
- Bottom thickness
- Lid thickness
- Corner radius
- Lip height
- Lip clearance
- Screw type
- Threaded insert type

All dimensions are controlled parametrically.

---

# 📐 Derived Geometry

The enclosure derives its geometry from the Adapter Plate.

Examples include:

- Overall footprint
- Mounting lug positions
- Internal clearances
- Mounting references
- Adapter Plate height

This approach minimizes duplicated information and allows changes to propagate automatically.

---

# 📏 Measured Variables

Where possible, Boxify measures geometry rather than relying on manually entered dimensions.

Examples include:

- Adapter Plate height
- Lug height
- Internal enclosure height
- Reference offsets

Measured variables improve consistency and reduce maintenance.

---

# 🔩 Threaded Inserts

The enclosure supports interchangeable threaded insert definitions from the Threaded Insert Library.

Rather than modelling insert geometry directly, the enclosure references the selected insert type to generate the appropriate mounting pockets.

This allows insert types to be changed without redesigning the enclosure.

<img width="1156" height="494" alt="image" src="https://github.com/user-attachments/assets/cdcd8059-6a76-43c5-95dc-5889efbc2660" />
<p align="center"><i>Section view of Enclosure with Treaded Inserts</i></p>

---

# 🎯 Design Principles

The enclosure follows several key principles.

## Shared Design Context

The Box Base and Box Lid are created in the same Part Studio.

This ensures both parts always share the same reference geometry and remain fully compatible.

---

## Parametric First

Every important dimension should be configurable through variables.

---

## Reuse Over Recreation

Geometry is derived from upstream components whenever possible instead of being recreated.

---

## Modular Design

The enclosure focuses solely on protecting and supporting the electronics.

Electronic design remains independent within the PCB Library and Adapter Plate.

---

## Maintainability

Changes to the PCB or Adapter Plate should automatically propagate through the enclosure with minimal manual intervention.

---

# 🚀 Future Enhancements

Possible future enclosure features include:

- Snap-fit lids
- Living hinges
- Waterproof sealing grooves
- Gasket support
- Ventilation patterns
- Cable glands
- DIN rail mounting
- Wall mounting options
- Modular front and rear panels

---

# 📚 Related Documentation

- Architecture
- PCB Library
- Adapter Plate
- Threaded Insert Library
- KiCad Integration
