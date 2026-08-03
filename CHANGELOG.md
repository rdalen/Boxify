# 📜 Changelog

All notable changes to Boxify are documented in this file.

The format is based on Keep a Changelog, adapted for the Boxify development process.

---

## [1.6] – KiCad STEP Integration

### Added

- Added support for importing KiCad STEP models into Boxify.
- Introduced a dedicated **PS KiCadStep** workflow for integrating imported electronics.
- Imported STEP models are converted into a **Composite Part**, keeping the Parts list clean and manageable.
- Added support for coupling an imported STEP model to a selected PCB from the PCB Library, allowing the STEP model to act as a visual representation ("passenger") of the PCB reference.

### Changed

- Electronics are now assembled in a dedicated intermediate Part Studio before being derived into the Adapter Plate.
- The Adapter Plate now derives the complete PCB + KiCad STEP assembly instead of separate PCB geometry.
- Moved PCB orientation (rotation) from the PCB workflow to **PS Enclosure**, making board orientation an enclosure-level configuration.
- Renamed **PS Box** to **PS Enclosure** to better reflect its responsibility for enclosure-level configuration and positioning.

### Improved

- Cleaner project organization using dedicated folders for Part Studios and CAD imports.
- Greatly reduced Parts list clutter by encapsulating imported STEP assemblies into Composite Parts.
- Improved separation between reusable framework components and project-specific electronics.
- Established a scalable architecture for future projects using imported KiCad assemblies without changing the enclosure workflow.

### Architecture

The electronics workflow is now structured as:

PCB Library
→ KiCad STEP (Composite)
→ Mated Electronics
→ Adapter Plate
→ Enclosure

This preserves the PCB Library as the mechanical reference while allowing imported KiCad assemblies to follow all positioning and enclosure configuration automatically.

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
