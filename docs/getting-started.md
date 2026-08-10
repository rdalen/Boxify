# 🚀 Getting Started

Welcome to **Boxify**!

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** for Onshape that makes it easy to generate custom electronics enclosures from a PCB design.

Instead of designing a new enclosure for every project, Boxify separates the **electronics** from the **enclosure** using a reusable architecture based on an **Adapter Plate**.

---

# 📋 Prerequisites

Before using Boxify, you should have:

- An Onshape account
- Basic knowledge of Onshape Part Studios and Assemblies
- A PCB design (recommended: KiCad)
- A STEP model of your PCB

---

# 📂 Open the Boxify Document

Open the latest Boxify Onshape document using the link provided in the repository.

The document contains several Part Studios that together form the complete enclosure framework.

---

# 🔄 Typical Workflow

The normal workflow is:

```text
Create PCB
      │
      ▼
Export STEP
      │
      ▼
Import STEP into Boxify
      │
      ▼
Create PCB Library Entry
      │
      ▼
Configure Adapter Plate
      │
      ▼
Configure & Generate Box Base & Lid
      │
      ▼
Create Assembly
```

---

# 1️⃣ Import your PCB

Export your PCB from KiCad as a STEP model.

Import the STEP file into the Boxify document.

For best results:

- Keep the PCB origin unchanged
- Include all components
- Export using millimetres

<img width="618" height="604" alt="image" src="https://github.com/user-attachments/assets/42280f8b-9f39-426e-b917-bcd274f2daf1" />
<p align="center"><i>An example of a zenertester design on a prototype board.</i></p>

---

# 2️⃣ Create a PCB Library Entry

Create a PCB Library entry where the imported STEP model is the "passenger" on a "carrier" (= prototype PCB from the onshape PCB library)

The PCB Library becomes the reusable definition of your PCB.

---

# 3️⃣ Configure the Adapter Plate

The Adapter Plate connects your PCB to the enclosure.

Typical settings include:

- PCB selection
- Adapter Plate height
- Mounting offsets
- Screw locations
- Clearance

The Adapter Plate automatically adapts to the selected PCB.

---

# 4️⃣ Generate the Enclosure

The enclosure consists of:

- Box Base
- Box Lid

Configure parameters such as:

- Wall thickness
- Box height
- Corner radius
- Lip dimensions
- Clearances

The enclosure is generated automatically from the Adapter Plate.

---

# 5️⃣ Add Threaded Inserts

If desired, select a threaded insert type from the Threaded Insert Library.

Boxify automatically creates the corresponding mounting pockets.

---

# 6️⃣ Create the Assembly

Open the Assembly Part Studio.

Insert:

- Box Base
- Adapter Plate
- PCB
- Box Lid

The Assembly allows you to verify:

- Clearances
- Component fit
- Screw alignment
- Overall dimensions

---

# 📐 Design Philosophy

Boxify is built around a few simple principles:

- Keep the PCB independent of the enclosure.
- Make every important dimension configurable.
- Derive geometry instead of recreating it.
- Measure instead of hardcoding values.
- Reuse components wherever possible.

---

# 📚 Next Steps

For more information, continue with:

- **Architecture**
- **PCB Library**
- **Adapter Plate**
- **Enclosure**
- **Threaded Insert Library**
- **KiCad Integration**

---

# 💡 Tips

- Create a Version before making major changes.
- Keep imported PCB STEP files unchanged.
- Use Configurations rather than editing sketches.
- Group related features into folders.
- Build reusable library components whenever possible.

---

Happy designing!
