# 📄 Configurations

## 🧠 Purpose

Configurations allow Boxify to generate different enclosure variants from a single parametric model.

Instead of maintaining separate designs for different combinations of PCB, dimensions, mounting methods or fastening options, Boxify uses configuration variables to define **what the user wants to build**.

The goal is to keep the enclosure configuration:

- Centralized
- Meaningful
- Easy to understand
- Independent of implementation details
- Reusable across enclosure variants

---

# 🎯 Configuration Philosophy

Every user-facing configuration option should answer one simple question:

> **"What do I want to build?"**

Configurations should describe the **design intent**, not how the model is constructed internally.

For example:

- `boxHeight` describes the desired enclosure height.
- `wallThickness` describes the desired wall thickness.
- `lidFasteningType` describes how the lid should be secured.

The user should not need to know which sketches, features or Derive operations are used to create the resulting geometry.

---

# 🏗 Configuration Architecture

In Boxify, user-facing enclosure configuration is centralized in **PS Enclosure**.

```text
                         User Configuration
                                │
                                ▼
                        ┌───────────────┐
                        │  PS Enclosure │
                        └───────┬───────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
        PCB information   Adapter Plate     Insert definition
              │                 │                 │
       PCB Library        derived/measured   Threaded Insert
                            geometry            Library
              │                 │                 │
              └─────────────────┼─────────────────┘
                                ▼
                       Box Base + Box Lid
```

This creates a clear distinction between:

### User configuration

Values that define the desired enclosure.

### Library definitions

Reusable definitions supplied by the PCB Library and Threaded Insert Library.

### Derived and measured information

Engineering values obtained from existing geometry.

This separation prevents configuration from becoming a collection of duplicated dimensions and implementation-specific settings.

---

# ⚙️ PS Enclosure Configuration

**PS Enclosure** is the primary user-facing configuration point.

Typical configuration options include:

### PCB

- PCB rotation
- Component side up or down
- Mounting side above or below the adapter plate

*Note: PCB Type is not an enclosure configuration option. It is selected when deriving the PCB Library entry in PS KiCadStep.*

### Adapter Plate

- Adapter Plate height
- Other enclosure-relevant interface settings

### Enclosure

- Box height
- Wall thickness
- Bottom thickness
- Lid thickness

### Lid

- Lid fastening type

### Threaded Inserts

- Insert configuration
- Insert enable/disable

### Interface Features

- Cut-out options
- Text label options

The exact set of available options can evolve as Boxify gains new capabilities.

---

# 🔩 Lid Fastening Configuration

The lid fastening system is controlled through a dedicated configuration option.

The configuration describes the **intended fastening method**, rather than exposing the individual features used to create it.

For example:

- None
- Counterbore screws
- Countersunk screws
- Selftapping screws

The selected fastening type controls which related geometry is required.

Features that are not applicable to the selected configuration can be **dynamically suppressed**.

This allows several lid fastening variants to be generated from the same enclosure model.

---

# 🔩 Threaded Insert Configuration

The **Threaded Insert Library** provides reusable definitions for supported inserts.

PS Enclosure selects the required insert definition and uses it when generating the enclosure.

The configuration can also disable threaded inserts completely.

```text
Threaded Insert Library
          │
          │ insert definition
          ▼
    PS Enclosure
          │
          │ configuration
          ▼
   Insert pockets / lugs
```

The enclosure therefore does not need to contain a separate hard-coded implementation for every insert type.

---

# 🔄 Dynamic Configuration

Boxify uses configuration to control not only dimensions but also **feature availability**.

Some configuration variables and features are only relevant when another option is enabled.

For example:

```text
Insert configuration
       │
       ├── No Inserts
       │       └── Insert-related features suppressed
       │
       └── Inserts enabled
               └── Insert configuration becomes relevant
```

Similarly, lid fastening options can enable the geometry required for the selected fastening method while suppressing geometry that is not needed.

This keeps the generated model clean and prevents irrelevant configuration options from being exposed to the user.

---

# 📏 Measured Variables

Not every value in Boxify should be configurable.

Whenever a value can be obtained reliably from existing geometry, it should preferably be **measured or derived** rather than entered manually.

Examples include:

- PCB dimensions
- PCB mounting-hole positions
- Adapter Plate dimensions
- Adapter Plate thickness
- Lug dimensions
- Internal reference dimensions

Measured and derived variables provide information to downstream features without requiring the user to maintain duplicate values.

This improves model robustness and reduces the possibility of inconsistent configuration.

