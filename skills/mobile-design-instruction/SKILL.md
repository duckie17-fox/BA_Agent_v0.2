---
name: mobile-design-instruction
description: >
  Converts an approved functional spec (write-spec output) into mobile screens drawn directly
  in Figma via figma-mcp-go, using Mobile design system tokens and visual patterns (393px viewport).
  Use when ready to hand off to design after spec is HITL-approved, or when someone says
  "tạo mobile design", "vẽ màn hình mobile lên Figma", "mobile design instruction",
  "convert spec to mobile Figma", hoặc "chuẩn bị design mobile cho Figma".
  Do NOT use before write-spec is approved — design instruction requires confirmed spec.
---

# Mobile Design Instruction Skill

## Handoff Input

**Receives from:** `write-spec` (HITL approved) + `write-prd` (khi feature có nhiều màn)
**Expected artifact:** Spec (`artifact: Spec`, `status: Approved`)
**Required fields:** Mermaid diagram (có happy path + ít nhất 1 error path), Bảng mô tả trường (đủ 4 cột), Data Model, Edge Cases
**Hard gate:** Nếu `status` ≠ Approved → từ chối, yêu cầu BA hoàn thành HITL trước. Nếu thiếu Mermaid diagram hoặc field table → dừng, yêu cầu bổ sung.
**Navigation gate:** Nếu feature có từ 2 màn trở lên → bắt buộc đọc PRD Section 4 (Screen Map & Navigation) trước khi vẽ. Nếu PRD chưa có Section 4 → dừng, yêu cầu BA bổ sung trước.

## Output

Screens drawn live trong Figma, grouped by user story (screenshot-ready). Sau khi hoàn thành:
1. Spec là `04-spec--[feature].md` → save log tại `artifacts/[folder]/05-design--[feature].md`
2. Log file ghi: Figma page link + story groups + screens list + trạng thái từng screen
3. Notify: `✓ [N] screens drawn in Figma · [M] story groups · Log: artifacts/[folder]/05-design--[feature].md`

## Position in pipeline

```
write-spec (approved)
        │
        ▼
mobile-design-instruction
  ├── Parse screens from Mermaid diagram
  ├── Map fields → Mobile components
  ├── Derive all states per component
  ├── Load design-tokens + visual reference
  └── Execute figma-mcp-go → screens live in Figma
```

## Input to collect

Extract from spec. Ask only for what's missing.

1. **Spec document** — write-spec output (required): Mermaid diagram, field table, data model, edge cases
2. **PRD** — write-prd output (**bắt buộc khi feature có nhiều màn**): đọc Section 4 (Screen Map & Navigation) để biết navigation pattern và shared shell elements trước khi vẽ màn đầu tiên
3. **User Persona** — extract từ spec/PRD; nếu không có thì derive từ "Người dùng bị ảnh hưởng" trong qualified ticket:
   - **Role**: job title / role trong hệ thống (e.g., Salesman, Brand Manager, Nhà phân phối)
   - **Goal**: họ đang cố làm gì khi dùng tính năng này?
   - **Context**: mobile field work, in-store, offline usage?
   - **Pain point**: vấn đề gì dẫn đến tính năng này?
   - Nếu nhiều persona → liệt kê hết, đánh dấu **primary**
4. **Figma file link** — nếu thêm vào file có sẵn (optional)

> Platform mặc định: **iOS 393px viewport** (iPhone 14/15). Android layouts tương tự nhưng sử dụng native nav bar thay home indicator nếu cần.

---

## Core extraction logic

| Spec element | Mobile design element |
|---|---|
| Mermaid diagram | Screen list + user flow |
| Field table — Textbox | Input Field 56px + states |
| Field table — Dropdown | Input Field + trailing chevron icon |
| Field table — Search | Search Field 56px |
| Field table — Price | Price Input (accent `#026483`) |
| Field table — Textarea | Textarea Field 142px+ |
| Field table — Switch | Toggle (48×32) |
| Field table — Checkbox | Checkbox 32×32 (radius 4px) |
| Field table — Radio | Radio Button (Large 32px / Medium 24px) |
| Field table — Quantity | Quantity Selector |
| Field table — OTP | OTP Input 56×56px cells |
| Field table — Button | Button (Large 48px / Medium 40px) + states |
| Field table — Text (display) | Text label read-only |
| Field table — Date picker | Date Picker component |
| Validation constraints | Error states per field + error message text |
| Default values | Default state per component |
| Enable/disable conditions | Disabled state per component |
| Edge cases | Empty / loading / timeout states |
| Data model | Data fields needed per component |

