# 📄 KiCad Integration

## 🧠 Purpose

Boxify supports custom PCBs designed in KiCad by importing the PCB as a STEP model.

This integration allows the enclosure to be generated directly from the actual PCB design, including mounted components, ensuring an accurate mechanical fit.

The imported PCB becomes the starting point for the Boxify workflow.

---

# 🔄 Workflow Overview

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
Configure Adapter Plate
      │
      ▼
Configure & Generate Enclosure
      │
      ▼
Assembly
```

---

# 📤 Exporting from KiCad

Export the PCB as a STEP model.

For best compatibility:

- Include all mounted components
- Export using millimetres
- Make sure the PCB origin is centered
- Ensure component orientation is correct

The exported STEP model represents the complete electronic assembly.

---

# 📥 Importing the STEP Model

The STEP model exported from KiCad is imported into the Boxify document as a **passenger**.

The passenger is an accurate mechanical representation of the electronic assembly. It contains the PCB and all mounted components, but it does **not** participate in the parametric design of the enclosure.

Instead, the passenger provides the geometry that the PCB Library references and measures.

The passenger should remain unchanged after import. All positioning, orientation and downstream design decisions are handled by the Boxify framework.

Keeping the passenger "read-only" ensures that the original PCB design remains the single source of truth for the electronics.

---

# 🧩 Creating a Composite Part

Imported STEP models often contain many individual parts.

Create a **Composite Part** that combines all PCB components into a single reusable reference.

Advantages include:

- Simplified selection
- Easier deriving
- Stable references
- Improved model organisation

---

# 📚 Creating a PCB Library Entry

Derive the Composite Part into a new PCB Library Part Studio and select a from the PCB Library a prototype board with the same sizes

This PCB Library entry becomes the reusable definition of the PCB within Boxify.

It provides:

- PCB geometry
- Reference Mate Connector
- Reference geometry

---

# 🔄 Updating the PCB

When the PCB design changes:

1. Export a new STEP model from KiCad.
2. Replace the imported STEP file in Onshape.
3. Update the Composite Part if necessary.
4. Regenerate the affected Part Studios.

Because Boxify is fully parametric, changes propagate through the framework automatically.

---

# 💡 Best Practices

- Keep imported STEP models unmodified.
- Use Composite Parts for imported assemblies.
- Avoid editing derived geometry.
- Create a Version before replacing imported models.
- Keep the PCB origin consistent between exports.
- Use meaningful names for imported PCB revisions.

---

# ⚠ Known Limitations

Current limitations include:

- Mechanical accuracy depends on the exported STEP model.
- Missing component models will not appear in the enclosure.
- Very large PCB assemblies may increase regeneration time.

---

# 🚀 Future Enhancements

Potential future improvements include:

- Automated PCB update workflow
- Support for additional ECAD tools
- Direct PCB metadata import
- Automatic mounting hole detection
- Automatic enclosure regeneration
- Multi-board assemblies

---

# 📚 Related Documentation

- Getting Started
- PCB Library
- Adapter Plate
- Enclosure
- Architecture
