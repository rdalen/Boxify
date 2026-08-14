# 🤝 Contributing to Boxify

Thank you for your interest in contributing to **Boxify**!

Boxify is a **Parametric Electronics Enclosure Framework (PEEF)** for Onshape. Contributions are welcome, whether they improve the parametric architecture, add reusable library definitions, improve documentation, or introduce new enclosure capabilities.

The goal is to keep Boxify **modular, reusable, predictable and configuration-driven**.

---

# 🏗 Understand the Architecture First

Before making changes, it is recommended to understand the Boxify architecture.

The main design flow is:

```text id="y8k4rx"
PCB / KiCad
     │
     │ detailed STEP
     ▼
PS KiCadStep  <--- PCB Library
     │
     │ PCB definition
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

Each part of the architecture has a specific responsibility.

### PCB Library

Defines reusable PCB-specific geometry and references.

### PS KiCadStep

Combines the PCB Library prototype with the detailed KiCad STEP model.

### Adapter Plate

Provides the mechanical abstraction between the PCB and enclosure.

### PS Enclosure

Provides the main user-facing enclosure configuration and generates the enclosure.

### Threaded Insert Library

Provides reusable definitions for supported threaded inserts.

### Assembly

Provides the final environment for verification.

For more information, see [Architecture](docs/architecture.md).

---

# 🎯 Contribution Principles

Boxify is intended to remain a **framework**, rather than becoming a collection of project-specific solutions.

When contributing, keep the following principles in mind.

## Separation of Responsibilities

New functionality should be implemented in the part of the architecture responsible for it.

For example:

- PCB-specific functionality → PCB Library / PS KiCadStep
- Mechanical interface functionality → Adapter Plate
- Enclosure functionality → PS Enclosure
- Insert definitions → Threaded Insert Library

Avoid placing functionality in a Part Studio simply because it is convenient.

---

## Centralized User Configuration

User-facing enclosure configuration belongs in **PS Enclosure**.

Avoid requiring users to:

- Edit downstream Part Studios
- Modify Derive features
- Maintain duplicate configuration variables
- Manually propagate configuration values

If a new feature requires user input, consider whether that input belongs in the PS Enclosure configuration.

---

## Configure Design Intent

Configuration variables should describe **what the user wants to build**, not how the geometry is implemented.

Prefer:

```text id="8s8vny"
lidFasteningType
boxHeight
wallThickness
insertConfiguration
```

over implementation-specific settings that expose internal feature details.

---

## Measure Rather Than Duplicate

If information can be obtained from existing geometry, prefer measuring or deriving it instead of introducing another manually maintained variable.

For example:

- PCB dimensions
- Mounting-hole positions
- Adapter Plate height
- Reference locations

This reduces the possibility of conflicting values.

---

## Use Dynamic Suppression

Optional features should be dynamically suppressed when they are not relevant to the selected configuration.

This is particularly useful for:

- Threaded inserts
- Lid fastening methods
- Optional cut-outs
- Optional labels

The goal is to allow one parametric model to represent multiple valid configurations.

---

## Prefer Reusable Libraries

If a new component represents a reusable physical definition, consider whether it belongs in a library.

Examples include:

- PCB definitions → PCB Library
- Threaded insert definitions → Threaded Insert Library

Avoid hard-coding reusable definitions into an individual enclosure.

---

# 🔧 Making Changes

Before making a significant change:

1. Create a new Git branch.
2. Understand the affected part of the Boxify architecture.
3. Identify which Part Studio or library should own the change.
4. Make the smallest change that solves the problem.
5. Test the resulting configuration.
6. Verify the final enclosure in the Assembly.
7. Update the documentation if the behavior or workflow has changed.

Use descriptive branch names, for example:

```text id="6l4v8k"
feature/lid-fastening
feature/insert-library
feature/interface-cutouts
fix/pcb-reference
docs/update-architecture
```

---

# 🧪 Testing Changes

Changes to parametric CAD models should be tested through the relevant configuration paths.

At minimum, verify:

### Geometry

- Expected geometry is generated.
- No unexpected geometry is created.
- References remain stable.

### Configuration

- Configuration variables behave as expected.
- Dependent options appear or disappear correctly.
- Dynamic suppression works correctly.

### PCB Integration

When applicable:

- PCB geometry is correctly positioned.
- Adapter Plate references remain valid.
- Updated STEP geometry behaves correctly.

### Enclosure

Verify:

- Box Base
- Box Lid
- Fastening features
- Threaded insert features
- Cut-outs
- Labels
- Clearances

### Assembly

Always perform a final verification in the Assembly for significant mechanical changes.

---

# 🔄 Testing Different Configurations

Because Boxify is configuration-driven, testing only the default configuration is not sufficient for changes that affect configurable features.

For example, a change to lid fastening should be tested with relevant alternatives:

```text id="v3n6gs"
Lid Fastening
    │
    ├── None
    ├── Counterbore screws
    ├── Countersunk screws 
    └── Selftapping screws