---

## Workflow

### Step 1 — Parse screens from Mermaid diagram
Read the Mermaid diagram. Each major node = one screen/frame. List all screens in order.
Popups, bottom sheets, overlays = separate frames.

### Step 1.5 — Pre-flight: Confirm navigation shell

Nếu feature có nhiều màn (PRD Section 4 tồn tại), xác nhận navigation shell trước:

```
Navigation pattern: [App Bar drill-through / Tab Bar / Bottom Sheet flow]
App shell per screen:
  - Status Bar: 48px (system — vẽ fill màu, không vẽ chi tiết icons)
  - App Bar: 104px total (48px status + 48px content + 8px gap), fill [màu theme]
  - Bottom Action Bar: 96–152px (nếu màn có CTA cố định)

Tab Bar (nếu feature dùng tabs):
  - Height: 48px
  - Tab labels: [Tab A | Tab B | Tab C]
  - Active style: stroke #026483, text #026483 Medium

Confirm pattern này trước khi vẽ frame đầu tiên?
```

**Chờ BA confirm.** Nếu navigation pattern sai → cập nhật PRD Section 4 rồi mới vẽ.

---

### Step 1.6 — Confirm screen list + story grouping

Trước khi vẽ bất kỳ frame nào, trình bày cho BA xác nhận:

```
Tìm thấy [N] screens từ spec. Đề xuất tổ chức như sau:

📋 Story: [Tên user story 1]
   → [Screen A — type: list]
   → [Screen B — type: form]
   → [Screen B-err — type: form + validation error]

📋 Story: [Tên user story 2]
   → [Screen C — type: bottom sheet]
   → [Screen D — type: success overlay]

Mỗi story group sẽ là 1 section trong Figma
→ Screenshot 1 section = 1 ảnh attach vào Jira story

Tiếp tục vẽ không?
```

**Chờ BA confirm** trước khi chuyển sang Step 2.
Nếu BA muốn điều chỉnh grouping → update rồi mới vẽ.

**Cách map screen → story:**
- Screens cùng 1 flow (vd: list → detail → action → success) = 1 story
- Bottom sheet xuất phát từ list = thuộc story của list
- State khác nhau của cùng 1 screen (default, error, empty) = cùng story

---

### Step 2 — Token declaration (hợp đồng thiết kế)

**Trước khi wireframe hay Figma** — khai báo đầy đủ spec cho **từng component** sẽ xuất hiện trong screen:

| Component | Variant / State | Fill | Border | Text | h / w | Padding | Radius |
|---|---|---|---|---|---|---|---|
| Button (Primary Large) | Default | `#013D50` | — | `#FFFFFF` bodyMedium16 | 48px | 24px h | 9999 |
| Button (Primary Large) | Disabled | `#C0C0C0` | — | `#FFFFFF` | 48px | 24px h | 9999 |
| Input Field | Default | `#FFFFFF` | `#DFDFDF` 1pt | placeholder `#A1A1A1` bodyRegular16 | 56px | 16px h | 16px |
| Input Field | Focus | `#FFFFFF` | `#013D50` 1pt | label `#013D50` captionRegular14 | 56px | 16px h | 16px |
| Input Field | Error | `#FFFFFF` | `#D50032` 1pt | error `#D50032` captionRegular14 | 56px | 16px h | 16px |
| _(thêm row cho mỗi component)_ | | | | | | | |

**STOP condition:** Nếu bất kỳ ô nào chưa rõ → quay lại Step 5 đọc `mobile/design-tokens.md`. Không được điền giá trị tự đoán.

