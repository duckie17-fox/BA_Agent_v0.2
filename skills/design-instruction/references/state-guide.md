# State Derivation Guide

## How to derive design states from spec constraints

Every state in the design must trace back to a constraint in the spec.
Do not invent states — derive them. Do not miss states — check every constraint.

---

## Field-level states

### From "Trường bắt buộc nhập/chọn"
→ **Error state required**
- Red/danger border color
- Error message below field: exact text from spec
- Example: "Vui lòng nhập Mã chương trình"

### From "Mặc định chọn [value]" or "Default: [value]"
→ **Default/pre-filled state**
- Component renders with value already selected
- Example: Dropdown showing "Bản nháp" on load

### From "Trường disable" or "disable khi [condition]"
→ **Disabled state**
- Grayed out, no cursor interaction
- Must show clearly not editable
- If conditional disable: also need **enabled state** as contrast

### From "Chỉ được nhập tối đa [N] ký tự"
→ **Character limit state**
- Counter "[current]/[max]" below or inside field
- At limit: counter turns red or warning color

### From "Mã chương trình đã tồn tại" or any duplicate error
→ **Async validation error state**
- Triggered after server response, not on blur
- Different visual from client-side required error (can be same style, note timing)

### From "Khi chọn vào, khung được viền đậm"
→ **Focus state**
- Border becomes darker/primary color on focus
- Standard — include for all inputs

---

## Screen-level states

### Always include for every screen:
| State | When |
|-------|------|
| Default | On first load |
| Loading | During async operations (save, fetch) |
| Error (form) | After failed save attempt |
| Success | After successful save |
| Empty | If list screen with no data |

### Derive from edge cases section:
| Edge case | State to design |
|-----------|----------------|
| Session timeout | Session expired modal/overlay |
| Network error | Error toast or inline message |
| Concurrent edit | Conflict warning state |
| No permission | Disabled/locked screen state |

### Derive from button conditions:
| Spec condition | State to design |
|----------------|----------------|
| "Button ẩn khi [condition]" | Two frame variants: with button / without button |
| "Button disable khi [condition]" | Disabled button state within same frame |
| "Hiển thị popup xác nhận" | Confirmation modal/overlay design |

---

## Popup/Modal states

Every popup mentioned in spec needs:
1. Default open state
2. Loading state (if async action)
3. Error state (if action can fail)
4. Closed state (implied — just the parent screen)

---

## State naming convention for Figma

Use consistent naming so Figma MCP creates properly named variants:

| State | Figma variant name |
|-------|--------------------|
| Default | state=default |
| Focus | state=focused |
| Filled | state=filled |
| Error | state=error |
| Disabled | state=disabled |
| Loading | state=loading |
| Success | state=success |
| Empty | state=empty |

When a component has multiple variant dimensions (e.g., size + state):
`size=large, state=error`

---

## Component variants from dropdown options

If a spec field has N options, the design needs:
- 1 default/closed state (showing default option)
- 1 open/expanded state (showing dropdown list)
- 1 selected state (showing chosen option)
- 1 disabled state (if spec says disable condition)

List all options in the design instruction so Figma MCP can populate the dropdown items.
