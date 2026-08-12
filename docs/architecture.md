# 🏗 Architecture

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** built for Onshape.

Unlike traditional enclosure designs, Boxify separates the **electronics** from the **mechanical enclosure** by introducing an intermediate **Adapter Plate**.

This modular architecture allows the same PCB to be reused with different enclosure styles while keeping every stage fully parametric.

<img width="836" height="400" alt="image" src="https://github.com/user-attachments/assets/c668ee63-fbc0-4809-b90f-31d5326c883d" />
<p align="center"><i>Boxify Enclosure design</i></p>



---

# 🔄 Workflow Overview

```text
                 PCB Design
                     │
                     ▼
              Imported STEP Model
                     │
                     ▼
               PCB Library Entry
                     │
                     ▼
               Adapter Plate
          (Mechanical Interface)
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
      Box Base   Enclosure    Box Lid
          │                     │
          └──────────┬──────────┘
                     ▼
                  Assembly
```

Each stage has a single responsibility and can evolve independently.

The information flows in one direction, each stage builds upon the previous stage.

Changes made upstream automatically propagate downstream.

---

<p align="center">
<img width="50%" alt="image" src="https://github.com/user-attachments/assets/e1c10873-f9fd-4df8-9d88-fa98777c8af0" />
</p>
<p align="center"><i>KiCad STEP</i></p>
  
<p align="center">
  <img width="50%" alt="image" src="https://github.com/user-attachments/assets/79765599-6296-46cc-861c-e51c30c22aa4" />
</p>
<p align="center"><i>Boxify Enclosure design</i></p>

<p align="center">
  <img width="50%" alt="image" src="https://github.com/user-attachments/assets/865c9a41-163f-49ba-be22-f7ac81d1cc89" />
</p>
<p align="center"><i>3D printed parts</i></p>

<p align="center">
  <img width="50%" alt="image" src="https://github.com/user-attachments/assets/6fb12a9c-95a2-4db4-b1dd-10970b38fe7a" />
</p>
<p align="center"><i>Assembling & check to see if everything fits</i></p>
  
<p align="center">
  <img width="50%" alt="image" src="https://github.com/user-attachments/assets/2a43b5b7-7397-4ead-834f-749adac874b5" />
</p>
<p align="center"><i>Final result</i></p>


---

# 📦 Core Components

## PCB Library

The PCB Library defines the electronics.

In Boxify, this serves as the “Carrier” for the KiCad STEP Model (the “Passenger”)

Each PCB is represented by:

- PCB geometry
- Mounting hole locations
- PCB dimensions
- Reference geometry

The PCB Library never contains enclosure-specific information.

Its only purpose is to describe the PCB itself.


<img width="29%" alt="image" src="https://github.com/user-attachments/assets/f4776759-e6ac-4e63-8ff4-193a4fcfcd65" />
<img width="69%" alt="image" src="https://github.com/user-attachments/assets/16429a6e-03f9-459d-8346-272a7b01088f" />
<p align="center"><i>PCB Library with the common ProtoType PCB's</i></p>


---

## Adapter Plate

The Adapter Plate is the heart of Boxify.

It provides the mechanical interface between the PCB and the enclosure.

Responsibilities include:

- Component orientation
- Mounting orientation
- Defining mounting locations
- Providing reference geometry
- Passing this information to the enclosure

Because the enclosure depends only on the Adapter Plate, the PCB and enclosure remain loosely coupled.

---

## Enclosure

The enclosure consists of two parts:

- Box Base
- Box Lid

These are generated from the Adapter Plate rather than directly from the PCB.

Typical enclosure features include:

- Walls
- Bottom
- Lid
- Mounting lugs
- Screw pockets
- Threaded insert pockets

---

## Threaded Insert Library

Threaded inserts are managed independently from the enclosure.

Each insert definition contains:

- Insert dimensions
- Pocket geometry

The enclosure simply references the selected insert type.

---

## Assembly

The Assembly combines all generated components.

Its purpose is verification rather than design.

Typical checks include:

- PCB fit
- Component clearance
- Screw alignment
- Insert alignment
- Overall dimensions

---


# 📏 Parametric Design

Every important dimension in Boxify is controlled by variables.

Examples include:

- Wall thickness
- Box height
- Adapter Plate height
- PCB offset
- Corner radius
- Insert type

This allows an enclosure to adapt automatically when the PCB changes.

Configurations allow a single model to support multiple hardware variants.

Typical configurable options include:

- PCB selection
- PCB rotation
- Mount side
- Component side
  Adapter plate height 
- Box Height
- Wall thickness
- Lid fastening
- Treaded Insert type

---

# 🎯 Design Principles

Boxify follows several guiding principles.

---

**Separation of Responsibilities**

Each Part Studio has one clearly defined purpose.

---

**Reusable Components**

Components should be reusable across multiple projects whenever possible.

---

**Parametric First**

Geometry should adapt through parameters rather than manual editing.

---

**Measure Rather Than Duplicate**

Whenever geometry already contains the required information, measure it instead of entering it again.

---

**Stable References**

Use reference geometry and derived parts instead of recreating geometry.

---

**Modular Growth**

New functionality should extend the framework without requiring existing designs to change.

---

# 📚 Related Documentation

For implementation details, see:

- Getting Started
- PCB Library
- Adapter Plate
- Enclosure
- Threaded Insert Library
- KiCad Integration
- Configurations