---

### Step 2.5 — ASCII wireframe + component map

**Trước khi gọi bất kỳ figma-mcp-go tool nào**, output wireframe đầy đủ của từng màn hình:

```
┌─── 393px ─────────────────────────────────────────┐
│  STATUS BAR · h:48 · fill:#013D50                  │
├────────────────────────────────────────────────────┤
│  APP BAR · h:56 · fill:#013D50                     │
│  [← Back]      [Screen Title headingBold20]   [⋮]  │
├────────────────────────────────────────────────────┤
│  CONTENT · fill:#F5F5F6 · padding:16               │
│                                                    │
│  [Element label]  ← Component: [tên] · [variant]  │
│                     Token: [hex / size / radius]   │
│                                                    │
├────────────────────────────────────────────────────┤
│  BOTTOM ACTION BAR · h:96 · fill:#FFFFFF           │
│  [pad:8t / 40b / 16h]                              │
│  [Button Primary 48px full-width fill:#013D50]     │
└────────────────────────────────────────────────────┘
```

Rules:
- Mọi interactive element → tên component + variant (e.g., `Input Field focus · stroke #013D50 · h 56px · radius 16px`)
- Mọi text node → typography token (e.g., `bodyRegular16 #333333`)
- Mọi màu → exact hex từ `mobile/design-tokens.md`
- Mọi spacing → bội số 4px (standard: 8/16/24px)
- Mọi radius → từ scale: 4 / 12 / 16 / 24 / 9999px

**Chờ BA confirm wireframe trước khi chạy Figma.** Sửa ở đây rẻ hơn nhiều so với sửa trong Figma.

**Pipeline mode (Override HITL):** Không chờ confirm từ BA. Lưu wireframe ASCII vào file log (05-design--[feature].md, section `## Wireframes`) TRƯỚC khi gọi bất kỳ figma-mcp-go tool nào.

---

### Step 3 — Map fields to components

Cho mỗi screen, map từng field sang mobile component type:
- Textbox → Input Field (radius 16px, 56px height)
- Price → Price Input (accent `#026483`, Bold)
- Dropdown → Input Field + trailing `keyboard_arrow_right` icon
- Search → Search Field (56px large / 48px medium)
- Switch → Toggle (Large 48×32px / Medium 40×24px)
- Checkbox → Checkbox 32×32px (radius 4px — unique)
- Radio → Radio Button (Large 32px / Medium 24px)
- Quantity → Quantity Selector (Large 40px / Medium 32px)
- OTP → OTP cells 56×56px grid
- Numeric display → bodyBold16 right-aligned
- Status pill → Tagline (radius 9999, semantic fill)
- Loading → Loading overlay với brand spinner

### Step 4 — Identify all states

Với mỗi component, derive states từ constraints trong spec:
- Has validation → error state (stroke `#D50032` + error message 14px `#D50032` dưới field)
- Has default value → default state defined
- Has disable condition → disabled state (fill `#F5F5F6`, stroke `#DFDFDF`, text `#C0C0C0`)
- Edge case: empty → dùng component `Empty State Illustration` từ local design system (xem Mục 6.7)
- Edge case: loading → brand spinner overlay
- Error / retry → "Đã có lỗi xảy ra" + "Thử lại" link `#007BFF`

### Step 5 — Load Mobile design reference

**Luôn đọc trước khi vẽ:**

| File | Dùng để |
|---|---|
| `mobile/design-tokens.md` | Tokens đầy đủ — màu, typography, spacing, radius, tất cả component specs + states |

> **Không có file reference thêm cho mobile.** Tất cả visual spec đã trong `mobile/design-tokens.md`.
> Nếu cần icon: dùng Material Icons (Google Fonts) — các icon name thường gặp: `chevron_left` (back button), `search`, `close`, `add`, `remove`, `check`, `keyboard_arrow_right`, `keyboard_arrow_down`, `more_vert`, `filter_list`, `qr_code_scanner`, `home`, `receipt_long`, `storefront`, `person`.
>
> **Local design system components** (dùng `get_local_components` → `clone_node` → `reparent_nodes`):
> - Empty State Illustration: componentSet ID `183:13360` — 17 variants (Default: `183:13473`). Dùng thay vì tự vẽ illustration.
> - Onboarding Illustration: componentSet ID `183:12160`
> - Status Illustration: componentSet ID `183:16112` (Pending / Fail / Successful)

