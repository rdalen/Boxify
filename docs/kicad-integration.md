# 📄 KiCad Integration

## 🧠 Purpose

Boxify supports custom PCBs designed in **KiCad** by importing the PCB as a STEP model.

This allows the enclosure to be generated from the actual mechanical representation of the PCB, including mounted components.

The KiCad STEP model provides the detailed geometry of the electronics, while Boxify provides the reusable mechanical framework around it.

The integration therefore separates:

- **Electronic design** → KiCad
- **Detailed PCB geometry** → imported STEP model
- **Reusable PCB definition** → PCB Library
- **Mechanical interface** → Adapter Plate
- **Enclosure configuration** → PS Enclosure

This keeps the PCB design as the source of truth for the electronics while allowing Boxify to generate the surrounding enclosure parametrically.

---

# 🔄 Position in the Boxify Workflow

```text
KiCad PCB
    │
    │ Export STEP
    ▼
PCB STEP Model
    │
    │ Import into Onshape
    ▼
PS KiCadStep  <--- PCB Library
    │
    │ PCB + reusable prototype
    ▼
Adapter Plate
    │
    │ mechanical interface
    ▼
PS Enclosure
    │
    │ user configuration
    ▼
Box Base + Box Lid
    │
    ▼
Assembly
```

The important distinction is:

> **The KiCad STEP model provides the detailed geometry; the PCB Library provides the reusable PCB definition used by the Boxify architecture.**

---

# 📤 Exporting from KiCad

Export the PCB from KiCad as a STEP model.

For best results:

- Include all mounted components that should be represented mechanically.
- Export using millimetres.
- Ensure component orientation is correct.
- Keep the PCB origin and coordinate system consistent between STEP exports.
- Include accurate 3D models for components that affect the enclosure.

The exported STEP model should represent the mechanical state of the electronic assembly that needs to fit inside the enclosure.

### PCB Origin

The PCB origin does **not** need to be centered specifically for Boxify.

What is important is that the origin and coordinate system remain consistent between PCB revisions.

This allows an updated STEP model to replace the previous model without unexpectedly changing the relationship between the PCB, Adapter Plate and enclosure.

---

# 📥 Importing the STEP Model

Import the STEP model into the Boxify Onshape document.

The imported model is treated as the **Passenger** in the Boxify carrier/passenger architecture.

The passenger represents the detailed physical PCB assembly:

- PCB
- Components
- Connectors
- Mechanical features
- Other relevant 3D models

The passenger is not intended to become the parametric definition of the enclosure.

Instead, it provides the detailed geometry required to represent the actual electronics.

---

<img width="50%" alt="image" src="https://github.com/user-attachments/assets/e55cbaf1-e128-4fd9-92a2-59690cd4e403" />
<p align="center"><i>An example of a zenertester design on a prototype board.</i></p>

---

# 🚗 Carrier / Passenger Architecture

Boxify separates the detailed imported STEP model from the reusable PCB definition.

```text
                 PCB Library
                     │
                     │ reusable prototype
                     ▼
                  CARRIER
                     │
                     │
                     ▲
                     │
                  PASSENGER
                     │
                     │ detailed geometry
                     │
               KiCad STEP
```

### Carrier

The **PCB Library prototype** acts as the **Carrier**.

It provides the stable reusable PCB definition used by the Boxify architecture.

The carrier can provide:

- PCB reference geometry
- PCB dimensions
- Mounting references
- Reference Mate Connector
- PCB-specific configuration information

### Passenger

The imported **KiCad STEP model** acts as the **Passenger**.

It provides:

- Actual PCB geometry
- Mounted components
- Connector geometry
- Detailed mechanical representation

The passenger can therefore be replaced when the PCB design changes without redesigning the Boxify architecture.

---

# 🧩 Creating a Composite Part

Imported STEP models can contain many individual parts.

Creating a **Composite Part** combines the imported PCB components into a single reusable reference.

This provides several advantages:

- Simplified selection
- Easier deriving
- Stable references
- Cleaner model organisation
- Easier handling of imported assemblies

The Composite Part represents the complete imported PCB assembly as a single mechanical reference.

---

<img width="50%" alt="image" src="https://github.com/user-attachments/assets/dab130d6-efd1-49a4-85e4-d71f27485fb3" />
<p align="center"><i>Composite part with aligned front-attached components.</i></p>

---

# 🏗 Creating a PCB Library Entry

After importing and preparing the STEP model:

1. Create the Composite Part.
2. Open **PS KiCadStep**.
3. Derive the imported Composite Part into PS KiCadStep.
4. Select an appropriate prototype from the **PCB Library**.
5. Use the PCB Library prototype as the Carrier.
6. Use the imported STEP geometry as the Passenger.
7. Establish the required relationship between the two.