```

Likewise, threaded insert changes should be tested with:

```text id="5f3n2k"
Threaded Inserts
    │
    ├── No Inserts
    └── Insert selected
```

The exact test matrix depends on the feature being changed.

---

# 📐 Onshape Versions

Create an **Onshape Version** before making significant changes to the model.

Versions provide a stable reference point for:

- Testing
- Comparing changes
- Reporting issues
- Reverting changes
- Reproducing previous configurations

When reporting or reviewing a CAD issue, include the relevant Onshape Version whenever possible.

---

# 📝 Documentation

Documentation is part of the Boxify project.

If a contribution changes:

- User workflow
- Configuration
- Architecture
- Feature behavior
- Library structure
- Naming
- Installation or usage

update the relevant documentation as part of the same change.

Documentation should describe the **current behavior**, not the intended future behavior.

Keep terminology consistent with the architecture:

- **PCB Library**
- **PS KiCadStep**
- **Adapter Plate**
- **PS Enclosure**
- **Threaded Insert Library**
- **Box Base**
- **Box Lid**
- **Assembly**
- **Carrier**
- **Passenger**
- **Configuration**
- **Measured / Derived Variables**

---

# 🌿 Branching

Use a separate branch for development work.

A typical workflow is:

```text id="5j0t3d"
main
 │
 ├── feature/...
 │
 ├── fix/...
 │
 └── docs/...
```

Keep branches focused on a single logical change whenever possible.

Avoid combining unrelated CAD changes, documentation changes and refactoring into one branch unless they are directly dependent on each other.

---

# 💾 Commits

Use clear and descriptive commit messages.

Examples:

```text id="4u8m1v"
Add dynamic lid fastening configuration
Add countersink lid option
Update threaded insert library
Fix Adapter Plate reference
Update v2.0 architecture documentation
```

A commit should ideally represent one logical change.

---

# 🔀 Pull Requests

Before opening a Pull Request:

- Verify the affected configurations.
- Verify the Assembly.
- Check for broken references.
- Update documentation where required.
- Review changed files.
- Remove temporary test geometry or unused features.
- Ensure naming is consistent.

The Pull Request description should explain:

### What changed

Describe the functionality or problem being addressed.

### Why

Explain the reason for the change.

### How

Briefly describe the implementation where useful.

### Testing

Describe which configurations and assemblies were verified.

---

# 🐛 Reporting Issues

When reporting a problem, provide enough information to reproduce it.

Where applicable, include:

- Boxify version
- Onshape Version
- PCB / project involved
- Configuration used
- Expected behavior
- Actual behavior
- Relevant screenshots
- Steps to reproduce

For configuration-related problems, include the relevant configuration values.

---

# 🧠 Design Review

For significant architectural changes, consider the following questions:

1. Does the change respect the existing separation of responsibilities?
2. Does it belong in the correct Part Studio or library?
3. Can the functionality be reused?
4. Does it introduce duplicate configuration?
5. Can information be measured or derived instead?
6. Can unnecessary features be dynamically suppressed?
7. Does it introduce tighter coupling between PCB and enclosure?
8. Does the user need to understand internal implementation details?
9. Does the documentation still describe the actual workflow?

The best Boxify contributions improve the framework rather than solving only one particular enclosure.

---

# 🚀 Adding New Features

When adding a new feature, consider the following workflow:

```text id="w4j5nm"
Define user intent
       │
       ▼
Determine configuration
       │
       ▼
Identify responsible component
       │
       ▼
Implement parametric geometry
       │
       ▼
Add dynamic behavior if required
       │
       ▼
Test configuration variants
       │
       ▼
Verify Assembly
       │
       ▼
Update documentation
```

This keeps new functionality aligned with the Boxify architecture.

---

# 📦 Releases and Versions

Boxify uses version numbers to communicate significant changes to the framework.

When a release is prepared:

- Update the `CHANGELOG.md`.
- Verify the documentation.
- Verify the relevant Onshape Version.
- Ensure the repository represents the intended release state.

The Git repository, documentation and Onshape model should describe the same functional version.

---

# 📜 Code of Conduct

Please be respectful and constructive when contributing.

Focus discussions on:

- The design
- The architecture
- The implementation
- The user experience

Different approaches are welcome, provided they are discussed constructively.

---

# 🙏 Thank You

Every contribution helps make Boxify more useful, reusable and easier to maintain.

Whether you add a new library definition, improve the parametric architecture, fix a reference, improve documentation or simply report a problem, your contribution is appreciated.

**Happy Boxifying! 🚀**
