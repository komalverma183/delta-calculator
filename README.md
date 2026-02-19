https://github.com/komalverma183/delta-calculator/releases

[![Releases](https://img.shields.io/badge/releases-download-blue?logo=github&style=for-the-badge)](https://github.com/komalverma183/delta-calculator/releases)

# Delta Calculator: Fast Kinematics & Dimensions for Delta 3D Printers

A modern toolset for delta and Kossel style 3D printers. It helps you compute geometry, convert dimensions, and produce ready-to-use configuration snippets for popular firmware like Klipper and Marlin. The project focuses on clear math, precise results, and smooth integration with your printer workflow.

This project is built to be dependable, fast, and easy to use. It covers core delta kinematics, kit-specific geometry, and practical tools to help you tune your machine without guessing. You can run the calculations locally, export configuration blocks, and verify results with sample scenarios.

Images and visuals to illustrate delta geometry accompany this README. See the diagrams below for quick intuition about how the arms, carriages, and effector relate to each other in a delta printer.

Optional hero visuals
- A simple vector-style diagram helps you see the three arms, the carriages on towers, and the effector plate. You can imagine the three towers at 120-degree spacing, with links forming a triangle around the bed.
- A 3D printer icon gives a quick sense of scale and hardware context.

An essential goal of this project is to keep things accessible. The math behind delta geometry can be tricky, but the software strives to present outputs in a way that is easy to reason about. If you are new to delta printers, this README walks you through core concepts, how to use the tool, and practical examples you can apply right away.

Overview of what you get
- Geometry calculator for delta and Kossel printers
- Real-time kinematics tests to sanity-check your numbers
- Exportable configuration blocks for Klipper and Marlin
- Command-line interface (CLI) plus optional library usage in Python or JavaScript
- Clear input validation, error handling, and helpful hints for beginners

Documentation structure
- Quick start for new users
- In-depth math and geometry explanations
- How to integrate with Klipper and Marlin
- Practical examples for common printer setups
- Testing, validation, and troubleshooting guidance
- Documentation of planned improvements and how to contribute

Where this fits in your toolchain
- If you maintain a delta or Kossel printer, delta-calculator helps you set accurate geometry quickly.
- It reduces the guesswork in base radius, arm lengths, and carriage offsets.
- It lets you generate tight, reproducible config blocks for firmware upgrades or tuning sessions.

Important note about the Releases page
- From the Releases page, download the release asset that matches your platform and run it. The release assets provide a standalone executable or installer to help you get started quickly. This is the recommended starting point for new users who want a no-installation experience. The release asset is designed to streamline setup and ensure you have a consistent baseline for your printer geometry.

What you will learn by using delta-calculator
- How the three tower positions affect the effective delta geometry
- How to translate physical measurements into firmware-ready values
- How small changes in base radius or rod length ripple through the kinematic solution
- How to compare different hardware configurations side by side
- How to export clean, copy-paste-ready blocks for Klipper and Marlin

Table of contents
- About this project
- Core concepts
- Features and capabilities
- Quick start
- Installation options
- How to use the CLI
- Geometry primitives and math
- Export formats and examples
- Firmware integration guides
- Practical workflows
- Testing and validation
- Troubleshooting
- Extending delta-calculator
- Roadmap
- Community and contributions
- License

About this project
Delta calculators have two broad roles. They help you design the geometry for a delta or Kossel machine. They also provide a bridge to firmware configuration so you can translate measurements into practical, working printer settings. This project aims to demystify delta geometry with precise calculations and a friendly interface.

Core concepts
- Delta geometry basics: a delta printer uses three towers arranged in an equilateral triangle. Each tower carries a carriage that moves along a vertical axis. The three carriages connect to a single effector through rods. The position of the effector is determined by the distances from each carriage to the effector plane.
- Base radius: the horizontal distance from the center of the print bed to a tower axis. This distance, together with rod length and carriage offsets, constrains the possible positions of the effector.
- Rod length: the length of each connecting rod from a carriage to the effector. This length, combined with tower geometry, determines reachable volumes and the interplay between axis movements.
- Tower offsets: the horizontal positions of each tower along the base plane. In a symmetric delta, these are evenly spaced at 120-degree intervals. Asymmetries can arise in non-ideal builds, and the calculator handles those cases with appropriate checks.
- Kinematics: the mathematical relationship that maps carriage heights to effector position. The solver uses geometry to convert between motor steps, physical motion, and end-effector coordinates.

Features and capabilities
- Geometry solver: computes the three carriage heights given a target end-effector position, or computes the end-effector position given the carriage heights.
- Input validation: checks for physically impossible configurations, like rod lengths shorter than required, or base radii that cause collisions.
- Units support: supports millimeters as the primary unit, with optional conversion utilities for inches if needed.
- Output formats: produces coordinates for the three steppers, recommended stepper positions, and optional delta compensation values.
- Firmware export: generates ready-to-paste blocks for Klipper and Marlin, including delta geometry parameters and basic motion limits.
- Import and export scripts: read geometry from a file and export back to your preferred format.
- CLI-first design with a programmatic API for integrations in Python or JavaScript projects.
- Cross-platform: works on Windows, macOS, and Linux via native binaries, Python, or Docker.

Quick start
- Install the tool in a few minutes using one of the supported pathways.
- Enter your printer’s geometry and see the computed results instantly.
- Export a firmware-ready snippet and paste it into your config.
- Validate the numbers by performing a simulated move and confirming expected behavior.

Installation options
- From source
  - Prerequisites: Python 3.8 or newer, or Node.js 16+ for a JavaScript variant, plus typical build tools.
  - Clone the repository and install dependencies.
  - Run a local library or CLI helper to begin calculations.
- Python package
  - pip install delta-calculator
  - Import the library and call calculator functions in a script.
- Docker image
  - docker run delta-calculator/cli delta-calc --help
  - This option avoids host OS compatibility concerns and provides a clean environment.
- Release assets
  - From the provided Releases page, download the release asset for your platform and execute it. This avoids manual setup and provides a ready-to-run tool.

How to use the CLI
- Basic workflow
  - delta-calc compute --base-radius 120 --rod-length 270 --tower-offsets "0,120,240" --target-x 5 --target-y 10 --target-z 180
  - The command returns carriage heights for each tower and the resulting end-effector coordinates if you specify a target.
- Interactive prompts
  - delta-calc interactive
  - The tool asks for base radius, rod length, tower positions, and desired end-effector coordinates, then prints a full solution.
- Validation mode
  - delta-calc validate --base-radius 120 --rod-length 270 --tower-offsets "0,120,240" --test-positions
  - This mode checks for singularities, collisions, and physically impossible configurations.
- Export formats
  - delta-calc export-klipper --base-radius 120 --rod-length 270 --tower-offsets "0,120,240"
  - delta-calc export-marlin --base-radius 120 --rod-length 270 --tower-offsets "0,120,240"
  - The commands generate copy-paste-ready blocks you can drop into your firmware.

Geometry primitives and math
- Three-rail system model
  - The model has three towers at 120-degree spacing.
  - Each carriage has a vertical axis with a known offset from the bed center.
  - The effector is connected to all three carriages by equal-length rods, forming a spherical intersection condition.
- Coordinate systems
  - Global coordinates: a convenient reference frame aligned with the bed plane.
  - Local coordinates: carriage frames for each tower. The solver converts between frames to solve the inverse problem.
- Inverse kinematics
  - Given the desired end-effector position, compute the three carriage heights.
  - The solver uses geometric relationships and solves a system of equations with numerical stability checks.
- Forward kinematics
  - Given the three carriage heights, determine the end-effector position.
  - Useful for simulating moves and verifying consistency with the intended path.
- Numerical stability
  - The solver uses robust methods to avoid division by near-zero terms.
  - Checks for singular configurations where small input changes yield large output changes.
- Tolerances and precision
  - The default precision is set to 0.01 mm for practical printing accuracy.
  - You can tune tolerances in the configuration to match your hardware and calibration goals.

Export formats and examples
- Klipper export
  - Klipper uses a delta geometry section and a configurable bed level compensation approach.
  - The export block includes delta_radius, rod_length, tower_offsets, and motion limits.
  - Example (pseudo):
    [stepper_x]
    type: delta
    radius: 120.0
    rod_length: 270.0
    tower_offsets: [0, 120, 240]
- Marlin export
  - Marlin expects delta geometry in its own configuration style, with careful alignment to the physical axes and steps per millimeter.
  - The export includes:
    - DELTA_RADIUS
    - DELTA_ROD
    - DELTA_DIAGONAL_ROD
    - DELTA_HOME_OFFSETS
  - Example (pseudo):
    # Delta geometry for Marlin
    # DELTA_RADIUS = 120
    # DELTA_DIAGONAL_ROD = 270
    # DELTA_SECTOR = 120
- Validation and sensitive checks
  - The exported blocks come with notes about safe operating ranges.
  - The calculator warns if the geometry approaches a near-singular configuration.

Practical workflows
- Start from a known printer
  - If you have an existing delta printer, measure base radius, carriage offsets, and rod length.
  - Input those into the calculator to confirm the geometry.
  - Use the export feature to generate a firmware snippet for Klipper or Marlin.
- Tune and re-calibrate
  - After updating firmware or mechanical parts, re-run the calculations.
  - Compare the predicted movements with measured test prints to refine geometry.
  - Use the validation mode to catch potential issues early.
- Multi-configuration exploration
  - Model several geometry variants in the same project.
  - Save configurations as JSON files and compare results side-by-side.
  - Choose the configuration that minimizes error in simulated paths and matches printing goals.
- Calibration sequence
  - Stage 1: measure the actual base radius and tower offsets on your machine.
  - Stage 2: input the measurements and compute the ideal rod length and offsets.
  - Stage 3: adjust the firmware and physically tweak the hardware to align with the computed geometry.
  - Stage 4: print a calibration test cube or a tower test to validate the changes.

Firmware integration guides
- Klipper
  - Step-by-step integration
    1) Export the delta geometry block from the calculator.
    2) Paste the block into the Klipper configuration where the delta printer is defined.
    3) Verify the resulting coordinates with a short print test or a motion test.
    4) Calibrate with standard delta test files to confirm the path alignment.
  - Common gotchas
    - Make sure the bed and gantry are square before running tests.
    - Check the stepper directions to ensure the expected physical movement matches the coordinate output.
