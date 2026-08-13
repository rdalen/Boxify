# 📄 Configurations

## 🧠 Purpose

Configurations allow Boxify to support multiple hardware variants from a single parametric model.

Instead of maintaining separate designs for each enclosure, Boxify uses configuration parameters to adapt the framework to different PCB designs, mounting methods and enclosure requirements.

Configurations make Boxify flexible while preserving a single source of truth for the design.

---

# 🏗 Configuration Philosophy

Every configurable option should answer one simple question:

> **"What do I want to build?"**

Configurations describe the desired result.

They should **not** describe *how* the model is built internally.

---

# 📦 Configuration Levels

Configurations exist at several levels within the Boxify framework.

```text
PCB Library
      │
      ▼
Adapter Plate
      │
      ▼
Enclosure
      │
      ▼
Assembly
```

Each level exposes only the parameters relevant to its own responsibility.

---

# ⚙️ PCB Configuration

Typical PCB configuration options include:

- PCB selection

These settings define how the PCB is presented to the rest of the framework.

---

# 🔩 Adapter Plate Configuration

Typical Adapter Plate options include:

- All the upstream Part Studio Configuration options
- Component side
- Mounting side
- PCB offset
- Corner radius
- Mounting clearances
- Reference orientation

These settings determine the mechanical interface between the PCB and the enclosure.

---

# 📦 Enclosure Configuration

Typical enclosure options include:

- All the upstream Part Studio Configuration options
- PCB rotation
- Adapter Plate height
- Box height
- Wall thickness
- Bottom thickness
- Lid thickness
- Lid fastenings methode

These settings control the overall enclosure geometry.

---

# 🔧 Threaded Insert Configuration

Typical insert options include:

- Insert type
- Thread size

The enclosure automatically adapts to the selected insert definition.

---

# 📏 Measured Variables

Not every value in Boxify should be configurable.

Whenever possible, dimensions are measured directly from geometry.

Examples include:

- PCB dimensions
- Adapter Plate thickness
- Lug height
- Internal reference dimensions

Measured variables reduce duplicate information and improve model robustness.

---

# 🎯 Configuration Guidelines

When adding new configuration options, consider the following principles.

## Configure Intent

Expose parameters that describe the desired design, not the implementation.

Good examples include:

- Box height
- Wall thickness
- PCB rotation

Avoid exposing dimensions that can be measured automatically.

---

## Avoid Duplicate Information

A value should only be entered once.

If it can be derived or measured, it should not become a configuration parameter.

---

## Keep Configurations Local

Each Part Studio should only expose the parameters that belong to its own responsibility.

Avoid sharing unnecessary configuration options between Part Studios.

---

## Prefer Meaningful Names

Configuration names should describe the design intent.

For example:

- `boxHeight`
- `wallThickness`
- `adapterPlateHeight`

rather than implementation-specific names.

---

## Maintain Backward Compatibility

Where possible, new configuration options should not invalidate existing designs.

This allows Boxify projects to evolve over time with minimal disruption.

---

# 🚀 Future Enhancements

Potential future configuration options include:

- Snap-fit enclosure
- Waterproof enclosure
- Ventilation style
- Mounting style
- Cable entry type
- Display cut-outs
- Button layouts

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)
- 📄 [Configurations](configurations.md)
