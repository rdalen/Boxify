# 📚 PCB Library

## 🧠 Purpose

The **PCB Library** contains reusable definitions of supported PCB Types for use within the Boxify framework.

A PCB Library entry provides the stable mechanical reference that Boxify uses to connect a (KiCad) PCB design to the Adapter Plate and, ultimately, the enclosure.

The PCB Library is deliberately separated from the detailed PCB STEP model exported from KiCad.

The library provides the **Carrier**.

The imported KiCad STEP model provides the **Passenger**.

Together they form the PCB definition used by the Boxify architecture.

---

<p align="center">
<img width="1850" height="722" alt="image" src="https://github.com/user-attachments/assets/81441788-bdcd-4b3f-b8de-fb667ab9c511" />
</p>
<p align="center"><i>Boxify PCB Library</i></p>

---

# 🏗 Position in the Boxify Architecture

```text id="n2n3p5"
PCB / KiCad
     │
     │ detailed STEP
     ▼
PS KiCadStep  <--- PCB Library
     │
     │ PCB + reusable PCB Type
     ▼
Adapter Plate
     │
     │ mechanical interface
     ▼
PS Enclosure  <--- Threaded Insert Library
     │
     ▼
Box Base + Box Lid
```

The PCB Library is therefore part of the **PCB definition stage**, not the enclosure configuration stage.

---

# 🚗 Carrier / Passenger Architecture

The PCB Library prototype acts as the **Carrier** for the imported KiCad STEP model.

```text id="9y3c2v"
                PCB Library
                     │
                     │ reusable PCB Type
                     ▼
                  CARRIER
                     │
                     ▲
                     │
                  PASSENGER
                     │
                     │ detailed PCB assembly
                     │
                 KiCad STEP
```

### Carrier

The Carrier provides the stable, reusable PCB reference.

It can contain:

- PCB outline
- Mounting-hole geometry
- Reference geometry
- Mate Connector
- PCB dimensions
- PCB-specific measured or derived variables

### Passenger

The Passenger is the detailed PCB assembly imported from KiCad.

It can contain:

- PCB
- Components
- Connectors
- Mounting hardware
- Other mechanical details represented by the KiCad STEP export

The Passenger can be replaced when the PCB revision changes, while the Carrier remains the reusable reference whenever the PCB definition itself has not changed.

---

# 🧩 What a PCB Library Entry Represents

A PCB Library entry represents a **specific PCB design definition** that can be reused by Boxify.

It should provide enough information for the downstream architecture to establish:

- PCB identity
- PCB coordinate system
- PCB outline
- Mounting locations
- Reference geometry
- Connection to the Adapter Plate

The library entry should not contain enclosure-specific information.

For example, the PCB Library should **not** define:

- Box height
- Wall thickness
- Lid fastening
- Threaded insert selection
- Enclosure labels
- Enclosure cut-outs

Those decisions belong to **PS Enclosure**.

---

# 📐 PCB Geometry

A PCB Library entry should contain the geometry required to establish the mechanical relationship between the PCB and the Adapter Plate.

Important reference geometry can include:

### PCB Outline

Defines the physical board boundary.

### Mounting Holes

Defines the locations used to mount the PCB.

### Reference Geometry

Provides stable references for downstream features.

### Mate Connector

Provides the primary coordinate and orientation reference used when positioning the PCB within the Boxify architecture.

The goal is to keep these references stable and predictable.

---

# 📏 PCB Variables

PCB-specific variables can be stored or derived as part of the PCB definition.

Examples include:

```text id="8j7o5c"
pcbWidth
pcbLength
mountingHoleDiameter
mountingHolePosition
pcbThickness (defined in the Variable Studio under **General Settings**)
```

These values describe the PCB itself.

They should not be confused with enclosure configuration variables such as:

```text id="xv9c7a"
boxHeight
wallThickness
lidFasteningType
```

The general rule is:

> **PCB Library variables describe the PCB; PS Enclosure variables describe the enclosure.**

Please note: PCB thickness is defined in the Variable Studio under **General Settings**. It is not part of the PCB Library configuration table.