- Marlin
  - Steps to integrate
    1) Use the exported delta geometry values to populate the DELTA constants.
    2) Update your configuration to reflect any hardware changes, such as new tower offsets.
    3) Run end-to-end tests, including homing and a small test move.
  - Common issues
    - Off-center homing and noticeable skew in the first few layers.
    - Inconsistent end-effector heights during diagonal moves.
- Compatibility tips
  - Keep firmware and hardware in sync. A small mismatch in rod length or radius yields noticeable artifacts.
  - Use the calculator as your single source of geometry changes to avoid drift between firmware updates and hardware.

Practical examples
- Example A: Symmetric delta with standard hardware
  - Base radius: 120 mm
  - Rod length: 270 mm
  - Tower offsets: 0, 120, 240
  - Target: a flat plane at Z = 180 mm
  - Result: carriage heights, end-effector position, and a ready-to-paste export for Klipper
- Example B: Slight asymmetry to accommodate a custom frame
  - Base radius: 125 mm
  - Rod length: 268 mm
  - Tower offsets: 0, 118, 238
  - Target: Z = 190 mm with a gentle tilt in Y
  - Result: new carriage heights and export blocks
- Example C: Quick iteration cycle
  - Input a few dozen variations of radius, rod length, and offsets
  - Use the side-by-side comparison to identify configurations with the smallest overall error across a sample test path