---

### Step 6 — Vẽ màn hình lên Figma (figma-mcp-go)

#### 6.1 — Chuẩn bị Figma file

```
1. get_document          → hiểu state file hiện tại
2. get_pages             → kiểm tra page đã có chưa
3. navigate_to_page      → đi đến page đang làm việc
   HOẶC add_page         → tạo page mới nếu cần
                            (đặt tên theo feature: vd "Chụp ảnh trưng bày")
```

#### 6.1b — Tổ chức Figma theo user story (screenshot-ready)

```
Page: "Chụp ảnh trưng bày"
│
├── [Section] "Story: Xem danh sách lượt chụp"
│   ├── Frame "Danh sách — Default"
│   └── Frame "Danh sách — Empty"
│
├── [Section] "Story: Thực hiện chụp ảnh"
│   ├── Frame "Camera — Viewfinder"
│   ├── Frame "Camera — Preview"
│   └── Frame "Camera — Upload success"
│
└── [Section] "Story: Xem chi tiết lượt chụp"
    └── Frame "Chi tiết — Default"
```

**Tạo section frame:**
```
create_frame  name="Story: [Tên]"  fills=transparent  stroke=none
→ width = (số screens × 393) + (gap × (n-1))   height = auto (minimum 852)
→ Frames xếp ngang, gap 48px giữa các screen
```

**Đặt tên frame chuẩn:** `[Tên màn hình] — [State]`
- Default: `Danh sách lượt chụp — Default`
- Error: `Form nhập — Validation error`
- Empty: `Danh sách — Empty`
- Loading: `Danh sách — Loading`
- Bottom Sheet: `Bottom Sheet — Chọn kế hoạch`

#### 6.2 — Naming conventions

| Element | Convention | Ví dụ |
|---|---|---|
| Page | Feature name (Vietnamese) | `Chụp ảnh trưng bày` |
| Frame (screen) | Screen name — State | `Danh sách lượt chụp — Default` |
| Frame (bottom sheet) | `Bottom Sheet / [Name]` | `Bottom Sheet / Chọn kế hoạch` |
| Frame (popup) | `Popup / [Name]` | `Popup / Xác nhận xóa` |
| Layer group | Component role | `App Bar`, `Content`, `Bottom Action Bar` |
| Component | Semantic name | `Button / Primary`, `Input Field / Focus`, `Tagline / Success` |

#### 6.3 — Mobile app shell structure

**RULE: Mọi màn hình chính ĐỀU PHẢI có App Bar.**
Bottom Sheet và Popup vẽ không có App Bar (overlay pattern).

**Full mobile screen** (BẮT BUỘC cho tất cả màn hình chính):
```
Frame 393×852 "Screen Name"   [auto-layout: VERTICAL, gap 0, padding 0]
│
├── Status Bar    [393×48, fill = app bar color hoặc transparent]
│   → Chỉ vẽ background fill, KHÔNG vẽ icons/time (system renders these)
│
├── App Bar       [393×56, fill = themed color, padding 0 16]
│   ├── Back btn  [48×48, icon arrow_back 24px]
│   ├── Title     [headingBold20, centered hoặc left-aligned]
│   └── Actions   [icon buttons 48×48 each, right-aligned]
│
├── Divider       [393×1, fill #DFDFDF]  ← dùng frame 1px thay stroke bottom vì figma-mcp-go không hỗ trợ individual side stroke
├── Tab Bar       [393×56, fill #FFFFFF]  ← VẼ NẾU PRD define tabs; height 56px để chứa icon+label
│   ├── Tab Item  [~98px × 56px, VERTICAL auto-layout, padding top/bottom: 6px]
│   │   ├── Icon  [Material Icons 22px, #A1A1A1 inactive / #026483 active]
│   │   └── Label [Roboto 11px, #A1A1A1 inactive / #026483 Medium active]
│   └── (4 tabs tổng cộng — active tab dùng #026483 cho cả icon lẫn label)
│
├── Content Body  [fill #F5F5F6, VERTICAL layout, padding 16, gap 12]
│   ├── Section Card 1  [fill #FFFFFF, radius 16, padding 16, stroke #DFDFDF 1pt]
│   ├── Section Card 2  ...
│   └── List Items / Form Fields / Charts
│
└── Bottom Action Bar [393×96 or 152, fill #FFFFFF]  ← VẼ NẾU màn có CTA cố định
    [padding top:8, bottom:40, horizontal:16]
    └── Button Primary [fill #013D50, h:48, radius 9999, full width]
```

