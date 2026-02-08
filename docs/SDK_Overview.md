# SDK Overview

Welcome to the CalSci Software Development Kit (SDK) documentation.

This chapter gives you the mental model needed to build apps on CalSci confidently before moving into implementation chapters.

---

## 1) What is CalSci?

CalSci is a MicroPython-based runtime for a programmable scientific calculator platform (ESP32-S3 + keypad + LCD).  
It provides a structured app system with:

- A launcher/runtime loop
- Shared global runtime objects
- UI buffers for menu/form/text experiences
- Display uploaders that render buffers to the LCD
- App routing and navigation state handling

In short: CalSci gives you a lightweight app framework tailored for constrained embedded hardware.

---

## 2) What is the SDK?

In this documentation, “SDK” means the **developer-facing APIs and patterns** already available in the CalSci codebase for creating apps.

The SDK primarily consists of:

- Shared objects exported through `data_modules.object_handler`
- Buffer modules in `process_modules/*buffer*.py`
- Input and typing flow (`typer`, keypad mapping)
- App navigation/routing (`App`, launcher, navbar states)
- Data/config patterns (`db/*.json`)
- Optional platform services (networking, app download/install)

---

## 3) Who this SDK is for

This SDK is written for:

- Developers building new calculator apps
- Contributors maintaining built-in apps
- Makers integrating sensors or device features into UI apps

You should be comfortable with:

- Python / MicroPython basics
- Event-loop style programming
- Basic embedded constraints (memory, latency, and display limits)

---

## 4) Core runtime model (high-level)

CalSci apps typically follow this loop:

1. **Read input** (`typer.start_typing()` / keypad)
2. **Update buffer state** (`menu`, `form`, or `text`)
3. **Render frame** with corresponding uploader (`menu_refresh`, `form_refresh`, `text_refresh`)
4. **Route/exit** by updating `app` object when navigation changes app context

This consistent model is what makes different apps feel uniform on-device.

---

## 5) SDK design goals

The CalSci SDK is built around a few practical goals:

- **Consistency**: all apps use similar loop and render patterns
- **Simplicity**: shared objects reduce app boilerplate
- **Low-resource operation**: buffer + uploader split keeps rendering focused
- **Modularity**: app groups and app names allow dynamic loading
- **Extensibility**: new app modules can be integrated with minimal runtime changes

---

## 6) Main SDK building blocks

### 6.1 Object handler exports
`data_modules.object_handler` wires and exposes commonly used globals, such as:

- `menu`, `form`, `text`
- `menu_refresh`, `form_refresh`, `text_refresh`
- `typer`
- `nav`
- `app`

This is the practical “import surface” for most apps.

### 6.2 Buffers
Buffers store UI state:

- **Menu buffer**: list navigation + cursor/viewport
- **Form buffer**: label/input editing flows
- **Text buffer**: text/cursor editing and command handling

### 6.3 Uploaders
Uploaders convert buffer state into actual LCD writes.  
Apps do not generally render pixels directly; they update buffers and call the appropriate uploader.

### 6.4 Navigation state
Navbar state (`default`, `alpha`, `beta`, `ALPHA`) reflects keypad mode and is passed into refresh calls for consistent UI feedback.

### 6.5 Routing
The `App` object holds current/next app context used by runtime app loading logic.

---

## 7) App categories you’ll see in the repo

The app tree is organized into groups such as:

- `apps/root` (entry and core menus)
- `apps/scientific_calculator` (math tools, matrix ops, equations)
- `apps/settings` (device/runtime settings)
- `apps/installed_apps` (downloaded or add-on apps)

The same SDK pattern is reused across these groups.

---

## 8) Documentation map (what to read next)

After this overview, continue in this order:

1. **Runtime Architecture**  
   Understand lifecycle, launcher flow, and dynamic app loading.

2. **Getting Started**  
   Set up environment and build your first app.

3. **Core App APIs**  
   Learn object exports, input handling, and navigation/routing.

4. **UI Buffer System**  
   Master menu/form/text buffers and uploaders.

5. **App Development Patterns + Recipes**  
   Build production-style apps faster.

---

## 9) Best practices from day one

- Prefer SDK shared objects over ad-hoc globals.
- Keep app loops deterministic (input → update → render).
- Use the correct buffer for the UX type (menu vs form vs text).
- Keep display refresh focused; avoid unnecessary redraws.
- Validate form/text input before calling compute logic.
- Keep JSON-backed data schemas stable and documented.

---

## 10) Summary

CalSci SDK is a practical embedded app framework built around a consistent loop, shared runtime objects, and buffer-driven UI rendering.

If you understand the model in this chapter, the rest of the SDK docs become implementation detail.
