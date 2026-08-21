# 📜 Changelog

All notable changes to Boxify are documented in this file.

The format is based on Keep a Changelog, adapted for the Boxify development process.

---
## [2.1] – PCB Panelization Support

### Added
- Added **Panel PCB types** to the PCB Library for panelized PCB manufacturing.
- Added standard panel dimensions derived from a **100 × 100 mm manufacturing panel** with **2 mm mouse-bite spacing**:
  - Panel w100×100mm
  - Panel w49×100mm
  - Panel w49×49mm
  - Panel w23.5×49mm
  - Panel w23.5×23.5mm
- These configurations support common panel layouts such as **2×1, 2×2, 4×2 and 4×4** PCB arrangements.

### Improved
- Clearly separated **Prototype** PCB types from **Panel** PCB types in the PCB Library.
- Made it easier to configure Boxify for PCBs intended to be manufactured as a panel.

## [2.0] – Unified Enclosure Configuration

### Added

- Introduced a unified **Enclosure Configuration** workflow where all user-configurable enclosure options are available directly from **PS Enclosure**.
- Added configurable **Lid Fastening Types**, allowing different screw mounting strategies to be selected from a single configuration.
- Added support for **None**, **Counterbore screws**, **Countersunk screws** and **Selftapping screws** lid fastening options through automatic feature suppression.
- Added configurable automatic **lid text labels**.
- Added configurable automatic **interface cut-outs**.
- Added dynamic configuration visibility, showing only parameters relevant to the selected configuration.
- Added support for configuration-driven feature suppression, enabling multiple enclosure variants from a single Part Studio.

### Changed

- Centralized all enclosure-related configuration into **PS Enclosure**.
- Eliminated the need to switch between Part Studios during normal enclosure configuration.
- Eliminated manual editing of **Derive** features when changing enclosure options.
- Converted enclosure options into fully configuration-driven feature sets.
- Simplified the enclosure workflow into a single configuration interface.
- No Generic variables in Variable Studio needed anymore by using measuredVariables

### Improved

- Cleaner and more intuitive configuration experience.
- Reduced visual clutter by hiding irrelevant configuration variables.
- Improved maintainability through dynamic feature suppression.
- Increased scalability for adding future enclosure options without changing the overall architecture.
- Further strengthened the separation between framework logic and user configuration.
- Boxify now behaves as a configurable enclosure generator rather than a collection of parametric Part Studios.

---

## [1.7] – Automatic Interface Cut-outs

### Added

- Added automatic Box Lid cut-out generation based on the imported KiCad STEP assembly.
- Introduced a dedicated **Interface Projection** workflow using projected sketches to generate enclosure openings.
- Added support for automatic propagation of lid cut-outs when the PCB orientation changes.
- Added configurable front interface alignment within **PS KiCadStep**, providing a single reference for enclosure interfaces.

### Changed

- Moved front interface alignment from **PS Enclosure** to **PS KiCadStep**, making imported electronics responsible for their own interface positioning.
- Simplified the enclosure workflow by separating interface preparation from enclosure generation.
- Consolidated all lid openings into a single parametric cut-out sketch and extrusion.

### Improved

- Improved robustness against updated KiCad STEP imports while preserving enclosure references.
- Cleaner separation between imported electronics and enclosure geometry.
- Automatic regeneration of lid cut-outs after PCB rotation or STEP updates.
- Reduced feature count and improved maintainability of enclosure cut-out generation.
- Enhanced design workflow by allowing enclosure dimensions to be optimized interactively using the Adapter Plate position.

---

## [1.6] - KiCad STEP Integration

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