**App Bar themes:**
| Theme | Status Bar fill | App Bar fill | Title color | Icon color |
|---|---|---|---|---|
| Colored (default) | `#013D50` | `#013D50` | `#FFFFFF` bodyBold20 | `#FFFFFF` |
| White | `#FFFFFF` | `#FFFFFF` | `#333333` bodyBold20 | `#333333` |
| Transparent | transparent | transparent | `#333333` | `#333333` |

**App Bar padding:** Dùng `paddingLeft: 4, paddingRight: 4` (không phải 16) để chevron + title sát trái, trông compact và aligned hơn trên mobile.

**Back button icon:** Dùng Material Icons `chevron_left` (24px), **KHÔNG** dùng text "←". Tạo text node với fontFamily="Material Icons", fontSize=24, đặt vào back button frame 48×48 → tự căn giữa ở (12, 12).

**Bottom Sheet** (không có App Bar):
```
Frame 393×[height] "Bottom Sheet / [Name]"
├── Drag Handle    [73px total: centered bar 56×5px, fill #333333, radius 100]
├── Content        [padding 0 16 16, gap 12]
└── Bottom Action  [padding top:8, bottom:40, horizontal:16]
```

**Popup** (centered overlay):
```
Frame 361×[height] "Popup / [Name]"   [fill #FFFFFF, radius 16, padding 16]
├── Header row     [Status icon 48×48 or Alert icon] + [close 32×32]
├── Title          [bodyBold16 #333333 centered]
├── Description    [bodyRegular16 #333333 centered]
└── Actions        [2 buttons — outline left / primary right, or stacked]
```

#### 6.4 — Execution sequence mỗi screen

**SEQUENTIAL RULE — depth-first:** Hoàn thành TOÀN BỘ 1 screen trước khi tạo frame tiếp theo. Không tạo placeholder frames rỗng. Nếu context window sắp đầy → dừng, ghi log screens đã xong và chưa vẽ, raise lại.

