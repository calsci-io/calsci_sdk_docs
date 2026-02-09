# CalSci SDK

Build apps for CalSci, a MicroPython-based scientific calculator runtime for ESP32-S3 devices.

---

## What is this SDK documentation?

This documentation is a developer guide for writing, integrating, and maintaining CalSci apps.

It explains:

- How CalSci apps are structured and launched.
- Which runtime modules and shared objects app developers should use.
- How to build menu-driven, form-driven, and text-driven applications.
- How to work with settings, data files, networking features, and installed apps.

---

## Who should read this?

- **App developers** creating new calculator features or utilities.
- **Contributors** maintaining built-in apps and runtime modules.
- **Makers/experimenters** adding sensor-driven tools and custom workflows.

---

## What you will learn

By following this SDK, you will be able to:

1. Understand the CalSci app lifecycle (`main` → app handler → app runner).
2. Use shared runtime objects from `data_modules.object_handler`.
3. Handle keypad input with `typer` and navigation states with `nav`.
4. Build robust UI flows using:
   - `Menu` + `menu_refresh`
   - `Form` + `form_refresh`
   - `Textbuffer` + `text_refresh`
5. Route between apps safely with the `App` object.

---

## SDK chapter map

This documentation is organized as separate chapters so you can read it progressively:

- **SDK Overview**: core concepts and architecture.
- **Getting Started**: setup and first app.
- **Core App APIs**: runtime exports and input/navigation APIs.
- **UI Buffer System**: buffer models, uploaders, and navbar behavior.
- **App Development Patterns**: reusable patterns used across built-in apps.
- **Platform Services**: db/config, networking, and app installation.
- **Reference & Recipes**: quick lookup and practical implementation recipes.

---

## Recommended reading path

If you are new to CalSci app development:

1. Start with **SDK Overview**.
2. Complete **Getting Started**.
3. Read **Core App APIs** and **UI Buffer System**.
4. Use **App Development Patterns** and **Recipes** while building your app.

---

## Conventions used in this documentation

- File paths are repo-relative.
- Code snippets are designed for MicroPython runtime usage in this codebase.
- “Buffer” means in-memory UI state model; “Uploader” means LCD render bridge.

---

## Next step

Continue to the **SDK Overview** chapter to understand how the runtime components connect and where your app code fits in the execution loop.