The resulting PCB definition becomes reusable within Boxify.

```text
KiCad STEP
    │
    ▼
Composite Part
    │
    ▼
PS KiCadStep
    │
    ├───────────────┐
    │               │
    ▼               ▼
Passenger        Carrier
STEP geometry    PCB Library prototype
    │               │
    └───────┬───────┘
            ▼
      PCB definition
            │
            ▼
       Adapter Plate
```

---

<img width="50%" alt="image" src="https://github.com/user-attachments/assets/a7fffb1a-8e42-4ea5-98ee-bf901e7f4a7c" />
<p align="center"><i>The new PCB Library entry.</i></p>

---

# 📐 PCB Library Prototype

The selected PCB Library prototype should represent the PCB that the imported STEP model belongs to.

The prototype provides the reusable reference information required by the downstream Boxify architecture.

The imported STEP model supplies the detailed representation of the actual electronic assembly.

This separation means that the enclosure does not need to depend directly on the internal structure of the imported STEP model.

---

# 🔄 Updating the PCB

When the PCB design changes:

1. Update the PCB in KiCad.
2. Export a new STEP model.
3. Replace the existing imported STEP model in Onshape.
4. Update the Composite Part if necessary.
5. Verify the Passenger / Carrier relationship.
6. Regenerate the affected geometry.
7. Verify the resulting enclosure in the Assembly.

The Boxify architecture allows the updated PCB geometry to propagate through the Adapter Plate and enclosure.

The PCB Library prototype should only need to be changed when the **PCB definition itself** changes, rather than simply because the detailed STEP model has been updated.

---

# 🔁 Typical PCB Revision Workflow

```text
KiCad
  │
  │ PCB revision
  ▼
Export new STEP
  │
  ▼
Replace Passenger
  │
  ▼
Update Composite Part
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
Updated Box Base + Box Lid
  │
  ▼
Assembly verification
```

This makes the imported STEP model replaceable while preserving the reusable Boxify architecture around it.

---

# ⚙️ Configuration

PCB-related configuration is intentionally kept separate from enclosure configuration.

The PCB Library and PS KiCadStep provide the information required to define the PCB and its mechanical relationship.

Once the PCB is available to Boxify, enclosure-related choices are made in **PS Enclosure**.

Examples include:

- PCB selection
- PCB rotation
- Component side
- Mounting side
- Adapter Plate height
- Box dimensions
- Lid fastening
- Threaded inserts
- Cut-outs
- Labels

The user should therefore not need to modify PS KiCadStep when configuring the enclosure.

---

# 💡 Best Practices

### Keep the STEP model consistent

Keep the PCB origin and coordinate system consistent between revisions.

### Include accurate component models

Components that affect enclosure clearance should have accurate 3D models in KiCad.

### Use a Composite Part

Use a Composite Part to simplify the imported PCB assembly.

### Keep the Passenger unmodified

Avoid manually changing the imported STEP geometry to compensate for enclosure requirements.

If a mechanical correction is required, make the appropriate change in the source design or Boxify's parametric architecture.

### Keep the Carrier reusable

The PCB Library prototype should represent the reusable PCB definition rather than a particular imported STEP revision.

### Create an Onshape Version

Create a Version before replacing a PCB STEP model when making a significant PCB revision.

### Verify the Assembly

After updating the PCB, verify:

- PCB position
- Component clearances
- Adapter Plate alignment
- Mounting locations
- Cut-outs
- Lid clearance
- Overall enclosure fit

---

# ⚠️ Known Limitations

The quality of the resulting enclosure depends on the quality of the imported STEP model.

Current limitations include:

- Missing KiCad 3D models will not appear in the enclosure.
- Incorrect component orientation can result in incorrect mechanical geometry.
- Large PCB assemblies can increase Onshape regeneration time.
- Mechanical features that are not represented in the STEP model cannot be detected automatically.
- Changes to the physical PCB definition may require an updated PCB Library prototype.

---

# 🧠 Design Principle

The KiCad integration follows one important rule:

> **KiCad remains the source of truth for the electronics; Boxify remains the source of truth for the enclosure.**

The STEP model connects the two.

```text
┌──────────────┐
│    KiCad     │
│              │
│ PCB design   │
└──────┬───────┘
       │
       │ STEP
       ▼
┌──────────────┐
│ PS KiCadStep │
│              │
│ Carrier +    │
│ Passenger    │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Adapter      │
│ Plate        │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ PS Enclosure │
│              │
│ Configuration│
└──────┬───────┘
       │
       ▼
 Box Base + Lid
```

This separation is what allows Boxify to remain reusable while still generating an enclosure that accurately represents the actual PCB assembly.

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [Configurations](configurations.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)

---