```
Với MỖI screen (xử lý tuần tự):

1. create_frame (393×852)     → main screen frame
2. set_fills (#F5F5F6)        → page background
3. set_auto_layout VERTICAL   → gap 0, padding 0

4. Vẽ STATUS BAR:
   create_frame (393×48, fill = app bar color)
   → reparent vào main frame ngay

5. Vẽ APP BAR:
   create_frame (393×56, fill = app bar color, padding 0 16)
   → add back button (48×48), title text, action icons
   → reparent vào main frame ngay

6. Vẽ TAB BAR (nếu feature dùng tabs):
   a. Tạo Divider frame (393×1, fill #DFDFDF) → reparent vào main frame
      (figma-mcp-go không hỗ trợ single-side stroke → dùng divider rectangle thay thế)
   b. create_frame Tab Bar (393×56, fill #FFFFFF)
      → Với mỗi tab item (4 tabs):
         create_frame (~98×56, VERTICAL auto-layout, padding top:6 bottom:6)
         → create_text icon: fontFamily="Material Icons" fontSize=22, color #A1A1A1 / #026483 active
         → create_text label: Roboto 11px, color #A1A1A1 / #026483 Medium active
         → Icon tạo TRƯỚC label (hoặc dùng reorder_nodes sendToBack sau khi tạo để đưa icon lên đầu)
      → reparent tab bar vào main frame

7. Vẽ CONTENT BODY:
   create_frame (393×auto, fill #F5F5F6)
   → set_auto_layout VERTICAL, padding 16, gap 12
   → Với mỗi card/section:
      create_frame → set_fills #FFFFFF → set_strokes #DFDFDF → set_corner_radius 16
      → add components bên trong → reparent vào content body ngay
   → reparent content body vào main frame

8. Vẽ BOTTOM ACTION BAR (nếu có):
   create_frame (393×96, fill #FFFFFF)
   → padding top:8, bottom:40, horizontal:16
   → add button primary (fill #013D50, h:48, radius 9999)
   → reparent vào main frame ngay

Screen Done Gate — kiểm tra trước khi sang screen tiếp:
□ Status Bar: đúng fill màu
□ App Bar: đúng height 56px, đúng theme, title readable
□ Tab Bar (nếu có): đúng labels, đúng active tab
□ Content: tất cả components theo spec đã vẽ đủ
□ Cards: đều có stroke #DFDFDF, radius 16
□ Inputs: đều có stroke, đúng height (56px standard)
□ Bottom Action Bar (nếu có): đúng padding, button đúng màu
□ KHÔNG có annotation text trong frame
→ Nếu bất kỳ ô nào chưa ✓ → sửa ngay, không sang screen tiếp
```

⛔ **TUYỆT ĐỐI KHÔNG tạo annotation text trong Figma frame.**
Tất cả ghi chú kỹ thuật và state description chỉ được lưu trong file log `05-design--[feature].md`.

#### 6.5 — Design tokens áp dụng khi vẽ

**HARD RULES — stroke bắt buộc (set_strokes):**

| Element | Stroke | Weight |
|---|---|---|
| Card / section | `#DFDFDF` | 1pt |
| Input field (default) | `#DFDFDF` | 1pt |
| Input field (focus) | `#013D50` | 1pt |
| Input field (error) | `#D50032` | 1pt |
| Outlined button | `#013D50` | 1pt |
| Bottom tab bar | `#DFDFDF` | 1pt (bottom only) |
| App bar (white theme) | `#DFDFDF` | 1pt (bottom only) |

**Colors** (từ `mobile/design-tokens.md`):
- Primary: `#013D50` (buttons, focus, App Bar colored theme)
- Primary 2: `#026483` (tab active, chip active, price input)
- Secondary: `#EF4429` (alert, destructive icon)
- Page bg: `#F5F5F6`
- Card bg: `#FFFFFF`
- Text primary: `#333333`
- Text secondary: `#A1A1A1`
- Text disabled: `#C0C0C0`
- Border default: `#DFDFDF`
- Info link: `#007BFF`
- Error: `#D50032`

**Typography** (Roboto — mọi text trong app đều dùng Roboto):
| Token | Size | Weight | Line-height |
|---|---|---|---|
| `headingBold20` | 20px | 700 | 28px |
| `headingBold18` | 18px | 700 | 24px |
| `bodyBold16` | 16px | 700 | 24px |
| `bodyMedium16` | 16px | 500 | 24px |
| `bodyRegular16` | 16px | 400 | 24px |
| `captionRegular14` | 14px | 400 | 18px |
| `labelRegular12` | 12px | 400 | 16px |

**Spacing** (4px grid, standard = 16px):
- Card padding: `16px`
- Page margin: `16px`
- Button horizontal padding: `24px`
- Component gap: `8–12px`
- Bottom Action Bar: top `8px`, bottom `40px`, horizontal `16px`

**Border Radius**:
- Standard (inputs, cards, popups, banners): `16px`
- Pill (buttons, chips, radio, toggle, badges): `9999px`
- Checkbox (unique): `4px`
- Drag handle / home indicator: `100px`

**⚠️ Lỗi phổ biến với màu rgba trong Figma API:**
```
SAI:  set_fills(color="rgba(0,0,0,0.4)")   ← API không nhận string rgba
ĐÚNG: set_fills(color="#000000") + set_opacity(40)
```