The PCB Library configuration table defines the PCB type and its mechanical interface. `pcbThickness` is a general PCB property used by Boxify when positioning the PCB and KiCad STEP geometry.

---

# 🧭 Coordinate System

The PCB Library provides a stable coordinate system for the PCB.

The coordinate system should remain consistent between:

- PCB Library geometry
- KiCad STEP exports
- PS KiCadStep
- Adapter Plate references

When a new STEP revision is imported, the PCB should retain the same coordinate relationship whenever possible.

This makes PCB updates much more predictable.

---

## Supported PCB Types

Boxify provides two categories of PCB Types in the PCB Library: **Prototype** and **Panel**.

### Prototype PCB Types

Prototype types are intended for the pre-drilled universal prototyping boards (so-called perfboard) you buy in bulk packs to build and test temporary or permanent DIY electronics

Examples include:

- Prototype w20×80mm
- Prototype w30×70mm
- Prototype w40×60mm
- Prototype w50×70mm
- Prototype w60×80mm
- Prototype w70×90mm

These are generic example formats and can be replaced or extended with project-specific PCB dimensions.

<p align="center">
<img width="30%" alt="image" src="https://github.com/user-attachments/assets/f4776759-e6ac-4e63-8ff4-193a4fcfcd65" />
</p>
<p align="center"><i>ProtoType PCBs</i></p>

### Panel PCB Types

Panel types are intended for PCBs that are manufactured as part of a **100 × 100 mm production panel**.

The supported dimensions take into account a **2 mm mouse-bite gap** between adjacent PCBs:

| Panel Type | Panel Layout | Calculation |
|---|---:|---:|
| Panel w100×100mm | 1 × 1 | 100 |
| Panel w49×100mm | 2 × 1 | 2 × 49 + 2 = 100 |
| Panel w49×49mm | 2 × 2 | 2 × 49 + 2 = 100 |
| Panel w23.5×49mm | 4 × 2 | 4 × 23.5 + 3 × 2 = 100 |
| Panel w23.5×23.5mm | 4 × 4 | 4 × 23.5 + 3 × 2 = 100 |

These panel dimensions make it possible to design the enclosure for the **individual PCB** while using the same PCB definition when the design is panelized for manufacturing.

> **Note:** The panel dimensions assume a 100 × 100 mm manufacturing panel and a 2 mm separation between PCBs. Actual panelization rules may vary between PCB manufacturers.
 
<p align="center">
<img width="30%" alt="image" src="https://github.com/user-attachments/assets/27c2d2b4-efbb-44ba-9353-04aeeab5edf3" />
</p>
<p align="center"><i>100 × 100 mm manufacturing panel with 4x Panel w49×49mm PCBs</i></p>

---

### 🧰 Matching KiCad Templates

Matching KiCad project templates are provided for the supported PCB Types.

The template names correspond directly to the PCB Library names, making it easy to select the correct KiCad template for a Boxify PCB definition.

See [KiCad Resources](../KiCad/README.md)

---

### 🔲 KiKit Panelization

Matching KiKit panelization configurations are provided for the supported Panel PCB Types.

These configurations can be used to panelize the PCB according to the corresponding Boxify Panel PCB definition

See [KiCad Resources](../KiCad/README.md)

---

# 🔄 Creating a PCB Library Entry

A typical workflow is:

1. Export the PCB from KiCad as STEP.
2. Import the STEP model into Onshape.
3. Create a Composite Part if required.
4. Open **PS KiCadStep**.
5. Select (or create) the appropriate PCB Type in the PCB Library.
6. Establish the Carrier / Passenger relationship.
7. Verify the PCB references.

The resulting PCB definition is derived into the Adapter Plate automatically.

The PCB Library entry then becomes reusable for the enclosure workflow.

For the complete import procedure, see [KiCad Integration](kicad-integration.md).

---

# 🔁 Updating a PCB

When the PCB design is revised, the KiCad STEP model can normally be updated without redesigning the enclosure architecture.

The general workflow is:

```text id="x9w7a8"
KiCad PCB revision
       │
       ▼
New STEP export
       │
       ▼
Replace / update Passenger
       │
       ▼
PS KiCadStep
       │
       │ Carrier remains reusable
       ▼
Adapter Plate
       │
       ▼
PS Enclosure
       │
       ▼
Updated enclosure
```

If the PCB outline, mounting locations or coordinate system remain compatible, the existing PCB Library definition can continue to be used.

If the actual PCB definition changes, the PCB Library entry may need to be updated or replaced.

---

<p align="center">
<img width="25%" alt="image" src="https://github.com/user-attachments/assets/3ae2a3f5-cb6f-4b84-bb2f-3e668f4aa7a9" />
<p>
<p align="center"><i>Updating the STEP model</i></p>

---

# 🧱 PCB Library and Adapter Plate

The PCB Library provides the PCB-specific references consumed by the Adapter Plate.

The Adapter Plate then translates those references into the mechanical interface required by the enclosure.

```text id="yflj28"
PCB Library
     │
     │ PCB definition
     ▼
Adapter Plate
     │
     │ mechanical interface
     ▼
PS Enclosure
```

This separation is important.

The Adapter Plate should not need to know how the PCB was designed in KiCad.

It only needs the stable PCB references provided by the PCB definition.

---

# ⚙️ PCB Configuration vs. Enclosure Configuration

PCB Type selection and orientation may be exposed through **PS Enclosure**, but the underlying PCB definition remains owned by the PCB Library.

For example:

### PCB Library

Defines:

- PCB Type
- PCB geometry
- Mounting locations
- Reference coordinate system
- PCB-specific information

### PS Enclosure

Selects and uses that definition and controls:

- PCB rotation
- Component side
- Mounting side
- Adapter Plate height
- Box height
- Lid fastening
- Inserts
- Cut-outs
- Labels

This distinction keeps the PCB reusable across different enclosure configurations.

---

# 🧠 Design Principles

## Reusable

A PCB Library entry should be reusable by multiple Boxify enclosure configurations.

## PCB-specific

The library should contain information about the PCB, not the enclosure.

## Stable

Reference geometry and coordinate systems should remain stable between compatible PCB revisions.

## Independent

The PCB definition should not depend on a particular enclosure design.

## Minimal

Only information required to define and interface the PCB should be stored in the library.

## Parametric

Where practical, PCB-specific geometry should be driven by parameters and stable references rather than manually duplicated dimensions.

---


# 🎯 Result

The PCB Library provides a stable and reusable definition of the PCB that sits between the electronic design and the mechanical enclosure.

Its role can be summarized as:

```text id="k8e8qf"
              KiCad
                │
                │ detailed STEP
                ▼
          PS KiCadStep
                │
                │ Passenger
                ▼
       ┌─────────────────┐
       │   PCB Library   │
       │                 │
       │     Carrier     │
       └────────┬────────┘
                │
                │ PCB definition
                ▼
         Adapter Plate
                │
                │ mechanical interface
                ▼
          PS Enclosure
                │
                ▼
        Box Base + Lid
```

The key principle is:

> **The PCB Library defines what the PCB is; PS Enclosure defines what the enclosure should be.**

This separation allows the same PCB definition to be reused across different enclosure configurations while keeping the enclosure independent from the PCB implementation.

---

## 🧩 Adding a New PCB Type

A new PCB Type can be added to the PCB Library without modifying the overall Boxify architecture.

A typical workflow is:

1. Create a new PCB Type in the PCB Library.
2. Define the PCB dimensions and relevant mounting/interface geometry.
3. Position the PCB Library Mate Connector at the **center of the PCB**.
4. Add the required configuration variables and options.
6. Verify the resulting Adapter Plate and enclosure geometry.
7. Verify the result in the Assembly.

The PCB Type acts as the **Carrier** for the actual PCB geometry used by Boxify. It provides the stable, reusable PCB definition on which the surrounding enclosure architecture depends.

When using an imported KiCad STEP model, make sure the PCB Type **has the same dimensions as the PCB in the STEP model** and that the PCB origin is centered accordingly.

The goal is to add a new PCB definition to the library rather than creating a separate, hard-coded implementation in the enclosure.

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)

---