Testing and validation
- Unit tests
  - Validate the math primitives with known test cases.
  - Confirm inverse and forward kinematics are consistent.
- Integration tests
  - Run export blocks through a parser that checks syntax for Klipper and Marlin.
  - Validate that the generated blocks produce plausible values when loaded.
- Visual validation
  - Use a simple 2D projection to ensure the three rods intersect in a physically plausible way for the chosen end-effector point.
  - Optional: render a quick diagram with the computed geometry to spot obvious issues.
- Performance
  - The solver is designed to be fast, with results produced in milliseconds for common configurations.
  - The library scales well for batch processing of multiple configurations.

Troubleshooting guide
- If the solver reports a singular configuration
  - Check that the base radius and offsets form a valid triangle in the horizontal plane.
  - Verify rod lengths are sufficient to connect all three carriages to the intended end-effector heights.
- If outputs do not match measured moves
  - Re-check measurements of tower positions, base radius, and vertical offsets.
  - Re-run the solver with revised inputs and compare the differences.
- If firmware exports fail to load
  - Confirm that you pasted the block into the correct section of the firmware configuration.
  - Ensure you are using the correct export type for your firmware.
  - Validate that the firmware version supports the parameters you provided.
- If the end effector tilts or wobbles during moves
  - The problem often points to asymmetrical geometry or a mismatch between physical hardware and the computed geometry.
  - Use the calculator to adjust offsets or rod lengths to restore symmetry and reduce tilt.

