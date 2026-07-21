# Design Instruction: [Feature Name]
**Source spec:** [write-spec-2 document name]
**Figma file:** [link or "new file"]
**Date:** [DD/MM/YYYY]

---

## Overview
[1-2 sentences describing what screens this covers and the user flow]

---

## Design System Components to Use

| Component | Design System Name | Used for |
|-----------|-------------------|----------|
| | | |

## New Components to Create

| Component | Description | Priority |
|-----------|-------------|----------|
| | | |

---

## Screen 1: [Screen Name]

### Frame Setup
- **Frame name:** [name]
- **Dimensions:** 1440 × [height]px
- **Background:** [color token]
- **Layout:** Vertical auto layout

### Components

#### [Field Name]
- **Component:** [DS name or "New: description"]
- **Label:** "[text]"
- **Required:** Yes / No
- **Default:** [value or empty]
- **Width:** Full width

### States

| State | Trigger | Visual |
|-------|---------|--------|
| Default | On load | |
| Error | Failed save | Red border + "[error message]" |
| Disabled | [condition] | Grayed out |
| Loading | Saving | Button spinner |
| Success | Save OK | Toast "[success message]" |

### Layout
- Outer padding: [N]px
- Section gap: [N]px
- Field gap: [N]px
- Button position: [location]

### Interactions

| Element | Trigger | Action |
|---------|---------|--------|
| [Button] | Click | [behavior] |

---

## Figma MCP Prompt

```
Create a Figma design for [Feature Name].

SCREEN 1: [Screen Name]
Frame: 1440 × [N]px, vertical auto layout, background [color]

Components:
- "[Label]": Input field, [required/optional], placeholder "[text]", full width
- "[Label]": Dropdown, options: [list], default: "[value]"
- "[Label]": Toggle, default [on/off]
- "[Label]": Date range picker, required
- Primary button "Lưu" — bottom right
- Secondary button "[text]"

States to design:
1. Default
2. Error — required fields: [list fields + error messages]
3. Disabled — fields: [list]
4. Loading — Lưu shows spinner
5. Success — toast "[message]"

Layout:
- Padding: [N]px all sides
- Section gap: [N]px
- Label above input, [N]px gap

[Repeat for each screen]
```