---

# 🔗 Configuration vs. Derived Information

It is important to distinguish between **configuration variables** and **engineering variables**.

### Configuration variables

Define what the user wants.

Examples:

```text
boxHeight
adapterPlateHeight
wallThickness
lidFasteningType
pcbRotation
insertConfiguration
```

### Measured / derived variables

Describe information already available in the model.

Examples:

```text
pcbWidth
pcbLength
mountingHolePosition
adapterPlateThickness
lugHeight
```

The general rule is:

> **If the user needs to choose it, configure it. If the model already knows it, derive or measure it.**

Some PCB properties are defined inputs rather than measured geometry, PCB thickness is one such property because it is required to position the detailed KiCad STEP before it enters the Adapter Plate.

```text
PCB definition
├── PCB length       → measured / derived
├── PCB width        → measured / derived
├── mounting holes   → measured / derived
└── PCB thickness    → defined input (`vs_pcbThickness`)

             ↓

        KiCad STEP
             ↓
      Adapter Plate
```

---

# 🧭 Configuration Flow

Configuration information flows through the Boxify architecture without requiring the user to manually propagate values between Part Studios.

```text
PCB / KiCad
     │
     ▼
PS KiCadStep  <--- PCB Library
     │
     ▼
Adapter Plate
     │
     │ derived / measured geometry
     ▼
PS Enclosure  <--- Threaded Insert Library
     │
     │ user configuration
     ▼
Box Base + Box Lid
```

The libraries provide reusable definitions, while PS Enclosure combines those definitions with the enclosure configuration.

This keeps the configuration workflow simple while preserving the modularity of the underlying architecture.

---

# 🧠 Configuration Guidelines

When adding new configuration options, follow these principles.

## Configure Intent

Expose parameters that describe the desired result.

Good examples:

- Box height
- Adapter Plate height
- Wall thickness
- PCB rotation
- Lid fastening type
- Insert option

Avoid exposing implementation-specific values unless the user genuinely needs to control them.

---

## Avoid Duplicate Information

A value should preferably be entered only once.

If it can be measured or derived from geometry, it should not become another independent configuration variable.

This reduces the risk of conflicting values.

---

## Centralize User Configuration

User-facing enclosure configuration belongs in **PS Enclosure**.

The user should not normally need to:

- Switch between Part Studios
- Edit Derive features
- Modify internal sketches
- Maintain duplicate configuration values

The underlying Part Studios should consume the configuration rather than require the user to manage it manually.

---

## Keep Libraries Reusable

Libraries should contain reusable definitions rather than project-specific configuration.

For example:

- PCB Library → reusable PCB definitions
- Threaded Insert Library → reusable insert definitions

The enclosure selects and consumes these definitions.

---

## Prefer Meaningful Names

Configuration variable names should describe their purpose and design intent.

Examples:

```text
boxHeight
wallThickness
adapterPlateHeight
lidFasteningType
insertConfiguration
```

Avoid names that expose implementation details or the internal feature structure.

---

## Separate Configuration from Engineering Variables

Do not expose measured or derived engineering values as user configuration unless there is a genuine design reason to do so.

This keeps the user interface focused on decisions the user actually needs to make.

---

## Use Dynamic Suppression Where Appropriate

When a configuration option makes a feature irrelevant, consider dynamically suppressing that feature.

This is particularly useful for:

- Optional threaded inserts
- Lid fastening methods
- Optional cut-outs
- Optional labels
- Other mutually exclusive enclosure features

Dynamic suppression allows one parametric model to represent several valid design variants without creating separate feature trees for each variant.

---

# 🚀 Configuration as a Design System

Boxify configuration is more than a collection of dimensions.

It defines a **design system** in which:

```text
User intent
     │
     ▼
Configuration
     │
     ▼
Reusable definitions
     │
     ▼
Derived / measured geometry
     │
     ▼
Dynamic feature selection
     │
     ▼
Generated enclosure
```

This is what allows Boxify to behave as a **configuration-driven enclosure generator** rather than a collection of independently configured Part Studios.

---

# 📚 Related Documentation

- 📄 [Getting Started](getting-started.md)
- 📄 [Architecture](architecture.md)
- 📄 [PCB Library](pcb-library.md)
- 📄 [Adapter Plate](adapter-plate.md)
- 📄 [Enclosure](enclosure.md)
- 📄 [Threaded Insert Library](threaded-insert-library.md)
- 📄 [KiCad Integration](kicad-integration.md)

---
