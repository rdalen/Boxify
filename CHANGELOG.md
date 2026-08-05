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
