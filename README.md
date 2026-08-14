# 📦 Boxify

**Parametric Electronics Enclosure Framework (PEEF)** for Onshape.

Boxify provides a reusable, configuration-driven framework for creating custom electronics enclosures from PCB designs.

Instead of designing an enclosure directly around a specific PCB, Boxify separates the electronics from the enclosure through a **parametric Adapter Plate**.

The result is a reusable enclosure architecture that can adapt to different PCB designs and configurations.

---

<img width="836" height="400" alt="image" src="https://github.com/user-attachments/assets/c668ee63-fbc0-4809-b90f-31d5326c883d" />
<p align="center"><i>Boxify Enclosure design</i></p>

---

# 🔗 Boxify Onshape Document

The parametric CAD model is maintained in Onshape:

👉 **[Open Boxify in Onshape](https://cad.onshape.com/documents/2b9b04247ce552c5f277d796)**

The GitHub repository contains the documentation and project history, while the Onshape document contains the parametric CAD model itself.

---

## 🧠 What is Boxify?

Boxify is designed around a simple principle:

> **The PCB defines the electronics. The Adapter Plate defines the mechanical interface. PS Enclosure defines what enclosure to build.**

The architecture separates the different responsibilities:

```text id="2a0p6m"
PCB / KiCad
     │
     │ detailed PCB STEP
     ▼
PS KiCadStep  <--- PCB Library
     │
     │ PCB + reusable prototype
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

This separation allows:

- Different PCBs to use the same enclosure architecture.
- Different enclosure configurations to be generated from the same PCB.
- PCB revisions to propagate through the mechanical design.
- Reusable PCB and hardware definitions to be maintained independently.
- Optional enclosure features to be dynamically enabled or suppressed.

---

# 🚀 Boxify v2.0

Boxify v2.0 represents a major step in the evolution of the framework.

The enclosure is now **configuration-driven**, with enclosure-related user configuration centralized in **PS Enclosure**.

This includes options such as:

- PCB selection and orientation
- Adapter Plate height
- Box dimensions
- Wall thickness
- Lid fastening type
- Threaded insert configuration
- Interface cut-outs
- Text labels

Configuration variables and features can change dynamically depending on the selected options.

This means the user normally does **not** need to:

- Switch between Part Studios to configure the enclosure.
- Edit Derive features.
- Maintain duplicate configuration values.
- Manually suppress irrelevant features.

Boxify now behaves as a **configurable enclosure generator rather than a collection of independently configured Part Studios**.

---

# 🔗 KiCad Integration

Boxify integrates with KiCad through the PCB STEP export.

The imported STEP model represents the detailed physical PCB assembly.

Boxify uses a **Carrier / Passenger** architecture:

```text id="j3h6vz"
PCB Library
     │
     │ reusable PCB prototype
     ▼
   Carrier
     ▲
     │
   Passenger
     │
     │ detailed geometry
     │
 KiCad STEP
```

The PCB Library provides the reusable PCB definition, while the imported STEP model provides the detailed representation of the actual PCB.

This allows the PCB STEP model to be updated while keeping the surrounding Boxify architecture reusable.

---

# 🏗 Core Architecture

The **Adapter Plate** is the central mechanical abstraction in Boxify.

It separates:

```text id="5q6x2v"
Electronics
    │
    ▼
PCB / KiCad
    │
    ▼
PS KiCadStep
    │
    ▼
Adapter Plate
    │
    ▼
PS Enclosure
    │
    ▼
Enclosure
```

The enclosure therefore does not need to depend directly on the internal structure of the PCB.

The Adapter Plate acts as the **contract between the PCB and the enclosure**.

---

# 📚 Documentation

The documentation is organized by responsibility.

### **[Getting Started](docs/getting-started.md)**
→ *How do I use Boxify?*

A practical introduction to the Boxify workflow, from importing a PCB to configuring and verifying an enclosure.

### **[Architecture](docs/architecture.md)**
→ *How is Boxify structured?*

Explains the overall architecture, responsibilities of each component and the flow of information through the framework.

### **[Configurations](docs/configurations.md)**
→ *How does configuration work?*

Explains user configuration, measured and derived variables, dynamic suppression and the configuration philosophy of Boxify.

### **[PCB Library](docs/pcb-library.md) / [KiCad Integration](docs/kicad-integration.md)**
→ *How do I bring my electronics into it?*

Explains the PCB Library, KiCad STEP integration and the Carrier / Passenger architecture.

### **[Adapter Plate](docs/adapter-plate.md)**
→ *How is the PCB separated from the enclosure?*

Explains the mechanical abstraction layer between the PCB and enclosure.

### **[Enclosure](docs/enclosure.md)**
→ *How is the enclosure generated?*

Explains the Box Base, Box Lid, fastening options, cut-outs, labels and dynamic feature suppression.

### **[Threaded Insert Library](docs/threaded-insert-library.md)**
→ *How are reusable fastening components handled?*

Explains reusable threaded insert definitions and their integration with PS Enclosure.

### **[Contributing](CONTRIBUTING.md)**
→ *How do I extend Boxify?*

Explains the development workflow, architecture principles, testing and contribution guidelines.

---

# ✨ Key Features

### 🧩 Parametric Enclosure

Generate a configurable Box Base and Box Lid from a single enclosure architecture.

### 🔌 KiCad STEP Integration

Use the actual PCB and component geometry exported from KiCad.

### 🚗 Carrier / Passenger Architecture

Separate the reusable PCB definition from the detailed imported STEP model.

### 🔧 Adapter Plate

Use a stable mechanical interface between electronics and enclosure.

### ⚙️ Centralized Configuration

Configure enclosure-related options from PS Enclosure.

### 🔩 Threaded Insert Library

Select reusable threaded insert definitions instead of hard-coding insert geometry into the enclosure.

### 🔄 Dynamic Suppression

Automatically enable or suppress optional features according to the selected configuration.

### 🔌 Interface Cut-outs

Generate enclosure cut-outs based on the available PCB/interface geometry.

### 🏷️ Configurable Labels

Add enclosure-specific labels without modifying the PCB definition.

---

# 📐 Design Philosophy

Boxify follows a few fundamental principles:

### Separation of Responsibilities

Each part of the architecture has a clear purpose.

### Configure Design Intent

Configuration describes **what the user wants to build**, not how the CAD features are implemented.

### Measure Rather Than Duplicate

If geometry already contains the required information, derive or measure it rather than entering the same value again.

### Reusable Components

Reusable definitions belong in libraries.

### Stable References

Downstream geometry should rely on stable mechanical references rather than fragile implementation details.

### Minimal Coupling

The enclosure should depend on the Adapter Plate rather than directly on the PCB implementation.

### Dynamic Configuration

Optional features should be enabled or suppressed according to configuration.

---

# 🛠️ Built With

- **[Onshape](https://www.onshape.com/)** — Parametric CAD
- **[KiCad](https://www.kicad.org/)** — PCB design and STEP export
- **GitHub** — Version control and documentation

---

# 📜 Version History

See the [CHANGELOG](CHANGELOG.md) for the Boxify development history.

Boxify v2.0 marks the transition to a centralized, configuration-driven enclosure workflow.

---

# 🤝 Contributing

Contributions are welcome!

Before making changes, please read the [Contributing Guide](CONTRIBUTING.md) and familiarize yourself with the [Architecture](docs/architecture.md).

---

# 📦 Project Status

Boxify is an actively evolving parametric enclosure framework.

The current **v2.0 architecture** provides:

- PCB Library integration
- KiCad STEP integration
- Carrier / Passenger architecture
- Parametric Adapter Plate
- Centralized enclosure configuration
- Box Base + Box Lid generation
- Threaded Insert Library
- Dynamic feature suppression
- Configurable lid fastening
- Interface cut-outs
- Configurable labels

Future development will continue to extend the framework while preserving its modular architecture.

---

**Happy Boxifying! 🚀**
