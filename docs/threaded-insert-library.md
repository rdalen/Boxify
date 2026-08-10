# 📄 Threaded Insert Library

## 🧠 Purpose

The Threaded Insert Library provides a collection of reusable threaded insert definitions for the Boxify framework.

Each insert definition contains the geometry and dimensions required to generate the correct mounting pocket within an enclosure.

By separating insert definitions from the enclosure itself, different insert types can be selected without modifying the enclosure design.
<img width="800" height="580" alt="image" src="https://github.com/user-attachments/assets/7439bafd-b270-442a-9c83-bebcb6d6b8c8" />
<img width="1914" height="890" alt="image" src="https://github.com/user-attachments/assets/8a0189ef-4f5e-4c37-b752-3c603636a3fd" />
<p align="center"><i>Treaded brass inserts</i></p>

---

# 🏗 Architecture

```text
Threaded Insert Library
           │
           ▼
    Insert Definition
           │
           ▼
      Box Enclosure
           │
           ▼
Generated Insert Pocket
```

The enclosure references the selected insert definition to create the appropriate mounting geometry.

---

# 📦 Responsibilities

The Threaded Insert Library is responsible for defining:

- Insert dimensions
- Pocket geometry
- Installation clearance
- Counterbore dimensions (where applicable)
- Reference geometry

The library intentionally contains **no enclosure geometry**.

---

# ⚙️ Configuration

Typical configuration options include:

- Insert type
- Thread size
- Installation method

Future versions may also support:

- Material
- Manufacturer
- Installation tolerance
- Printing tolerance

---

# 📐 Insert Definition

Each insert definition contains the information required to generate a mounting pocket.

Typical parameters include:

- Thread size
- Insert length
- Outside diameter
- Pocket diameter
- Pocket depth
- Lead-in chamfer
- Clearance

These values are stored once and reused throughout the framework.

---

# 📤 Outputs

Each insert definition provides:

- Insert reference geometry
- Pocket geometry
- Installation dimensions
- Mounting references

These outputs are consumed by the Enclosure Part Studio.

---

# 🔩 Supported Inserts

The library is designed to support multiple insert families.

Examples include:

- Heat-set threaded inserts
- Press-fit inserts
- Brass inserts
- Plastic threaded inserts

Additional insert types can be added without changing the enclosure architecture.

---

# 🎯 Design Principles

## Library-Based Design

Every insert type is defined once and reused wherever required.

---

## Interchangeable Components

Changing the insert type should only require selecting another library definition.

No enclosure redesign should be necessary.

---

## Parametric First

Insert geometry is generated from configurable dimensions rather than fixed sketches.

---

## Separation of Responsibilities

The Threaded Insert Library defines the insert.

The Enclosure determines where inserts are placed.

---

## Extensible

New insert families can be added without affecting existing enclosure designs.

---

# 🚀 Future Enhancements

Potential future additions include:

- Manufacturer-specific insert libraries
- Metric and imperial insert families
- Automatic pocket tolerance selection
- Material-specific recommendations
- Installation guides
- Printable insert reference sheets

---

# 📚 Related Documentation

- Architecture
- Adapter Plate
- Enclosure