#### 6.6 — Vietnamese content rules
- Currency: `482,500,000 ₫` (comma thousands, space trước ₫)
- Date: `DD/MM/YYYY` (e.g. `12/04/2026`)
- Button text: Sentence case (`Thêm mới`, `Lưu lại`, `Xác nhận`)
- Error messages: lowercase trừ chữ cái đầu câu
- Status pills: ngắn gọn (`Đang chạy`, `Hoàn thành`, `Từ chối`)

#### 6.7 — Component patterns chuẩn từ design system

**Search Field icon:**
```
→ Tạo text node fontFamily="Material Icons" fontSize=24 text="search" color=#A1A1A1
→ Đặt làm CHILD ĐẦU TIÊN của search field frame (horizontal auto-layout)
→ Nếu tạo sau placeholder text: reorder_nodes(nodeIds=[iconId], order="sendToBack")
   (sendToBack = index 0 = đầu tiên trong horizontal layout = bên trái)
```

**Empty State (màn danh sách):**
```
1. clone_node(nodeId="183:13473")  → Default Empty State Illustration (170×170)
   (Chọn variant phù hợp từ componentSet 183:13360 nếu có variant đặc thù hơn)
2. reparent_nodes(nodeIds=[cloneId], parentId=[emptyStateFrameId])
3. set_auto_layout trên emptyStateFrame: VERTICAL, CENTER/CENTER, gap=16
4. resize_nodes emptyStateFrame: giữ đúng kích thước (không để shrink)
5. reorder_nodes(nodeIds=[illustrationId], order="sendToBack")  → illustration lên trên text
```

**Auto-layout ordering rule (VERTICAL):**
- `sendToBack` = index 0 = TRÊN CÙNG trong layout
- `bringToFront` = index cuối = DƯỚI CÙNG trong layout
- Áp dụng cho: icon trên label trong tab bar, illustration trên text trong empty state

**figma-mcp-go tool — cú pháp đúng:**
```javascript
// save_screenshots — PHẢI dùng items array (KHÔNG dùng nodeId trực tiếp)
save_screenshots(items=[{nodeId, outputPath, scale}])
// → dùng scale: 0.5 để tránh timeout khi export

// set_corner_radius — PHẢI dùng nodeIds array + cornerRadius param
set_corner_radius(nodeIds=["id"], cornerRadius=16)

// reorder_nodes — PHẢI dùng nodeIds array + order string
reorder_nodes(nodeIds=["id"], order="sendToBack")  // hoặc bringToFront, sendBackward, bringForward

// reparent_nodes — PHẢI dùng nodeIds array + parentId string
reparent_nodes(nodeIds=["id"], parentId="targetId")

// move_nodes — nhận absolute canvas coordinates; x offset ổn định, y offset có thể drift
// khi parent auto-layout resize. Nếu y không chính xác → dùng set_auto_layout CENTER thay thế.
```

---

### Step 7 — Screenshot + structured verify

Sau khi vẽ xong tất cả story groups:

**1. Save screenshot gốc — 1 ảnh / 1 story group:**
```
save_screenshots
  nodeId:     [Section frame ID của story group]
  outputPath: artifacts/[feature-folder]/screenshots/[story-slug].png
  scale:      1
```

**2. Đọc ảnh → verify từng row trong bảng Step 2:**

| Component | Property | Expected (Step 2) | Actual | Pass / Fail |
|---|---|---|---|---|
| Button (Primary Large) | height | 48px | ? | ✓ / ✗ |
| Button (Primary Large) | fill | `#013D50` | ? | ✓ / ✗ |
| Button (Primary Large) | radius | 9999px | ? | ✓ / ✗ |
| Input Field | height | 56px | ? | ✓ / ✗ |
| Input Field | border (default) | `#DFDFDF` | ? | ✓ / ✗ |
| Input Field | radius | 16px | ? | ✓ / ✗ |
| App Bar | height | 56px | ? | ✓ / ✗ |
| Status Bar | height | 48px | ? | ✓ / ✗ |
| _(thêm row cho mỗi property trong Step 2)_ | | | | |

