# 📜 Changelog

All notable changes to Boxify are documented in this file.

The format is based on Keep a Changelog, adapted for the Boxify development process.

---

## [1.6] - KiCad STEP Integration (Work in Progress)

### Added

- Support for imported KiCad STEP models ("Passenger")
- Composite Part workflow for imported PCB assemblies
- Derive workflow from Composite Part into the PCB Library
- Configurable PCB positioning using Mate Connectors
- Improved document organization using folders

### Improved

- Separation between imported PCB geometry and parametric framework
- Stable workflow for updating imported PCB revisions
- Foundation for future ECAD integrations

---

## [1.5] - Threaded Insert Library

### Added

- Reusable Threaded Insert Library
- Parametric insert definitions
- Automatic insert pocket generation
- Configurable insert selection

### Improved

- Library-based insert architecture
- Separation between enclosure geometry and insert definitions

---

## [1.4] - Enclosure Architecture

### Added

- Fully parametric enclosure generation
- Shared Box Base / Box Lid architecture
- Improved enclosure feature organization

### Improved

- Better feature grouping
- Cleaner enclosure workflow
- Increased design robustness

---

## [1.3] - Threaded Insert Preparation

### Added

- Mounting lug architecture
- Initial insert placement strategy
- Boolean workflow experiments

### Improved

- Lug geometry
- Preparation for reusable insert library

---

## [1.2] - Parametric Box Base & Lid

### Added

- Unified Box Base and Box Lid Part Studio
- Configurable Box Height
- Configurable Adapter Plate Height
- Measured Adapter Plate Height
- Automatic Lug Height calculation
- Feature folders for improved organization

### Improved

- Shared design context
- Better separation between user variables and engineering variables
- Improved maintainability

---

## [1.1] - Parametric Box Base

### Added

- First parametric Box Base
- Automatic wall generation
- Initial enclosure architecture

### Improved

- Integration with the Adapter Plate
- Foundation for future lid generation

---

## [1.0] - Stable Adapter Plate Architecture

### Added

- Configurable PCB Library
- Adapter Plate framework
- Measured PCB variables
- Configurable PCB Rotation
- Configurable Component Side
- Configurable Mount Side

### Improved

- Single source of truth for PCB geometry
- Robust configuration handling
- Generic enclosure architecture

---

## Pre-release History

### v0.3

- Introduced measured variables
- Improved variable ownership
- Reduced duplicated dimensions

### v0.2

- Introduced the Adapter Plate concept
- Separated PCB and enclosure architecture

### v0.1

- First configurable PCB Library
- Initial proof of concept
