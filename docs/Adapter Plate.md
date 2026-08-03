# 📄 Adapter Plate

## 🧠 Purpose

The Adapter Plate forms the mechanical interface between the PCB Library ("the electronics") and the generic enclosure.

It determines:

- PCB position
- PCB orientation
- PCB mounting height
- Mounting side
- Mechanical interface to the enclosure

The enclosure itself remains completely independent of the electronics.

## ⚙️ Configuration

The PCB is instantiated using a **Derived Part**.

Changing the PCB configuration automatically rebuilds:

- Adapter Plate
- Box Base
- Box Lid

Additional configuration options include:

- Component Side
- Mount Side
- Rotation

## Outputs

- PCB support
- PCB mounting points
- Mechanical interface to the Box Base
- Structural reinforcement
