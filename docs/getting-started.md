# 🚀 Getting Started

Welcome to **Boxify**!

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** for Onshape that makes it easy to generate custom electronics enclosures around PCB-based electronics

Instead of designing a new enclosure for every project, Boxify separates the **electronics** from the **enclosure** using a reusable architecture based on an **Adapter Plate**.

The Boxify workflow is configuration-driven: enclosure-related settings are managed from **PS Enclosure**, while the underlying Part Studios provide the reusable geometry and libraries.

---

<img width="836" height="400" alt="Boxify Enclosure design" src="https://github.com/user-attachments/assets/c668ee63-fbc0-4809-b90f-31d5326c883d" />

<p align="center"><i>Boxify Enclosure design</i></p>

---

# 📋 Prerequisites

Before using Boxify, you should have:

- An Onshape account
- Basic knowledge of Onshape Part Studios and Assemblies
- A PCB design
- A STEP model of the PCB
- KiCad is recommended for PCB and STEP generation

---

# 📂 Open the Boxify Document

Open the latest Boxify Onshape document using the link provided in the repository.

The document contains several Part Studios that together form the complete enclosure framework.

### Main components

- **PS Enclosure**
  - Main user-facing configuration point
  - Controls enclosure-related configuration
  - Generates the Box Base and Box Lid

- **PS Adapter Plate**
  - Provides the mechanical interface between the PCB and enclosure
  - Uses the selected PCB geometry and configuration

- **PS KiCadStep**
  - Integrates an imported PCB STEP model with a reusable PCB Library prototype
  - The STEP model acts as the **passenger** on the PCB Library **carrier**

- **PS LIBs**
  - **PS PCB Library**
  - **PS Threaded Insert Library**

The internal Part Studios normally do not need to be edited during regular enclosure configuration.

---

# 🔄 Typical Workflow

The normal Boxify workflow is:

```text
Create PCB
    │
    ▼
Export PCB as STEP
    │
    ▼
Import STEP into Boxify
    │
    ▼
Create / select PCB Library entry
    │
    ▼
Configure the enclosure in PS Enclosure
    │
    ├── PCB Type, -orientation and -clearences
    ├── Adapter Plate height
    ├── Box height
    ├── Lid fastening
    ├── Threaded inserts
    ├── Cut-outs
    └── Labels
    │
    ▼
Generated Box Base + Box Lid
    │
    ▼
Verify in Assembly
```

You normally configure the complete enclosure from PS Enclosure. There is no need to switch between Part Studios to edit configuration variables or manually modify Derive features.

---

# [1️⃣ Import your PCB](kicad-integration.md#importing-the-step-model)

Export your PCB from KiCad as a STEP model and import it into the Boxify document.

For best results:

- Make sure the PCB origin is always in the center of the PCB board (Boxify uses the centered Mate Connector in the PCB Library as the reference for mating the imported STEP model)
- Keep the PCB origin and coordinate system consistent between STEP exports.
- Include the PCB and all components that should be represented in the enclosure.
- Export using millimetres.
- Align the components that interface with the enclosure front to the same level in KiCad (before exporting) or manually in Onshape after importing

The imported STEP model becomes the detailed representation of the actual PCB assembly.

---

<p align="center">
<img width="50%" alt="image" src="https://github.com/user-attachments/assets/416cff42-1892-4567-96c6-c941a4b0970f" />
</p>
<p align="center"><i>Example: a STEP file of a Zenertester design on a prototype board.</i></p>

---

# [2️⃣ Create a PCB Library Entry](kicad-integration.md#creating-a-pcb-library-entry)

Create a PCB Library entry for the imported PCB.

Boxify uses a **carrier/passenger architecture**:

- The PCB Library PCB is the reusable **carrier**.
- The imported KiCad STEP model is the **passenger**.
- The combination provides both reusable PCB reference geometry and the detailed representation of the actual PCB.

The PCB Library therefore becomes the reusable definition of the PCB used by the enclosure.

Once the PCB Library entry is established, the PCB Type can be selected from the enclosure configuration.

---

# [3️⃣ Configure the Enclosure](enclosure.md)

Open **PS Enclosure**.

This is the main configuration point for the Boxify enclosure.

Depending on the selected configuration, you can control parameters such as:

- PCB Type selection
- PCB orientation
- Component side up or down
- Mounting side above or below the adapter plate
- Adapter Plate height
  Box height
- Box-wall, box-bottom and box-lid  thickness
- Lid fastening type
- Threaded insert configuration
- Interface cut-outs
- Text labels

Configuration variables and features are shown or suppressed dynamically according to the selected options.

This allows the same Boxify enclosure framework to support different enclosure variants without manually editing individual Part Studios or Derive features.

---
<p align="center">
<img width="50%" alt="image" src="https://github.com/user-attachments/assets/3804b72e-1345-4d8f-b7f1-c345c5bd8106" />
</p>
<p align="center"><i>Adjust the Adapter Plate height and Box height so that everything fits properly.</i></p>

---

# [4️⃣ Generate the Enclosure](enclosure.md)

The enclosure consists of:

- **Box Base**
- **Box Lid**

Both are generated from the common enclosure configuration and the geometry provided by the Adapter Plate.

The generated enclosure automatically adapts to the selected PCB Type and its associated configuration.

The Box Lid and Base can be configured with different fastening options. Features that are not required by the selected configuration are automatically suppressed.

---

# [5️⃣ Configure Threaded Inserts](threaded-insert-library.md)

If threaded inserts are required, select the appropriate insert configuration from the **Threaded Insert Library**.

Boxify uses the selected insert definition to generate the corresponding mounting geometry.

Depending on the configuration, threaded inserts can be enabled or disabled without manually editing the enclosure features.

This makes it possible to use the same enclosure design for configurations with or without threaded inserts.

---

# 6️⃣ Verify the Enclosure in Assembly

After generating the enclosure, open the **Assembly**.

Insert or verify:

- Box Base
- Adapter Plate
- PCB
- Box Lid

The Assembly provides the final verification of the generated enclosure.

Check:

- Clearances
- Component fit
- PCB position
- Screw and insert alignment
- Lid fit
- Interface cut-outs
- Overall dimensions

The Assembly is intended primarily as a **verification environment**. Configuration changes should normally be made in **PS Enclosure**.

---

# 🧭 Understanding the Boxify Workflow

The Boxify architecture separates the design into several responsibilities:

```text
PCB / KiCad
     │
     │ detailed PCB STEP
     ▼
PS KiCadStep  <--- PCB Library
     │
     │ PCB + reusable PCB Type
     ▼
Adapter Plate
     │
     │ mechanical interface
     ▼
PS Enclosure <--- Treaded Insert Library
     │
     │ user configuration
     ▼
Box Base + Box Lid
     │
     ▼
Assembly
```

The key principle is:

> **Configure the enclosure from PS Enclosure; let the underlying architecture propagate the required geometry and configuration.**

This keeps the electronics, mechanical interface, enclosure generation, and final assembly clearly separated.

---

# 📚 Next Steps

For more information, continue with:

- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)

---

# 💡 Tips

- Create an Onshape Version before making major changes.
- Configure the enclosure from **PS Enclosure** rather than editing downstream Part Studios.
- Use configuration variables instead of manually editing sketches or Derive features.
- Keep reusable geometry in the appropriate library.
- Group related features into logical folders.
- When developing new features, keep user configuration separate from engineering and measured variables.

---

**Happy Boxifying! 🚀**
