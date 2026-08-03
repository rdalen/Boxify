# 🏗 Design Philosophy

## 🧠 Introduction

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** designed for Onshape.

The primary goal of Boxify is to simplify enclosure design by separating the electronics from the mechanical enclosure.

Rather than creating a unique enclosure for every PCB, Boxify provides a reusable framework that adapts automatically to different electronic assemblies.

The framework is built around a small number of architectural principles that guide every design decision.

---

# 🎯 Design Goals

The design of Boxify is driven by the following goals.

## Reusability

Every major component should be reusable across multiple projects.

The framework should minimise duplicated CAD work and encourage the creation of reusable libraries.

---

## Separation of Responsibilities

Each Part Studio has a clearly defined responsibility.

Examples include:

- PCB definition
- Mechanical interface
- Enclosure generation
- Assembly verification

Keeping responsibilities separate makes the framework easier to maintain and extend.

---

## Parametric First

Geometry should adapt through parameters rather than manual editing.

Changing a configuration should regenerate the enclosure without requiring sketch modifications.

---

## Single Source of Truth

Every piece of information should exist only once.

Examples include:

- PCB dimensions
- Mounting hole locations
- Insert definitions
- Reference geometry

Other parts of the framework should derive or measure this information rather than duplicate it.

---

## Measured Geometry

Whenever possible, dimensions should be measured from existing geometry instead of entered manually.

Measured variables reduce duplicated information and improve robustness.

Examples include:

- PCB dimensions
- Adapter Plate height
- Lug height
- Internal enclosure dimensions

---

## Stable References

Stable reference geometry is preferred over recreated geometry.

The framework makes extensive use of:

- Derived Parts
- Composite Parts
- Mate Connectors
- Measured Variables

This improves regeneration reliability as the design evolves.

---

# 🏗 Framework Architecture

The Boxify framework consists of several independent layers.

```text
Electronics
──────────────────────
Passenger (Imported STEP)
PCB Library

Mechanical Interface
──────────────────────
Adapter Plate

Enclosure
──────────────────────
Box Base
Box Lid

Verification
──────────────────────
Assembly
```

Each layer builds upon the previous one while remaining independent.

---

# 🔄 Information Flow

Information flows in a single direction.

```text
Passenger
      │
      ▼
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

Downstream components consume information from upstream components but never modify them.

This keeps dependencies simple and predictable.

---

# 📦 The Passenger Concept

The imported STEP model is referred to as the **Passenger**.

Like a passenger travelling in a vehicle, it is carried by the Boxify framework but does not become part of the framework itself.

The Passenger provides an accurate mechanical representation of the electronics while remaining read-only.

This ensures that the original PCB design remains the single source of truth for the electronics.

---

# 🔩 Library-Based Design

Reusable libraries are a central concept within Boxify.

Current libraries include:

- PCB Library
- Threaded Insert Library

Future libraries may include:

- Fasteners
- Connectors
- Displays
- Buttons
- Ventilation patterns

Libraries reduce duplication and promote consistency across projects.

---

# ⚙️ Configurations and Variables

Boxify distinguishes between three types of values.

## Configurations

User choices describing what should be built.

Examples:

- PCB selection
- Box height
- Insert type

---

## Variables

Engineering parameters that control how the framework generates geometry.

Examples:

- Wall thickness
- Lip clearance
- Corner radius

---

## Measured Variables

Values derived automatically from geometry.

Examples:

- PCB width
- Adapter Plate thickness
- Lug height

This distinction keeps the user interface simple while maintaining engineering flexibility.

---

# 🚀 Extensibility

Boxify is designed to grow.

New features should extend the framework rather than replace existing functionality.

Examples include:

- New enclosure styles
- Additional PCB formats
- New insert families
- Manufacturing optimisations
- New hardware libraries

The core architecture should remain stable as the framework evolves.

---

# 💡 Design Principles

Every addition to Boxify should support these principles:

- Reuse before redesign.
- Measure before entering dimensions.
- Derive before recreating geometry.
- Configure before modifying sketches.
- Keep responsibilities separate.
- Build libraries instead of projects.
- Design for change.

---

# 🏁 Conclusion

Boxify is more than a collection of CAD models.

It is a framework that transforms a PCB design into a manufacturable enclosure through a modular, parametric and reusable architecture.

The long-term vision is to minimise repetitive CAD work while maximising flexibility, maintainability and reuse.
