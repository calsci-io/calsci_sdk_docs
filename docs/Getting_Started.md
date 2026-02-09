# Getting Started

This chapter helps you go from zero to your first working CalSci app.

---

## 1) Prerequisites

Before building apps, make sure you have:

- A CalSci-compatible hardware setup (ESP32-S3 based target)
- MicroPython firmware environment ready
- This repository available on your system
- Basic Python/MicroPython familiarity

Optional but useful:

- Serial monitor for runtime logs
- A clean way to upload files to the board (`upload.sh` in this repo can help)

---

## 2) Understand the project layout

At a high level, these folders matter most for app development:

- `apps/` → app modules (grouped by category)
- `process_modules/` → runtime and UI buffer logic
- `data_modules/` → shared objects and configuration wiring
- `db/` → JSON-driven app lists and settings
- `lib/` → utility and feature libraries used by apps

You’ll spend most of your time in `apps/` and occasionally `db/`.

---

## 3) Runtime flow you should know first

CalSci app execution model is:

1. `main.py` starts the runtime.
2. App handler loop runs continuously.
3. App runner resolves current app group/name.
4. Target app module is imported and executed.
5. App reads input, updates buffer, refreshes display, and routes if needed.

As an app developer, you mostly implement step 5.

---

## 4) Shared objects used by apps

Most apps rely on objects created in `data_modules.object_handler`, such as:

- `menu`, `form`, `text` (UI state buffers)
- `menu_refresh`, `form_refresh`, `text_refresh` (render uploaders)
- `typer` (keypad input abstraction)
- `nav` (mode/navbar state)
- `app` (app routing control)

These are the building blocks for almost every built-in app.

---

## 5) Minimal app checklist

When creating a new app, follow this checklist:

- [ ] Create your app file under the appropriate `apps/<group>/` folder.
- [ ] Import only the runtime objects you need.
- [ ] Implement a simple loop:
  - read input
  - update buffer
  - refresh display
- [ ] Handle back/exit navigation.
- [ ] Register app in the relevant `db/*.json` app list so it appears in menus.

---

## 6) Your first minimal app (menu-style)

Use this skeleton as a starting point:



    # apps/installed_apps/hello_sdk.py

    from data_modules.object_handler import menu, menu_refresh, typer, nav, app

    def hello_sdk():
        menu.menu_list = ["Hello SDK", "About", "Back"]
        menu.update()
        menu_refresh.refresh(state=nav.current_state())

        while True:
            key = typer.start_typing()

            if key == "nav_u" or key == "nav_d":
                menu.update_buffer(key)
                menu_refresh.refresh(state=nav.current_state())

            elif key == "ok":
                selected = menu.menu_list[menu.menu_cursor]
                if selected == "Back":
                    app.set_group_name("root")
                    app.set_app_name("home")
                    return
                else:
                    menu.menu_list = [selected, "Back"]
                    menu.update()
                    menu_refresh.refresh(state=nav.current_state())

            elif key == "power":
                app.set_group_name("root")
                app.set_app_name("home")
                return

---

## 7) Register the app in menu JSON

To make your app visible, add it to the appropriate menu list JSON in `db/` (for example installed apps list or root list, depending on where you want it surfaced).

General pattern:

- App display name (what user sees)
- Group name (folder under `apps/`)
- App module name (python file name without `.py`)

Make sure names match exactly, otherwise dynamic import will fail.

---

## 8) Input + UI pattern to follow

For stable behavior on-device, stick to this per-frame pattern:

1. `inp = typer.start_typing()`
2. `buffer.update_buffer(inp)` (or custom state update)
3. `*_refresh.refresh(state=nav.current_state())`

Choose buffer by screen type:

- List selection UI → `menu`
- Multi-field input UI → `form`
- Free text/calculation entry UI → `text`

---

## 9) Common mistakes (and fixes)

### App not opening from menu

- Check group/app names in JSON vs file path/module name.
- Ensure function entrypoint name matches runtime expectation if your app runner expects one.

### Screen not updating

- Confirm you are calling the corresponding refresh uploader after updates.

### Keys seem unresponsive

- Verify key tokens in app logic match actual keypad mapping output.

### Back navigation broken

- Always set `app.set_group_name(...)` and `app.set_app_name(...)` before returning.

---

## 10) Development workflow recommendation

Use this cycle:

1. Add or modify one app file.
2. Register/update JSON entry.
3. Deploy to device.
4. Test navigation, input, refresh, and back flow.
5. Repeat in small increments.

Small steps are safer on constrained embedded runtimes.

---

## 11) What to read next

After this chapter:

1. Read **Core App APIs** for exact object/method behavior.
2. Read **UI Buffer System** to master menu/form/text interactions.
3. Use **App Development Patterns** for production-ready structure.

---

## Summary

You now have a practical path to create and register a CalSci app:

- understand runtime flow,
- use shared SDK objects,
- implement the input → buffer → refresh loop,
- register the app in JSON,
- and route safely back to launcher screens.