Extending delta-calculator
- Plugin architecture
  - The calculator supports extensions for new kinematic models and additional export formats.
  - You can implement a plugin to add alternate geometry definitions or to support a new firmware.
- Language bindings
  - Python binding provides a friendly API for integration in data pipelines or embedded dashboards.
  - JavaScript binding suits web-based frontends and Node.js automation scripts.
- Data persistence
  - Store configurations as JSON with a schema that tracks geometry, firmware, and version metadata.
  - Use versioning to keep a history of geometry changes and firmware exports.

Roadmap
- Improve user interface
  - Add a lightweight GUI for common tasks.
  - Visualize a live delta model with interactive adjustments to base radius, rod length, and tower offsets.
- Expand firmware coverage
  - Add export templates for more firmware variants.
  - Improve validation with firmware-specific constraints and recommended safe limits.
- Calibration workflow enhancements
  - Provide guided calibration steps with on-screen prompts.
  - Integrate with test print routines to automate measurements and updates.
- Community features
  - Implement a plug-in ecosystem for user-contributed geometry models.
  - Create a gallery of common delta configurations with community feedback.

Community and contributions
- How to contribute
  - Start by opening a GitHub issue to propose a feature or report a bug.
  - Submit a pull request with a clear, small change set.
  - Include tests and documentation updates with each PR.
- Style and quality
  - Follow the project’s code style guidelines.
  - Keep functions small and well-documented.
  - Provide unit tests for new features or fixes.
- Codes of conduct
  - Be respectful and constructive in all interactions.
  - Respect others’ time and expertise.
- Documentation standards
  - Update README and docs with every feature change.
  - Include practical examples and clear usage notes.

License
- Delta Calculator is released under the MIT license.
- The license permits reuse, modification, and distribution with attribution.
- For corporate use, ensure compliance with any internal policies and terms of service.

Appendix: quick references
- Terminal keyboard shortcuts
  - Ctrl+C to cancel a running calculation
  - Ctrl+D to exit interactive mode
- Common parameters
  - base_radius: horizontal distance from bed center to each tower axis
  - rod_length: length of the connecting rod from carriage to the effector
  - tower_offsets: three numeric values describing tower positions in degrees or coordinates
  - target coordinates: x, y, z for desired end-effector position
- Typical export blocks
  - Klipper
  - Marlin
- Unit handling
  - Default unit is millimeters
  - You can convert to inches with a built-in converter if needed

Notes on structure and usage
- The project emphasizes clarity and correctness. Every feature has a rationale rooted in delta geometry.
- The CLI is designed to be approachable for beginners while still powerful for advanced users.
- The conversions to firmware blocks are designed to minimize manual editing after export.
- The documentation uses practical numbers and common printer configurations to help users learn by example.

Acknowledgments
- The delta-calculator project benefits from the open-source community of delta printer builders.
- Thanks to early users who provided feedback and test data that guided improvements.

Community links
- Releases page: https://github.com/komalverma183/delta-calculator/releases
- Main project repository: https://github.com/komalverma183/delta-calculator
- If you want to see how the tool fits into your workflow, check the Releases page for downloadable assets and examples.

Notes on design philosophy
- The goal is to keep geometry understandable without sacrificing precision.
- The design favors explicit math and transparent outcomes over black-box heuristics.
- The tool is built to be reliable in real-world calibration and ongoing tuning.
- It supports a range of printer configurations, from simple to complex delta variants.

End of documentation; you now have a solid, comprehensive guide to delta-calculator.