- Mọi ✗ → kèm fix instruction cụ thể (tool call nào, set value nào)
- Những gì cần sửa thủ công trong Figma → note riêng

**3. Fix tất cả ✗.**

**4. Sau khi tất cả rows = ✓ — save verify screenshot:**
```
save_screenshots
  nodeId:     [Section frame ID của story group]
  outputPath: artifacts/[feature-folder]/screenshots/verify-[story-slug].png
  scale:      1
```

**5. Xóa ảnh gốc** `[story-slug].png`. Output cuối chỉ giữ `verify-[story-slug].png`.

> ⚠️ Còn ✗ chưa fix → KHÔNG xóa ảnh gốc → fix tiếp → lặp lại từ bước 3.

---

## Quality Control

| Check | Criteria |
|---|---|
| **App Bar present** | Mọi màn hình chính ĐỀU có App Bar 56px — thiếu = fail |
| **Status Bar present** | Mọi màn hình chính ĐỀU có Status Bar 48px — thiếu = fail |
| **Tab bar consistent** | Nếu PRD define tabs: tất cả frames ĐỀU có tab bar, đúng labels, đúng active tab |
| **Borders present** | Cards, inputs ĐỀU có stroke #DFDFDF 1pt — thiếu = fail |
| **Bottom Action Bar** | Màn có CTA cố định ĐỀU có BAR với padding-bottom 40px |
| Screen coverage | Every screen trong Mermaid diagram có frame trong Figma |
| Story grouping | Screens grouped theo user story, mỗi group là 1 Section frame |
| Component mapping | Every field trong spec maps to a mobile component type |
| States complete | Required fields có error state; disabled fields có disabled state |
| Empty state | Ít nhất màn hình danh sách có empty state |
| Brand tokens | Primary `#013D50`, secondary `#EF4429`, Roboto, radius 16px standard |
| VN formatting | Số tiền `482,500,000 ₫`, ngày `DD/MM/YYYY` trong tất cả mock data |
| Frame dimensions | Mỗi frame 393px width, auto height (minimum 852px) |

## HITL Checklist

Sau khi vẽ xong trên Figma, BA validate:
- [ ] Tất cả màn hình trong spec flow đều có frame trong Figma?
- [ ] App Bar + Status Bar nhất quán qua tất cả frames?
- [ ] Tab bar (nếu có) nhất quán? Active tab đúng với từng màn?
- [ ] Bottom Action Bar padding-bottom 40px (safe area buffer)?
- [ ] Component names đúng với Mobile design system?
- [ ] Tất cả states bắt buộc đã được vẽ (default, focus, error, disabled)?
- [ ] Brand tokens áp dụng đúng (màu, font Roboto, spacing, radius)?
- [ ] VN formatting (số tiền, ngày tháng) đúng?
- [ ] Component nào cần build từ đầu (custom) đã được flagged rõ?

## v2 — Parity với design-instruction (Control Library, 7-block, nav authority)

- **Deviation model:** đọc `../design-instruction/references/control-type-library.md` — dùng cả phần chung + **§Mobile Controls** (Bottom Tab Bar, Bottom Sheet, Swipe, Pull-to-refresh, FAB, Segmented). Kế thừa Default Behaviour/States, chỉ tả deviation.
- **7-block screen spec** (khớp write-spec v2): Overview (Portal/Nav/**Bottom-tab active**) · Fields `(default)` · Control States · Message Placement (MSG-* + surface) · wireframe/state có **Message Zones legend** · Overlay Context (**Bottom Sheet** Close Triggers × "lưu?") · Section J Phụ thuộc chéo.
- **Nav authority:** Bottom Tab Bar item phải ∈ Menu Item List PRD → ngoài list `MENU_ITEM_GAP`; đổi pattern/shell = `DESIGN_GAP` (hard).
- **Wording:** từ chuẩn hành vi + thêm cử chỉ mobile: **vuốt / kéo / giữ** (control-library §Terminology).
- **Touch target ≥44px** + primary action trong tầm ngón cái.
