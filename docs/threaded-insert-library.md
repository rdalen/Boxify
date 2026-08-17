# 🔩 Threaded Insert Library

## 🧠 Purpose

Boxify supports optional **threaded inserts** for securing the enclosure.

The **Threaded Insert Library** contains reusable definitions for supported insert types. These definitions are consumed by **PS Enclosure** when threaded inserts are enabled.

This separates the reusable insert geometry from the enclosure itself.

The result is a flexible system in which the same enclosure architecture can be generated:

- Without threaded inserts
- With different threaded insert types
- With different insert configurations as the library expands

---

<img width="39%" alt="image" src="https://github.com/user-attachments/assets/7439bafd-b270-442a-9c83-bebcb6d6b8c8" />
<img width="59%" alt="image" src="https://github.com/user-attachments/assets/8a0189ef-4f5e-4c37-b752-3c603636a3fd" />
<p align="center"><i>Boxify Threaded inserts Library</i></p>


---

# 🏗 Position in the Boxify Architecture

```text
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

The **Threaded Insert Library** supports PS Enclosure in the same way that the **PCB Library** supports PS KiCadStep.

The library provides reusable definitions; PS Enclosure determines how those definitions are used.

---

# 📚 Threaded Insert Library

The Threaded Insert Library contains reusable definitions for threaded inserts.

A library entry can define:

- Insert type
- Thread size
- Insert dimensions
- Recommended pocket dimensions
- Installation geometry
- Reference geometry
- Other insert-specific parameters

The library should describe the **physical insert**, rather than the enclosure in which it will be used.

---

# 🔩 Insert Definition

A threaded insert definition represents the geometry required to accommodate a particular insert.

For example:

```text id="z8q4mn"
Threaded Insert
     │
     ├── Thread size
     ├── Outer diameter
     ├── Length
     ├── Pocket diameter
     └── Pocket depth
```

The exact parameters depend on the insert type.

The purpose is to provide a reusable definition that can be consumed by the enclosure features.

---

# ⚙️ Configuration

Threaded insert configuration is controlled from **PS Enclosure**.

The user can select whether threaded inserts are required and, when enabled, which insert definition should be used.

```text id="4x0j9k"
PS Enclosure
     │
     │ Insert configuration
     ▼
Threaded Insert Library
     │
     │ selected insert definition
     ▼
Enclosure insert geometry
```

This keeps the user-facing configuration centralized.

The user should not normally need to edit the Threaded Insert Library when configuring an enclosure.

---

# 🚫 No Inserts

Threaded inserts are optional.

The enclosure can be configured with:

> **No Inserts**

When inserts are disabled, all insert-related features are dynamically suppressed.

```text id="h7p2qw"
Insert Configuration
        │
        ├── No Inserts
        │      │
        │      └── Insert features suppressed
        │
        └── Insert selected
               │
               └── Insert geometry enabled
```

This allows the same enclosure model to support both insert and non-insert variants.

---

# 🔄 Dynamic Suppression

Boxify uses **dynamic suppression** to control insert-related features.

When threaded inserts are not required:

- Insert pockets are suppressed.
- Insert-related lug features are suppressed where applicable.
- Other insert-dependent geometry is suppressed.

When inserts are enabled, the corresponding features become active.

This avoids maintaining separate enclosure models for different insert configurations.

---

# 🧱 Insert Geometry

The enclosure does not simply model the threaded insert itself.

Instead, Boxify generates the **mechanical accommodation geometry** required to install the selected insert.

Depending on the insert definition, this can include:

- Insert pocket
- Bore
- Counterbore
- Installation clearance
- Supporting lug geometry

The insert definition therefore drives the geometry required by the enclosure.

---

# 📍 Insert Locations

Insert locations are determined by the enclosure architecture.

The Threaded Insert Library defines **what the insert is**.

The enclosure defines **where the insert is used**.

This distinction is important:

```text id="6r9m2d"
Threaded Insert Library
        │
        │ What?
        ▼
   Insert definition

PS Enclosure
        │
        │ Where?
        ▼
   Insert locations
```

Insert locations therefore remain part of the enclosure design rather than becoming part of the reusable insert library.

---

# 🔄 Configuration Flow

The complete relationship is:

```text id="x4v8pc"
Threaded Insert Library
          │
          │ reusable definition
          ▼
     PS Enclosure
          │
          │ user configuration
          ▼
   Insert feature logic
          │
          ▼
     Box Base / Lid
```

This allows the library to remain independent of any particular enclosure.

---

# 🧩 Adding a New Insert Type

A new insert type can be added to the library without redesigning the overall enclosure architecture.

A typical workflow is:

1. Add the new insert definition to the Threaded Insert Library.
2. Define its relevant dimensions and installation geometry.
3. Make the definition available to PS Enclosure.
4. Add the new option to the appropriate configuration.
5. Use the **Lug Verification Help** to verify that the insert and surrounding lug geometry are correctly sized and positioned.
6. Verify the generated enclosure geometry.
7. Verify the result in the Assembly.

The **Lug Verification Help** provides a visual check of the insert installation geometry and helps identify potential clearance or positioning issues before finalizing the enclosure.

The enclosure should consume the new definition rather than containing a completely separate hard-coded implementation.
<p align="center">
<img width="25%" alt="image" src="https://github.com/user-attachments/assets/3943e839-669b-4988-908c-2c3f80364760" />
<p>
<p align="center"><i>Lug Verification Help</i></p>

---

# 🧪 Verification

When using a threaded insert configuration, verify:

- Insert type
- Thread size
- Insert pocket diameter
- Insert pocket depth
- Insert position
- Lug Verification Help
- Lug geometry
- Wall thickness around the insert
- Screw clearance
- Lid alignment
- Installation accessibility

The final result should always be verified in the Assembly.

---

# 🧠 Design Principles

## Reusable Definitions

The library contains reusable insert definitions rather than project-specific enclosure geometry.

## Centralized Configuration

Insert selection belongs to **PS Enclosure**.

## Separate "What" and "Where"

The library defines **what the insert is**.

The enclosure defines **where it is used**.

## Optional by Design

Threaded inserts are an optional enclosure feature.

## Dynamic

Insert-related features should be dynamically suppressed when inserts are disabled.

## Parametric

Insert geometry should be driven by the insert definition rather than duplicated manually in enclosure features.

## Extensible

New insert types should be addable without fundamentally changing the enclosure architecture.

---

# 🎯 Result

The Threaded Insert Library allows Boxify to support different fastening hardware while keeping the enclosure architecture reusable.

The complete relationship can be summarized as:

```text id="8m2k7v"
                    Threaded Insert
                       Library
                          │
                          │ What?
                          ▼
                    Insert Definition
                          │
                          ▼
                     PS Enclosure
                          │
                          │ Where?
                          ▼
                   Insert Locations
                          │
                          ▼
                  Insert Pocket Geometry
                          │
                          ▼
                    Box Base + Lid
```

The key principle is:

> **The Threaded Insert Library defines the insert; PS Enclosure decides whether and where it is used.**

This keeps reusable hardware definitions separate from enclosure-specific design decisions.

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)

---
