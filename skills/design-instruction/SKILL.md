---
name: design-instruction
description: >
  Converts an approved functional spec (write-spec output) into screens drawn directly
  in Figma via figma-mcp-go, using Brand Portal design system tokens and visual patterns.
  Use when ready to hand off to design after spec is HITL-approved, or when someone says
  "tạo design instruction", "vẽ màn hình lên Figma", "design instruction",
  "convert spec to Figma", or "chuẩn bị design cho Figma MCP".
  Do NOT use before write-spec is approved — design instruction requires confirmed spec.
---

# Design Instruction Skill

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
design-instruction
  ├── Parse screens from Mermaid diagram
  ├── Map fields → Brand Portal components
  ├── Derive all states per component
  ├── Load design-tokens + visual reference
  └── Execute figma-mcp-go → screens live in Figma
```

## Input to collect

Extract from spec. Ask only for what's missing.

1. **Spec document** — write-spec output (required): Mermaid diagram, field table, data model, edge cases
2. **PRD** — write-prd output (**bắt buộc khi feature có nhiều màn**): đọc Section 4 (Screen Map & Navigation) để biết navigation pattern và shared shell elements trước khi vẽ màn đầu tiên
3. **User Persona** — extract from spec/PRD nếu có; nếu không có thì derive từ "Người dùng bị ảnh hưởng" trong qualified ticket:
   - **Role**: job title / role trong hệ thống (e.g., Brand Admin, Salesman, Nhà phân phối)
   - **Goal**: họ đang cố làm gì khi dùng tính năng này?
   - **Context**: khi nào / ở đâu họ dùng? (web desktop, mobile, back-office...)
   - **Pain point**: vấn đề gì dẫn đến tính năng này?
   - Nếu nhiều persona → liệt kê hết, đánh dấu **primary**
4. **Screen dimensions** — desktop/tablet/mobile? Default: desktop 1440px
5. **Figma file link** — nếu thêm vào file có sẵn (optional)

## Core extraction logic

| Spec element | Design element |
|---|---|
| Mermaid diagram | Screen list + user flow |
| Field table — Textbox | Input field + states |
| Field table — Dropdown | Select component + options |
| Field table — Switch | Toggle + active/inactive states |
| Field table — Date picker | Date picker + calendar states |
| Field table — Button | Button + show/hide/disable conditions |
| Field table — Text (display) | Text label, read-only style |
| Validation constraints | Error states per field |
| Default values | Default state per component |
| Enable/disable conditions | Disabled state per component |
| Edge cases | Empty / loading / timeout states |
| Data model | Data fields needed per component |

---

## Workflow

### Step 0 — Pre-flight gate (bắt buộc, chạy TRƯỚC Step 1, không được bỏ qua)

Đây là nơi enforce lại `Hard gate` + `Navigation gate` đã khai báo ở mục "Handoff Input" — không chỉ đọc lướt, phải tick thật trước khi làm bất kỳ việc gì khác:

```
□ Spec input có artifact: Spec, status: Approved? → Nếu KHÔNG (thiếu status, status=Draft, hoặc chỉ có mô tả miệng/ticket không phải spec) → STOP, trả lời BA: "Chưa có Spec Approved, không vẽ Figma." KHÔNG được vẽ kiểu "demo/exploration" khi chưa có spec — mọi frame vẽ ra phải trace được về 1 spec Approved cụ thể.
□ Spec có Mermaid diagram (happy path + ≥1 error path) và field table đủ 4 cột? → Nếu thiếu → STOP, yêu cầu bổ sung.
□ Feature có ≥ 2 màn? → Nếu có: PRD Section 4 (Screen Map & Navigation) đã tồn tại và đã đọc chưa? → Nếu chưa có Section 4 → STOP, yêu cầu BA bổ sung PRD trước khi sang Step 1.
```

Nếu cả 3 mục đều ✓ → sang Step 1. Nếu bất kỳ mục nào ✗ → dừng lại tại đây, không gọi bất kỳ tool figma-mcp-go nào.

### Step 1 — Parse screens from Mermaid diagram
Read the Mermaid diagram. Each major node = one screen/frame.
List all screens in order. Include popup/modal as separate frames.

### Step 1.5 — Pre-flight: Confirm navigation shell

Nếu feature có nhiều màn (PRD Section 4 tồn tại), xác nhận navigation shell trước:

```
Navigation pattern: [Tabs / Sidebar sub-nav / Drill-through]
Shared shell elements:
  - [Tab bar: Tab A │ Tab B │ Tab C]
  - [Filter bar persist: TM Program, Date Range, ...]

Mỗi spec sẽ vẽ 1 tab/view. Shell (tab bar + filter bar) xuất hiện ở tất cả frames.
Confirm pattern này trước khi vẽ frame đầu tiên?
```

**Chờ BA confirm.** Nếu navigation pattern sai → cập nhật PRD Section 4 rồi mới vẽ.

⛔ **Hard-stop riêng cho Tab Bar trên Portal:** Nếu `Navigation pattern` xác nhận ở trên = `Tabs` VÀ màn đang vẽ là Portal web admin (không phải SFA mobile app) → **KHÔNG tự vẽ tiếp**. Theo platform rule ở section 6.3, Portal web admin không dùng Tab Bar (dùng Sidebar sub-nav). Phải hỏi lại BA bằng câu hỏi cụ thể: "PRD định nghĩa Tabs cho 1 màn Portal — xác nhận đây có đúng là pattern mong muốn không, hay nên chuyển sang Sidebar sub-nav?" và chờ câu trả lời rõ ràng trước khi vẽ bất kỳ frame nào có Tab Bar.

---

### Step 1.6 — Confirm screen list + story grouping

Trước khi vẽ bất kỳ frame nào, trình bày cho BA xác nhận:

```
Tìm thấy [N] screens từ spec. Đề xuất tổ chức như sau:

📋 Story: [Tên user story 1]
   → [Screen A — type: list]
   → [Screen B — type: form]

📋 Story: [Tên user story 2]
   → [Screen C — type: dialog]
   → [Screen D — type: empty state]

Mỗi story group sẽ là 1 section trong Figma
→ Screenshot 1 section = 1 ảnh attach vào Jira story

Tiếp tục vẽ không?
```

**Chờ BA confirm** trước khi chuyển sang Step 2.
Nếu BA muốn điều chỉnh grouping → update rồi mới vẽ.

**Cách map screen → story:**
- Screens cùng 1 flow (vd: list → create → success) = 1 story
- Dialog xuất phát từ list = thuộc story của list
- State khác nhau của cùng 1 screen (default, error, empty) = cùng story

### Step 2 — Token declaration (hợp đồng thiết kế)

**Trước khi wireframe hay Figma** — khai báo đầy đủ spec cho **từng component** sẽ xuất hiện trong screen. Bảng này là "hợp đồng" mà Step 7 sẽ verify lại.

| Component | Variant / State | Fill | Border | Text | h / w | Padding | Radius |
|---|---|---|---|---|---|---|---|
| Button (Primary) | Default | `#071640` | `#071640` | `#ffffff` | 44px | 16px 16px | 8px |
| Button (Primary) | Disabled | `#E5E7EB` | `#E5E7EB` | `#9CA3AF` | 44px | 16px 16px | 8px |
| Field / Input | Default | `#ffffff` | `#D1D5DB` | placeholder `#9CA3AF` | 44px | 12px 16px | 8px |
| _(thêm row cho mỗi component)_ | | | | | | | |

**STOP condition:** Nếu bất kỳ ô nào chưa rõ → tra cứu trong Component quick-ref (Section 6.6). Nếu vẫn không rõ và file `.claude/skills/design-instruction/brand-portal/design-tokens.md` tồn tại → đọc file đó. Không được điền giá trị tự đoán.

---

### Step 2.5 — ASCII wireframe + component map

**Trước khi gọi bất kỳ figma-mcp-go tool nào**, output wireframe đầy đủ của từng màn hình và declare component cho mọi element.

```
┌──────────────────────────────────────────────────────────────┐
│  SECTION NAME · h:Xpx · bg:#XXXXXX · padding:Xpx             │
│                                                               │
│  [Element label]  ← Component: [tên] · [variant]             │
│                     Token: [hex / size / spacing / radius]    │
└──────────────────────────────────────────────────────────────┘
```

Rules:
- Mọi interactive element → tên component + variant (vd: `Button primary · fill #071640 · h 44px · radius 8px`)
- Mọi text node → typography token (vd: `Body/sm 14px Regular #111827`)
- Mọi màu → exact hex từ design-tokens.md, không tự đặt màu
- Mọi spacing & padding → bội số 4px (4, 8, 12, 16, 20, 24, 32...)
- Mọi radius → từ scale: 0 / 2 / 4 / 8 / 12 / 16 / 24 / 100px

**Chờ BA confirm wireframe trước khi chạy Figma.** Sửa ở đây rẻ hơn nhiều so với sửa trong Figma.

**Pipeline mode (Override HITL):** Không chờ confirm từ BA. Thay vào đó: lưu wireframe ASCII tất cả screens vào `05-design-state.md` (section per-screen `### ASCII Wireframe`) TRƯỚC khi gọi bất kỳ figma-mcp-go tool nào. Wireframe phải có trong state file — không được bỏ qua.

---

### Icon System — áp dụng toàn bộ file, không ngoại lệ

**Mục tiêu: icon PHẢI ra icon thật. Rectangle KHÔNG phải 1 lựa chọn ngang hàng — chỉ là last-resort có điều kiện, và khi xảy ra phải flag rõ.** Không bao giờ dùng emoji, Unicode ký tự, hay text thường.

**Thứ tự ưu tiên fallback (climb từ trên xuống, chỉ hạ tầng khi tầng trên thật sự không khả dụng):**

| # | Tầng | Cách dùng |
|---|---|---|
| 1 | Font "Material Icons" có trong file | `create_text` font="Material Icons", size=20, fill theo context — dùng cho TOÀN BỘ icon trong file |
| 2 | Icon font khác đã có sẵn (`get_fonts`) | Nếu không có "Material Icons" nhưng có "Material Symbols"/"Font Awesome"/"Feather"... → dùng font đó nếu có glyph tương ứng |
| 3 | Component/instance icon đã tồn tại | `search_nodes` / `get_local_components` tìm icon component/instance từ page khác cùng Portal hoặc màn đã vẽ trước → `clone_node` tái sử dụng, KHÔNG tạo rectangle |
| 4 | Asset gốc | `import_image` với asset icon chuẩn nếu user/ticket cung cấp |
| 5 | Rectangle placeholder (LAST-RESORT) | Chỉ khi 1–4 đều không có gì → `create_rectangle` 20×20, fill theo context. **BẮT BUỘC** ghi vào `05-design-state.md` mục "Icon Gaps" (icon nào là rectangle + lý do) và RAISE ba-lead xin asset — KHÔNG coi là "hoàn thành bình thường" |

> ⚠️ Trước khi dùng tầng 5, kiểm tra file Figma đã có page/frame nào khác (cùng Portal/project) dùng thành công Material Icons font chưa — nếu có, gần như chắc chắn font available cho toàn document, không có lý do hợp lệ để rơi vào rectangle. Bằng chứng lỗi cũ: màn "Danh sách Nhãn" rơi rectangle cho phần lớn icon sidebar dù màn khác cùng thời điểm dùng được Material Icons đầy đủ.

**Tên icon Material Icons thường dùng:**
- Navigation: `arrow_back`, `chevron_right`, `expand_more`, `menu`
- Action: `add`, `edit`, `delete`, `save`, `close`, `search`, `filter_list`, `download`, `upload`
- Status: `check_circle`, `error`, `warning`, `info`
- Data: `bar_chart`, `table_chart`, `trending_up`
- UI: `more_vert`, `refresh`, `settings`, `visibility`, `visibility_off`

**Kiểm tra font 1 lần duy nhất** (Step 6.4 đã có) → ghi nhớ kết quả → áp dụng cho TẤT CẢ icons trong file, kể cả ngoài sidebar.

> 🔍 **Ghi nhận thực tế (DOM export thật, 2026-07-05):** Portal thật KHÔNG dùng thuần 1 font "Material Icons" — icon thực tế là hỗn hợp nhiều thư viện SVG khác nhau tuỳ vị trí: `feather` (vd icon "box" ở Quản lý sản phẩm), `heroicons_outline` (hamburger, fullscreen, user-circle ở Topbar), `heroicons_solid` (chevron-right ở nav), `heroicons_mini` (chart-pie ở "Báo cáo"), `mat_solid` (settings), và font ligature "material-icons" chỉ cho vài icon lẻ (`more_vert` trong menu 3 chấm của table row, `file_upload` ở vài action button). figma-mcp-go không import được nhiều thư viện SVG khác nhau theo tên — **quy tắc 2 cách ở trên (Material Icons font / rectangle fallback) vẫn là giải pháp thực dụng để vẽ**, đây chỉ là ghi chú để design-agent không ngạc nhiên nếu thấy icon thật trông khác approximation khi so sánh side-by-side. Xem `.claude/skills/design-instruction/brand-portal/app-shell-reference.md` §3 để biết icon nào thuộc thư viện nào nếu cần đối chiếu.

---

### Step 3 — Map fields to components
For each screen, map each field to a Brand Portal component type:
- Textbox → Input field (radius 8px, 44px height)
- Dropdown → Select field
- Button primary → raised navy button
- Table → data table with pagination
- Toggle → pill-shaped toggle (radius 100px)
- Date picker → date input with calendar
- Numeric display → IBM Plex Mono, right-aligned

### Step 4 — Identify states (phạm vi giới hạn)

Chỉ vẽ các states sau — không thêm:

| Loại màn hình | States cần vẽ |
|---|---|
| List / Danh sách | Default (có data) + Empty state |
| Form Create / Edit | Default (form trống, button Lưu active) |
| Detail / View | Default |
| Dialog / Modal | Default popup only |

Skip hoàn toàn: error/validation state, loading state, disabled state, edge case screens.

### Step 5 — Load Brand Portal design reference (BẮT BUỘC — không còn optional)

> Section 6.6 chỉ là quick-ref rút gọn — có thể lệch so với Portal thật nếu chưa cập nhật. `.claude/skills/design-instruction/brand-portal/design-tokens.md` là **nguồn xác nhận cao nhất** hiện có (đã verify 1 phần từ CSS/HTML thật, xem mục "Nguồn xác nhận & mức độ tin cậy" cuối file đó).
> **Bắt buộc đọc trước khi vẽ bất kỳ frame nào** — không được bỏ qua vì "đã biết token". Nếu có xung đột giữa Section 6.6 và `design-tokens.md` → **`design-tokens.md` thắng**.

**Bắt buộc đọc (nếu file tồn tại — chỉ bỏ qua khi thật sự không tìm thấy file, phải RAISE ba-lead nếu thiếu):**

| File | Dùng để |
|---|---|
| `.claude/skills/design-instruction/brand-portal/design-tokens.md` | Tokens đầy đủ + mức độ tin cậy từng token — đọc mục Changelog + "Nguồn xác nhận" trước tiên |
| `.claude/skills/design-instruction/brand-portal/app-shell-reference.md` | **Reference đã verify từ DOM export thật** (2026-07-05) — Sidebar (đủ 9 nhóm cấp 1 + sub-item, allowlist đầy đủ), Topbar, Logo zone, icon library thật. Áp dụng cho MỌI màn có full app shell — đọc file này thay vì xin lại raw DOM |
| `.claude/skills/design-instruction/brand-portal/app-layout.html` | App shell structure: sidebar accordion, topbar |
| `.claude/skills/design-instruction/brand-portal/design-reference.html` | Visual demos: colors, type scale, components |
| `.claude/skills/design-instruction/brand-portal/screenshots/Quản lý chấm hình.png` | **Bắt buộc riêng cho feature VisibilityPRO** — xác nhận nhóm cấp 1 "Quản lý Chấm hình" + tên sub-item hiện có trước khi thêm nav item mới. Ảnh có theme màu cũ (navy) — CHỈ dùng để đọc tên/cấu trúc nhóm, KHÔNG dùng để lấy hex màu (dùng `design-tokens.md` cho màu) |

**Nếu có ảnh/HTML thật của đúng feature đang vẽ (BA cung cấp trong ticket) → ưu tiên tuyệt đối so với mọi file trong `.claude/skills/design-instruction/brand-portal/`.** Đọc kỹ layout thật trước, chỉ dùng `.claude/skills/design-instruction/brand-portal/*` cho phần chưa có ảnh thật che phủ.

**Đọc nếu tồn tại (visual reference — chỉ khi vẽ loại màn hình tương ứng):**

| File | Dùng khi |
|---|---|
| `.claude/skills/design-instruction/brand-portal/dom-reference-list-screen-pattern.md` | Vẽ màn **list/danh sách** (page header + filter bar + table + pagination) — reference đã verify từ DOM thật, khác app-shell |
| `.claude/skills/design-instruction/brand-portal/design-reference.html` | Vẽ màn hình create/edit form, form với validation errors, pagination — chưa có screenshot riêng cho các case này, dùng demo markup trong file này (§Form/§Pagination) |
| `.claude/skills/design-instruction/brand-portal/screenshots/Quản lý đơn hàng.png` | Reference table page với tabs |
| `.claude/skills/design-instruction/brand-portal/screenshots/Quản lý sản phẩm.png` | Reference filter grid |
| `.claude/skills/design-instruction/brand-portal/screenshots/Signin.png` | Màn hình đăng nhập |
| `.claude/skills/design-instruction/brand-portal/assets/logo.png` | Logo BonBon Shop |

---

### Step 6 — Vẽ màn hình lên Figma (figma-mcp-go)

#### 6.1 — Chuẩn bị Figma file

```
1. get_document          → hiểu state file hiện tại
2. get_pages             → kiểm tra page đã có chưa
3. navigate_to_page      → đi đến page đang làm việc
   HOẶC add_page         → tạo page mới nếu chưa có
                            (đặt tên theo feature: vd "Quản lý đơn hàng")
```

#### 6.1b — Tổ chức Figma theo user story (screenshot-ready)

Mỗi story group = 1 **Section frame** lớn chứa tất cả screens liên quan.
Screenshot 1 section = 1 ảnh → attach thẳng vào Jira ticket.

```
Page: "Quản lý đơn hàng"
│
├── [Section] "Story: Xem danh sách đơn hàng"     ← screenshot toàn section
│   ├── Frame "Danh sách — Default"
│   └── Frame "Danh sách — Empty"
│
├── [Section] "Story: Tạo đơn hàng mới"
│   ├── Frame "Thêm mới — Form"
│
└── [Section] "Story: Xóa đơn hàng"
    └── Frame "Dialog — Xác nhận xóa"
```

**Tạo section frame:**
```
create_frame  name="Story: [Tên]"  fills=transparent  stroke=none
→ width = (số screens × 1440) + (gap × (n-1))  height = auto
→ Frames xếp ngang, gap 64px giữa các screen
```

**Đặt tên frame chuẩn:** `[Tên màn hình] — [State]`
- Default state: `Danh sách đơn hàng — Default`
- Empty: `Danh sách đơn hàng — Empty`
- Dialog: `Dialog — Xác nhận xóa`

#### 6.2 — Naming conventions

| Element | Convention | Ví dụ |
|---|---|---|
| Page | Feature name (Vietnamese) | `Quản lý đơn hàng` |
| Frame (screen) | Screen name from Mermaid | `Danh sách đơn hàng`, `Thêm đơn hàng` |
| Frame (dialog) | `Dialog / [Name]` | `Dialog / Xác nhận xóa` |
| Layer group | Component role | `Topbar`, `Sidebar`, `Page Header`, `Filter Panel`, `Table`, `Pagination` |
| Component | Semantic name | `Button / Primary`, `Input / Default`, `Status / Success` |

#### 6.3 — App shell vs Content-only

**RULE: Mọi màn hình chính (list, form, detail, dashboard) ĐỀU PHẢI có sidebar + topbar.**
Chỉ dialog/modal mới được vẽ không có sidebar.

**Full app shell** (BẮTBUỘC cho tất cả màn hình không phải dialog):
```
Frame 1440×900 "Screen Name"
├── Sidebar [280×900, fill #162040]           ← LUÔN VẼ
│   ├── Logo [text bonbon shop]
│   └── Nav [accordion groups + sub-items]
└── Content [1160×900, fill #F3F4F6]
    ├── Topbar [1160×64, fill #FFFFFF, shadow-sm]     ← CHỈ: hamburger (trái) + right cluster (phải). TUYỆT ĐỐI KHÔNG đặt Page Title ở đây — title thuộc Page Header bên dưới.
    │   Right cluster (trái→phải): icon/language (globe) → gap 8 → icon/fullscreen → gap 8 → icon/account_circle → gap 8 → User Email
    │   KHÔNG có Divider giữa các cụm (đã verify từ DOM thật — app-shell-reference.md §2). Gap đồng nhất 8px.
    ├── Tab Bar [1160×48, fill #FFFFFF, border-bottom #E5E7EB]  ← VẼ NẾU PRD define tabs
    │   ├── Tab Active [padding 12 16, border-bottom 2px #071640, text #071640 600]
    │   └── Tab Inactive [padding 12 16, text #6B7280 400]
    │   (Active tab = tab đúng với màn đang vẽ trong spec này)
    └── Content Body [VERTICAL layout, NO padding — padding handled per zone]
        ├── Page Header [fill #FFFFFF, full-width]     ← KHÔNG margin
        ├── Filter Bar  [fill #FFFFFF, full-width]     ← KHÔNG margin (nếu chưa ở Topbar)
        └── Cards Container [fill #F3F4F6,             ← WRAPPER bắt buộc
             VERTICAL layout, paddingL:24, paddingR:24,
             paddingT:16, paddingB:24, gap:16]
            ├── Card / Chart / Table
            └── ...thêm cards
```

⚠️ **FULL-WIDTH STRETCH RULE — áp dụng cho MỌI child của Content Body và Cards Container:**
Sau khi `reparent_nodes` BẤT KỲ frame nào vào VERTICAL auto-layout container, phải gọi ngay:
```
set_constraints(nodeId=<child_id>, horizontal="STRETCH", vertical="TOP")
```
Áp dụng cho: **Page Header, Filter Bar, Sort Control, Cards Container, mọi Card/Panel/Table.**
KHÔNG được để width cố định (vd: `width=416`, `width=261`) trên bất kỳ full-width element nào.
Nếu không set STRETCH → Figma giữ nguyên width lúc tạo → element bị cụt so với container (lỗi phổ biến nhất).

**Tab Bar rules:**
- Chỉ vẽ khi PRD Section 4 define navigation pattern = Tabs
- Tất cả frames trong cùng feature đều có tab bar — chỉ khác tab nào đang active
- Tab labels lấy từ PRD Screen Map, giữ nguyên thứ tự
- Không tự sáng tạo tab bar khi PRD không define
- **Platform rule (HARD STOP — không phải gợi ý):** Tab Bar dùng cho **SFA mobile app**. **Portal web admin KHÔNG dùng Tab Bar** — dùng Sidebar sub-nav thay thế. Nếu feature là Portal screen mà PRD define Tabs → đây là dấu hiệu PRD sai pattern, không được tự vẽ Tab Bar để "khớp PRD" — phải dừng và raise lại cho BA xác nhận trước (xem hard-stop ở Step 1.5). Vi phạm rule này là nguyên nhân phổ biến nhất khiến output không giống Portal thật.

> ⚠️ **Cards Container là bắt buộc.** Figma auto-layout đặt children tại x=0 mặc định — nếu đặt card trực tiếp vào Content Body không có wrapper, card sẽ flush-left (sát mép trái) dù width nhỏ hơn 1160px. Chỉ Page Header, Filter Bar, Topbar là full-width (không margin). Tất cả cards/charts/tables phải nằm trong `Cards Container` với paddingLeft:24, paddingRight:24.

**Sidebar drawing spec (chi tiết — lý do nav hay bị xấu):**

```
Sidebar: width=280, height=900, fill=#162040
  auto-layout: VERTICAL, padding=0, gap=0, clip-content=true

  ⚠️⚠️ QUY TẮC BẮT BUỘC — fill=NONE cho MỌI frame con lồng bên trong Sidebar (lỗi thật đã
  xảy ra, SCR-02 Cấu hình người nhận email, 2026-07-06): `create_frame` của figma-mcp-go mặc
  định gán fill TRẮNG cho frame mới tạo. Nếu vẽ hàng loạt Nav Item/Sub-item/wrapper (Logo Zone,
  Logo Text Row, Nav Items container, từng Nav Item, Icon Label Group, Sub-item outer, Group
  Label Wrapper...) mà KHÔNG tự tay set fill=NONE cho từng cái, TOÀN BỘ sidebar sẽ hiện thành
  1 khối trắng phủ kín nền navy #162040 bên dưới (dù frame `Sidebar` gốc vẫn đúng navy) — vì
  các frame con full-width 280px xếp chồng theo VERTICAL auto-layout, mỗi lớp trắng che mất lớp
  navy phía sau nó. Bug này KHÔNG giới hạn ở Logo zone/Spacer (đã ghi từ trước) — nó lặp lại ở
  MỌI frame con bất kỳ nếu quên override fill mặc định.
  → Quy trình bắt buộc: sau khi `create_frame` bất kỳ frame nào lồng trong Sidebar, gọi ngay
    `set_fills(nodeId, [])` hoặc fill=NONE trong cùng lệnh tạo — KHÔNG để mặc định.
    Ngoại lệ DUY NHẤT: state overlay cố ý (vd active nav/sub-item dùng
    `background: rgba(255,255,255,0.12)` để tạo hiệu ứng highlight) — đây không phải bug, giữ nguyên.
  → Trước khi coi Sidebar hoàn thành: `get_nodes_info` toàn bộ children lồng nhau trong Sidebar,
    liệt kê fill của từng node — bất kỳ node nào (trừ active-state overlay) có `fills` khác rỗng
    (đặc biệt #FFFFFF) đều là lỗi, phải sửa lại NONE ngay.

Logo zone — BƯỚC 0 BẮT BUỘC trước khi dựng logo bằng text: tìm logo vector/asset thật đã có trong file:
  → get_local_components / search_nodes(query="bonbonshop") → nếu tìm thấy component/instance logo thật
     → BẮT BUỘC clone/instance component đó, KHÔNG dùng kỹ thuật text 2 màu bên dưới.
  → MẶC ĐỊNH (asset thật đã có sẵn, verify 2026-07-06): `import_image` file
     `.claude/skills/design-instruction/brand-portal/assets/logo-wordmark-dark.svg` (bản chữ trắng #FFFFFF + cam #EF4429 —
     dùng cho nền navy #162040 của Sidebar). Đây là vector wordmark đầy đủ "bonbon shop" +
     tagline "giá ngon, bán gọn", pixel-accurate với logo thật (khác `logo-wordmark-light.svg`
     chỉ ở màu fill: navy #013D50 thay vì trắng — dùng cho nền sáng, KHÔNG dùng trong Sidebar).
     ⚠️ TUYỆT ĐỐI KHÔNG import nhầm bản `-light` vào Sidebar — chữ navy trên nền navy sẽ biến mất
     (lỗi tương tự trắng-trên-trắng bên dưới).
  → Nếu asset trên không tồn tại/không import được → fallback: asset SVG/PNG gốc khác do
     user/ticket cung cấp → import_image asset đó.
  → CHỈ khi không tìm thấy component/asset NÀO → mới rơi về kỹ thuật "colored-o" text bên dưới
     (fallback cuối cùng, không phải mặc định).

Logo zone (kỹ thuật "colored-o" — FALLBACK CUỐI CÙNG, chỉ dùng khi KHÔNG import được asset thật ở trên):
  Frame: width=280, height=auto, padding=20 16 16 16
  fill=transparent — KHÔNG set fill riêng (đặc biệt KHÔNG set #FFFFFF) cho frame này.
  Logo zone phải kế thừa nền navy #162040 của Sidebar bên dưới.
  ⚠️ Lỗi thường gặp: set fill=#FFFFFF cho Logo zone → chữ logo (cũng fill=#FFFFFF) biến mất vì trắng-trên-trắng.
  Row 1 — Logo text rich-text "bonbon shop" trong đó:
      - chữ thường (b,n,n,s,h,o,p không phải chữ "o" đầu âm tiết) → fill=#FFFFFF
      - 2 chữ "o" đầu (trong "b[o]nb[o]n") → fill=#EF4429 (brand orange, khớp app-layout.html `.r{color:#EF4429}`)
        ⚠️ Giá trị cũ `#E11D48` (rose/red) đã lỗi thời — brand orange hiện hành là `#EF4429` (design-tokens.md v3+).
      - font=Inter, size=24, weight=900, letter-spacing=-0.02em
    ⚠️ Nếu figma-mcp-go không hỗ trợ multi-color trong 1 text node → tách thành nhiều create_text liền kề cùng hàng (HORIZONTAL auto-layout, gap=0): "b" trắng, "o" đỏ, "nb" trắng, "o" đỏ, "n shop" trắng.
  Row 2 — Tagline:
    create_text "giá ngon, bán gọn"
    font=Inter, size=9, fill=#FFFFFF, opacity=40
    margin-top=4

  ⚠️ Biết giới hạn: đây là bản xấp xỉ (text 2 màu) — chỉ nên xuất hiện khi có lý do kỹ thuật cụ thể
  khiến import_image thất bại; asset vector thật đã có sẵn (xem BƯỚC 0) nên trường hợp này hiếm.

Nav parent item (COLLAPSED — chevron right):
  Frame: width=280, height=48
  auto-layout: HORIZONTAL, padding=12 16, gap=8
  fill=transparent (no fill — để lộ sidebar bg)
  → Icon: create_text, font="Material Icons", size=20, fill=#FFFFFF, opacity=50
    text = tên icon (ví dụ: "bar_chart", "store", "settings", "event_note", "people")
    ← font Material Icons render ligature name thành icon vector, KHÔNG phải chữ thường
    KHÔNG dùng emoji (📊🎛️...) và KHÔNG dùng regular text làm icon
    Fallback nếu font "Material Icons" không có trong file: Rectangle 20×20, fill=#FFFFFF, opacity=50
  → Label: Inter 14/600, fill=#FFFFFF, opacity=85
    gap=8 từ icon   ← khoảng cách icon → title
  → Spacer: width=auto (fill remaining space), fill=NONE/transparent — TUYỆT ĐỐI không set background màu (kể cả trắng): lỗi thật ở SCR-01 set Spacer background=#FFFFFF → hiện thành 1 sợi line trắng mảnh nối liền Label→Chevron trong mọi nav item. Nếu dùng layoutGrow=1 để đẩy chevron sang phải, đảm bảo layer đó không có fill nào cả.
  → Chevron: create_text, font="Material Icons", size=16, fill=#FFFFFF, opacity=55
    text="chevron_right"   ← KHÔNG dùng "›" hay ">" text thường

Nav parent item (EXPANDED — chevron down):
  Tương tự collapsed, đổi chevron text="keyboard_arrow_down"   ← KHÔNG dùng "expand_more" (renders như chữ v mỏng), KHÔNG dùng "∨" hay "v" text thường
  → BẮT BUỘC rename_node thêm hậu tố " — Expanded" vào tên frame nav parent khi mở (vd `Nav / Cài đặt chung — Expanded`)
    để tên node phản ánh đúng trạng thái, tra được bằng search_nodes.

Sidebar Section Label (divider phân nhóm dạng text, vd "QUẢN LÝ"):
  Frame: width=280, height=auto
  auto-layout: HORIZONTAL, paddingLeft=16 (khớp padding các item khác), paddingRight=16,
               marginTop=24 (cách nav item liền trước — dùng itemSpacing của container hoặc frame spacer),
               marginBottom=8 (cách nav item liền sau)
  → Text: Inter 11/600, UPPERCASE, fill=#9CA3AF (hoặc #FFFFFF opacity thấp tương đương)
  ⚠️ KHÔNG để label sát mép trái (padding trái phải = 16px như mọi item), KHÔNG để spacing trên/dưới cập kênh.

Ranh giới cuối group expanded (BẮT BUỘC):
  Sau item cuối cùng của 1 group đang expanded, phải có margin-bottom tối thiểu 8px trước khi
  sang top-level item / section label kế tiếp — để phân biệt rõ ranh giới nhóm (tránh nhầm sub-item
  của group này với top-level item liền sau).

Item đứng riêng sau Section Label (BẮT BUỘC — lỗi thật đã xảy ra):
  Khi 1 nhóm sau Section Label chỉ có ĐÚNG 1 item (không phải parent có children, ví dụ "Quản lý tài khoản"
  đứng riêng dưới "QUẢN LÝ"), item đó VẪN dùng nguyên spec "Nav parent item" (auto-layout HORIZONTAL,
  counterAxisAlignItems=MIN, paddingLeft=16) — TUYỆT ĐỐI KHÔNG set counterAxisAlignItems=CENTER hay
  tự căn giữa cụm icon+label. Icon của item này phải thẳng hàng cột dọc (x=16) với icon của mọi
  top-level item khác trong sidebar, kể cả khi nó là item duy nhất trong nhóm.

Sub-item (INACTIVE):
  Frame: width=280, height=40
  auto-layout: HORIZONTAL, paddingLeft=48, paddingRight=16,
               paddingTop=8, paddingBottom=8
  fill=transparent
  → Text: Inter 14/600, fill=#FFFFFF, opacity=65
    ← weight=600 (cùng font đậm như active), chỉ khác opacity

Sub-item (ACTIVE — quan trọng nhất):
  Outer Frame: width=280, height=44, padding=2 8 (top/bottom 2, left/right 8)
  fill=transparent
  Inner Frame: width=264, height=40, border-radius=4
    fill=#FFFFFF, opacity=12
  → Text: Inter 14/700, fill=#FFFFFF, opacity=100
    ← weight=700 + opacity=100 để nổi bật rõ hơn nền mờ
```

**⚠️ Lỗi phổ biến với màu rgba trong Figma API:**
```
SAI:  set_fills(color="rgba(255,255,255,0.1)")   ← API không nhận string rgba
ĐÚNG: set_fills(color="#FFFFFF") + set_opacity(10)   ← fill hex + opacity %
```

**Content-only** (CHỈ dùng cho dialogs, modals, overlays):
```
Frame [width: 576px confirm / 70vw form, centered]
├── Header [title, border-bottom 1px #EEF2F6]
├── Body [form fields / message]
└── Footer [Hủy ghost + Lưu/Xác nhận navy, right-aligned]
```

#### 6.4 — Execution sequence mỗi screen

**SEQUENTIAL RULE — depth-first, không batch:** Hoàn thành TOÀN BỘ 1 screen (sidebar + topbar + content + tất cả components) trước khi tạo bất kỳ frame nào cho screen tiếp theo. **Không tạo placeholder frames rỗng.** Nếu context window sắp đầy → dừng lại, ghi log screens đã xong và screens chưa vẽ, raise lại để tiếp tục sau.

> **Clone flow exception:** Steps 1–7 bên dưới áp dụng cho draw-from-scratch (MASTER screen). Với screens clone từ MASTER → bỏ qua steps 1–7, thực hiện theo **Clone strategy** (cuối section 6.4).

Lý do: breadth-first (tạo tất cả frames trước, fill sau) dẫn đến tất cả screens đều 70% hoàn chỉnh khi context cạn. Depth-first đảm bảo mỗi screen hoặc là 100% hoặc là chưa tồn tại — không có trạng thái "nửa vời".

```
Với MỖI screen (xử lý tuần tự, xong screen này mới sang screen khác):

1. create_frame        → tạo frame với đúng width/height
2. set_fills           → bg color (#F3F4F6 cho app shell, #FFFFFF cho dialog)
3. set_auto_layout     → direction VERTICAL, padding 0, gap 0
4. Vẽ SIDEBAR đầy đủ trước (bắt buộc với mọi non-dialog screen):
   PRE-CHECK icon source (1 lần duy nhất, trước khi vẽ sidebar đầu tiên trong file):
     Chạy thứ tự ưu tiên fallback ở §Icon System (5 tầng) để xác định tầng cao nhất khả dụng cho file này:
       get_fonts → "Material Icons"? → nếu không, icon font khác? → nếu không, search_nodes/get_local_components
       tìm icon component/instance sẵn có? → nếu không, asset import? → nếu tất cả không có → rectangle (tầng 5).
     Ghi nhớ tầng đã chọn, áp dụng cho TẤT CẢ screens trong lần chạy — không check lại mỗi screen.
     ⚠️ Nếu buộc dùng tầng 5 (rectangle) → ghi "Icon Gaps" vào 05-design-state.md + RAISE ba-lead, không coi là xong.
   a. create_frame sidebar (width=280, height=900, fill=#162040) → lưu sidebar_id
      → Sau khi reparent sidebar vào main frame: set_constraints(sidebar_id, horizontal="LEFT", vertical="STRETCH")
      → Main frame phải dùng auto-layout HORIZONTAL với counterAxisAlignItems="STRETCH" để sidebar fill full chiều dọc
   b. Logo zone: create frame → reparent_nodes vào sidebar_id ngay lập tức → verify bằng get_node(sidebar_id)
   c. Mỗi nav parent item (tạo và reparent từng cái, không batch):
      - create_frame (280×48) → reparent_nodes vào sidebar_id ngay → sau đó mới tạo icon + label bên trong
      - KHÔNG tạo tất cả items rồi mới reparent một lần — lý do: reparent batch dễ silent fail
   d. Mỗi nav sub-item INACTIVE: create frame (280×40, paddingLeft=48) → reparent_nodes vào sidebar_id ngay
   e. Sub-item ACTIVE: tạo outer frame (280×44) → reparent vào sidebar_id → tạo inner frame (264×40) → reparent vào outer frame
   f. Verify sau khi vẽ xong toàn bộ sidebar:
      get_node(sidebar_id) → đếm số children.
      Số children phải = 1 (logo zone) + số nav items + sub-items.
      Nếu số lượng sai → search_nodes trên page để tìm node "loose" (nằm ngoài sidebar) → reparent_nodes lại.
      Node loose = node xuất hiện trên canvas ngoài sidebar frame = BUG, phải fix trước khi vẽ topbar.
5. Vẽ TOPBAR (height=64, fill=#FFFFFF, stroke bottom #E5E7EB)
   → Sau khi reparent topbar vào Content frame: set_constraints(topbar_id, horizontal="STRETCH", vertical="TOP")
   → Content frame dùng auto-layout VERTICAL với counterAxisAlignItems="STRETCH" → topbar tự fill full chiều ngang
   → KHÔNG hard-code width=1160 cho topbar — để Figma tự stretch theo Content width
6. Vẽ CONTENT (page header → filter bar → cards container → table/form/cards)
   - Với mỗi zone: create_frame → set_fills → set_auto_layout → add components → reparent
7. reparent_nodes: sidebar + topbar + content vào main frame

Screen Done Gate — kiểm tra trước khi sang screen tiếp:
□ Sidebar: logo zone + đúng số nav items + active sub-item highlighted
□ Sidebar stretch: sidebar fill full chiều dọc của frame (không bị cắt ngắn giữa chừng)
□ Topbar stretch: topbar fill full chiều ngang của Content frame (không có khoảng trắng bên phải)
□ Topbar: đúng height, bottom border #E5E7EB
□ Page Header stretch: width = đúng bằng Content width (không hard-code px nhỏ hơn) — verify bằng get_node
□ Filter Bar stretch: width = đúng bằng Content width — verify bằng get_node, KHÔNG tin giá trị lúc create
□ Cards Container stretch: width = đúng bằng Content width — verify bằng get_node
□ Chevron nav: verify fontFamily="Material Icons" bằng get_node — KHÔNG được là text Unicode ("›", "∨", "v", ">")
   Cách verify: get_node(chevron_node_id) → xem style.fontFamily = "Material Icons"
   Nếu sai → xóa node → tạo lại với create_text, font="Material Icons", text="chevron_right" (hoặc "keyboard_arrow_down")
□ Content: tất cả components theo spec đã được vẽ
□ Borders trên cards, inputs, filter chips, table đã có (set_strokes)
□ Pagination (nếu có table): render đủ 4 nút điều hướng First/Previous/Next/Last — kể cả khi disabled, KHÔNG được ẩn/xóa nút nào
□ Table header/data alignment (nếu có table): get_nodes_info từng cell Header row + Data row đầu tiên,
   liệt kê width theo đúng index cột, so sánh cặp — mỗi cột phải cùng width & cùng thứ tự ở cả 2 hàng.
   Khớp TỔNG width (rule ở trên) không đủ để coi là đúng — phải khớp positional từng cột.
□ Empty State (nếu có): glyph/title/description xếp VERTICAL center-center, gap 16px/8px — verify bằng
   get_node từng layer, KHÔNG được là 3 layer position:absolute cùng toạ độ (0,0) — lỗi thật đã xảy ra
   2026-07-06, gây chữ/icon chồng đè vỡ hình.
□ Layer naming: tên layer cột/field phải khớp nội dung hiển thị thật, KHÔNG trùng tên với layer khác
   trong cùng row/section — trùng tên hoặc tên lệch nội dung = dấu hiệu copy-paste từ bảng khác chưa rename.
□ Table cell content (nếu có table): mỗi cell vertical-align middle trong row h=64, Data Row padding
   0px 16px khớp Header Row, cột fixed-width tràn chữ → truncate ellipsis (không wrap 2 dòng).
□ Sidebar alignment: icon của MỌI top-level item (kể cả item đứng riêng 1 mình sau Section Label,
   ví dụ "Quản lý tài khoản" dưới "QUẢN LÝ") thẳng hàng cột dọc x=16 — verify bằng get_node,
   so sánh x của icon đó với icon "Cài đặt chung"/top item khác. Lệch cột = counterAxisAlignItems bị set CENTER, phải sửa lại MIN.
□ Sidebar fill: `get_nodes_info` TOÀN BỘ children lồng nhau trong Sidebar (Logo Zone, Nav Items
   container, từng Nav Item, Icon Label Group, Sub-item outer, Group Label Wrapper...) — bất kỳ
   node nào có `fills` khác rỗng (đặc biệt #FFFFFF) đều là lỗi (trừ active-state overlay
   rgba(255,255,255,0.12) cố ý). Lỗi thật đã xảy ra (SCR-02, 2026-07-06): quên set fill=NONE khi
   create_frame hàng loạt → default fill trắng của figma-mcp-go phủ kín navy #162040, cả sidebar
   hiện thành khối trắng.
□ KHÔNG có annotation text node trong frame (xem quy tắc bên dưới)
□ Double border: rà soát mọi cặp khối liền kề (card cạnh card, filter bar cạnh table...) theo bảng
   "Border giữa các khối liền kề" trong `design-tokens.md` — chỉ 1 trong 2 khối chạm nhau được giữ
   stroke, khối còn lại bỏ border phía chạm nhau (xem SKILL.md gotcha #7).
□ Icon Gaps: nếu có icon nào phải dùng rectangle placeholder (tầng 5, last-resort) → đã ghi vào
   `05-design-state.md` mục "Icon Gaps" + đã RAISE ba-lead — KHÔNG coi screen là xong nếu còn icon
   rectangle chưa ghi log.
→ Nếu bất kỳ ô nào chưa ✓ → sửa ngay, không sang screen tiếp
```

⛔ **TUYỆT ĐỐI KHÔNG tạo annotation text trong Figma frame:**
Không được tạo text node, frame, hay layer nào bên trong screen frame với mục đích giải thích state, điều kiện, hay logic kỹ thuật (ví dụ: "STATE A: Đang chạy → nút navy", "↑ xem state B", "TODO: thêm tooltip"). Những ghi chú này làm hỏng canvas và không có giá trị cho developer.
Tất cả ghi chú kỹ thuật và state description chỉ được lưu trong:
- File log `05-design--[feature].md` (section State Coverage)
- Wireframe ASCII (section Wireframes trong log)

**Icon trong sidebar:** Dùng `create_text` với font `"Material Icons"` — font này render tên icon thành vector glyph (ví dụ text `"bar_chart"` → icon biểu đồ cột). Nếu font không có → **climb thứ tự ưu tiên fallback ở §Icon System (5 tầng)** trước khi rơi về rectangle; rectangle (tầng 5) là last-resort, phải ghi "Icon Gaps" + RAISE ba-lead. KHÔNG dùng emoji hay Unicode ký tự.

**Clone strategy — tối ưu từ screen 2 trở đi (kể cả screens từ feature khác):**
- Screen 1 của story group đầu tiên trong queue (MASTER): vẽ đầy đủ từ đầu (sidebar + topbar + content). Sau khi hoàn thành → ghi frame ID vào `05-design-state.md` với key `MASTER_FRAME_ID: [node_id]`. Ba-lead đọc giá trị này và truyền cho mọi @design-agent tiếp theo.
- Nếu ba-lead truyền `master_frame_id` ≠ "NONE": bắt buộc dùng `clone_node(master_frame_id)` làm điểm xuất phát cho screen đầu tiên trong story group — kể cả khi story group này thuộc feature khác.
- Nếu `master_frame_id = "NONE"` (story group đầu tiên của toàn bộ CR): vẽ đầy đủ từ đầu → ghi MASTER_FRAME_ID vào 05-design-state.md.
- Từ screen 2 trở đi trong cùng story group: tiếp tục `clone_node` từ MASTER frame → rename → cập nhật 2 điểm:

  **Bước A — Tìm node sau khi clone:**
  Sau `clone_node(MASTER_FRAME_ID)` tất cả node ID đều mới. Dùng naming convention (section 6.5) để tìm:
  ```
  search_nodes(query="Nav / Sub / [tên active hiện tại] — Active", nodeId=cloned_frame_id)
  search_nodes(query="Nav / Sub / [tên active mới]", nodeId=cloned_frame_id)
  ```

  **Bước B — Đổi active nav (cấu trúc khác nhau, không chỉ đổi style):**

  Active → Inactive (xóa inner frame):
  ```
  1. search_nodes → lấy outer_frame_id của sub-item đang active (tên: "Nav / Sub / [label] — Active")
  2. get_node(outer_frame_id) → lấy inner_frame_id (child có fill=#FFFFFF opacity=12, width=264)
  3. delete_nodes(inner_frame_id)
  4. resize outer frame: height=40
  5. set_auto_layout: paddingLeft=48, paddingTop=8, paddingBottom=8, paddingRight=16
  6. rename_node: bỏ " — Active" khỏi tên
  7. get text node → style: fontWeight=600, opacity=65
  ```

  Inactive → Active (tạo inner frame):
  ```
  1. search_nodes → lấy inactive_frame_id (tên: "Nav / Sub / [label]")
  2. get_node(inactive_frame_id) → lấy text_node_id
  3. create_frame: width=264, height=40, radius=4 → lưu inner_frame_id
  4. set_fills(inner_frame_id, color="#FFFFFF") + set_opacity(inner_frame_id, 12)
  5. reparent_nodes([text_node_id], inner_frame_id)
  6. reparent_nodes([inner_frame_id], inactive_frame_id)
  7. resize inactive_frame_id: height=44
  8. set_auto_layout: paddingTop=2, paddingBottom=2, paddingLeft=8, paddingRight=8
  9. rename_node: thêm " — Active" vào tên
  10. get text node → style: fontWeight=700, opacity=100
  ```

  **Bước C — Replace content zone:**
  ```
  search_nodes(query="Content", nodeId=cloned_frame_id) → lấy content_id
  delete_nodes(content_id)
  → vẽ lại content đúng spec cho screen này (page header → filter bar → cards container)
  ```

  Không vẽ lại sidebar + topbar từ đầu — đảm bảo consistency và tiết kiệm thời gian.

⚠️ **Sau MỌI `clone_node`/copy-paste của 1 template dùng chung (Sidebar/Topbar/Popup/Dialog) sang screen khác — chạy Recursive Bounds-Check (xem gotcha #12, section 6.9) trước khi coi bước clone là xong.** Lỗi thật (CR-20260714, 8 popup screens, section `351:7305`): sau khi popup template được nhân bản sang từng screen, nhiều node con ở nhiều tầng lồng nhau giữ nguyên toạ độ x/y tính theo parent CŨ thay vì tính lại theo parent MỚI → nội dung bị đẩy ra ngoài bounds, frame hiện trắng/trống dù data vẫn còn nguyên bên trong. Không phải mọi node trong subtree đều lỗi (lỗi rải rác, có thể chỉ 2-3/12 children) — phải bounds-check TOÀN BỘ descendant tree, không suy diễn từ 1 vài node.

#### 6.5 — Node naming convention

Đặt tên node theo pattern cố định — không dùng "Frame 1", "Rectangle 2":

| Node | Pattern | Ví dụ |
|---|---|---|
| Screen frame chính | `[Feature] / [Screen name]` | `Evaluation Plan / List`, `Display Rules / Create` |
| Sidebar | `Sidebar` | |
| Nav item active | `Nav / [label] — Active` | `Nav / Danh sách đơn hàng — Active` |
| Nav item inactive | `Nav / [label]` | `Nav / Quản lý kho` |
| Nav item cấp 1 (expanded) | `Nav / [label] — Expanded` | `Nav / Cài đặt chung — Expanded` |
| Nav sub-item | `Nav / Sub / [label] — Active/Inactive` | |
| Sidebar section label | `Section / [LABEL]` | `Section / QUẢN LÝ` |
| Table header cell | `Col / [tên cột]` | `Col / Tên nhãn`, `Col / Ngày tạo` |
| Table data cell | `Cell / [tên cột]` | `Cell / Tên nhãn`, `Cell / Ngày tạo` |
| Table row | `Row [N] — [data]` | `Row 3 — VPR-001`, `Row 1 — Nguyên tắc A` |
| Status badge | `badge-[status]` | `badge-Active`, `badge-Stopped` |
| Icon node | `icon/[name]` | `icon/search`, `icon/chevron-right` |
| Button | `btn-[variant]` | `btn-primary`, `btn-ghost` |

> ⚠️ Icon fallback rectangle (§Icon System tầng 5) vẫn phải đặt tên `icon/[name]` như icon font thật — KHÔNG dùng tên generic (vd `icon-placeholder`, `Rectangle 2`).

⚠️ **Cột "Ví dụ" ở bảng trên chỉ minh họa FORMAT đặt tên node — không phải label thật.** Không được copy nguyên văn các label ví dụ (`Danh sách đơn hàng`, `Quản lý kho`, `VPR-001`...) vào bất kỳ screen nào không thật sự thuộc feature đó. Label thật của mọi nav item / row / button PHẢI lấy từ PRD Section 4 (Screen Map & Navigation) hoặc Mermaid diagram của spec đang vẽ — nếu không tìm thấy label thật tương ứng, dừng lại và hỏi BA, không tự đặt tên.

---

#### 6.6 — Component states quick-ref

Dùng bảng này khi khai báo Step 2 token contract — không tự đoán giá trị:

**Button**
```
Primary default   fill=#071640 · border=#071640 · text=#FFFFFF · h=44 · pad=10 16 · r=8
Primary disabled  fill=#E5E7EB · border=#E5E7EB · text=#9CA3AF · h=44 · pad=10 16 · r=8
Ghost default     fill=#FFFFFF · border=#E5E7EB · text=#4B5563 · h=44 · pad=10 16 · r=8
Ghost hover       fill=#F3F4F6 · border=#E5E7EB · text=#4B5563
Danger            fill=#DC2626 · border=#DC2626 · text=#FFFFFF · h=44 · pad=10 16 · r=8
```

**Field / Input**
```
Default    fill=#FFFFFF · border=#D1D5DB · placeholder=#9CA3AF · h=44 · pad=12 16 · r=8
Focus      fill=#FFFFFF · border=#071640 · text=#111827         · h=44
Error      fill=#FFFFFF · border=#DC2626 · text=#111827         · h=44
Disabled   fill=#F3F4F6 · border=#E5E7EB · text=#9CA3AF         · h=44
```

**Table**
```
Container     bg=#FFFFFF · border=#E5E7EB · r=12
Header row    h=48 · bg=#F9FAFB · border-bottom #E5E7EB · text=12px/600/UPPERCASE/#6B7280
Data row odd  h=64 · bg=#FFFFFF · border-bottom #E5E7EB
Data row even h=64 · bg=#F9FAFB · border-bottom #E5E7EB
Row hover     bg=#F3F4F6
Row selected  bg=#EEF0F6 · border-left 3px #071640
Pagination    h=56 · bg=#FFFFFF · border-top #E5E7EB
```

**Status badge (text-only)**
```
Active/Đang chạy  text=#059669 · bg=#DCFCE7 · r=4 · pad=2 8
Stopped/Dừng      text=#DC2626 · bg=#FEE2E2 · r=4 · pad=2 8
Pending/Mới       text=#D97706 · bg=#FEF9C3 · r=4 · pad=2 8
Draft             text=#4B5563 · bg=#F3F4F6 · r=4 · pad=2 8
```

**Dialog / Modal**
```
Overlay     fill=#111827 · opacity=50
Container   bg=#FFFFFF · r=16 · shadow "0 10px 28px rgba(15,23,42,.10)"
Header      h=64 · border-bottom #E5E7EB · pad=0 24
Body        pad=24 · gap=16 · direction VERTICAL
Footer      h=64 · border-top #E5E7EB · pad=0 24 · justify=flex-end
```

**Sort Control / Filter Chip** (dùng cho dashboard toolbar: sắp xếp, tổng count)
```
width      = fill-container (layoutGrow=1) — TUYỆT ĐỐI không hard-code px
height     = 48px
padding    = 8px 16px (không phải 24px — 24px quá chật)
gap        = 8px (giữa icon và label text)
border     = 1px #E5E7EB
radius     = 8px
bg         = #FFFFFF
layout     = HORIZONTAL · justify-content=space-between · align-items=center
Left side  = icon (Material Icons 16px #6B7280) + label text (Inter 14/400 #4B5563)
Right side = count badge hoặc meta text (Inter 12/600 #111827)
```
⚠️ Lỗi phổ biến: tạo Sort Control với `width=261` (fixed) → chip không fill theo container.
Luôn dùng `layoutGrow=1` hoặc `resize_nodes` theo đúng container width sau khi reparent.

---

#### 6.7 — Design tokens áp dụng khi vẽ

**HARD RULES — border bắt buộc (set_strokes):**
Đây là lý do phổ biến nhất khiến output trông "thiếu". Không được bỏ qua:

| Element | Stroke | Weight |
|---|---|---|
| Card, panel | `#E5E7EB` | 1px |
| Input field | `#D1D5DB` | 1px |
| Filter chip / tag | `#E5E7EB` | 1px |
| Button ghost | `#E5E7EB` | 1px |
| Table container | `#E5E7EB` | 1px |
| Table header/row separator | `#F3F4F6` | 1px (bottom only) |
| Dialog | `#E5E7EB` | 1px |
| Topbar | `#E5E7EB` | 1px (bottom only) |

**Colors** (từ design-tokens.md):
- Frame bg: `#F3F4F6` (app), `#FFFFFF` (card/topbar), `#162040` (sidebar)
- Primary button: fill `#071640`, text `#FFFFFF`
- Ghost button: fill `#FFFFFF`, stroke `#E5E7EB`, text `#4B5563`
- Input default: fill `#FFFFFF`, stroke `#D1D5DB`
- Input focus: stroke `#071640` + inset shadow
- Input error: stroke `#DC2626`
- Status text only: green `#059669` / amber `#D97706` / red `#DC2626` / blue `#2563EB`

**Typography** (Body/sm = default):
- Default body text: Inter 14px / 400 / lh 1.6
- Page title: Inter 24px / 500 (tracking-tight) — chốt 2026-07-06 (BA Lead điều chỉnh xuống từ 30px DOM thật), thay giá trị 20px/600 cũ (xem `.claude/skills/design-instruction/brand-portal/design-tokens.md` §Headings)
- Section header: Inter 16px / 600
- Table header: Inter 12px / 600 / UPPERCASE
- Caption/meta: Inter 12px / 400
- Numbers/IDs: IBM Plex Mono 12–14px

**Spacing** (4px grid):
- Card padding: 24px (comfortable) / 16px (compact)
- Page horizontal padding: 24px
- Field gap: 16px
- Filter field gap: 16px
- Topbar height: 64px
- Sidebar width: 280px

**Border Radius**:
- Tags, badges: 4px
- Buttons, inputs: 8px
- Cards: 12px
- Dialogs: 16px
- Pills, toggles: 100px

**Shadows**:
- Card resting: `0 1px 3px rgba(15,23,42,.06), 0 1px 2px rgba(15,23,42,.04)`
- Card hover: `0 4px 12px rgba(15,23,42,.08)`
- Dialog: `0 10px 28px rgba(15,23,42,.10)`

#### 6.8 — Vietnamese content rules
- Currency: `482,500,000 ₫` (comma thousands, space trước ₫)
- Date: `DD/MM/YYYY` (e.g. `12/04/2026`)
- Labels: UPPERCASE (e.g. `TÊN CỬA HÀNG`, `TRẠNG THÁI`)
- Button text: Sentence case (e.g. `Thêm mới`, `Xuất dữ liệu`)
- Tab text: plain (e.g. `Tất cả 128`, `Chờ duyệt`)

### Step 7 — Screenshot + structured verify

Sau khi vẽ xong tất cả story groups:

**1. Save screenshot gốc — 1 ảnh / 1 story group:**
```
save_screenshots
  nodeId:     [Section frame ID của story group]
  outputPath: artifacts/[feature-folder]/screenshots/[story-slug].png
  scale:      1
```

**2. Đọc ảnh → verify từng row trong bảng Step 2** — so sánh Expected vs Actual:

| Component | Property | Expected (Step 2) | Actual | Pass / Fail |
|---|---|---|---|---|
| Button (Primary) | height | 44px | ? | ✓ / ✗ |
| Button (Primary) | radius | 8px | ? | ✓ / ✗ |
| Button (Primary) | fill | `#071640` | ? | ✓ / ✗ |
| Field | height | 44px | ? | ✓ / ✗ |
| Field | border | `#D1D5DB` | ? | ✓ / ✗ |
| _(thêm row cho mỗi property trong Step 2)_ | | | | |

- Mọi ✗ → kèm fix instruction cụ thể (tool call nào, set value nào cần chạy để sửa)
- Những gì cần sửa thủ công trong Figma (icon, shadow, clip content) → note riêng

**3. Fix tất cả ✗.**

**4. Sau khi fix xong — CHỈ KHI tất cả rows = ✓ — save verify screenshot:**
```
save_screenshots
  nodeId:     [Section frame ID của story group]
  outputPath: artifacts/[feature-folder]/screenshots/verify-[story-slug].png
  scale:      1
```

**5. Xóa ảnh gốc** `[story-slug].png` (có lỗi). Output cuối chỉ giữ `verify-[story-slug].png` — prefix `verify-` = đã qua QC.

> ⚠️ Nếu còn ✗ chưa fix → KHÔNG xóa ảnh gốc → fix tiếp → lặp lại từ bước 3.

**6. Chạy QC checklist** sau khi đã xem verify screenshot.

---

#### 6.9 — figma-mcp-go Auto-layout Gotchas (BẮT BUỘC đọc trước khi vẽ bất kỳ frame nào)

Phát sinh thật khi vẽ SOS Dashboard (2026-07-04) và CR-20260714 (2026-07-14) — 12 lỗi lặp lại nhiều lần vì không biết hành vi auto-layout/coordinate của figma-mcp-go. Đọc kỹ để không lặp lại:

**1. Auto-layout ghi đè x/y thủ công.** Nếu parent frame có auto-layout (`layoutMode` HORIZONTAL/VERTICAL), tham số `x`/`y` truyền vào `create_frame`/`create_text`/`create_rectangle` **bị bỏ qua hoàn toàn** — con mới luôn bị auto-layout tự đặt vị trí theo thứ tự tạo. Trước khi đặt children thủ công vào 1 frame, luôn `get_node` frame đó trước để biết có auto-layout hay không (frame KHÔNG auto-layout — như Sidebar, Screen — mới tôn trọng x/y truyền vào).

**2. Chèn 1 phần tử vào giữa 2 sibling có sẵn trong auto-layout container.** Không có tham số "chèn tại vị trí N". Cách làm đúng: tạo phần tử ở cuối (nó sẽ tự rơi xuống cuối danh sách con) → dùng `reorder_nodes` với `order: "sendBackward"` gọi lặp lại N lần để đưa nó lên đúng vị trí mong muốn (mỗi lần lùi 1 bậc). KHÔNG dùng `move_nodes` cho việc này (xem mục 4).

**3. `move_nodes` chỉ có tác dụng khi parent KHÔNG có auto-layout.** Với parent có auto-layout, gọi `move_nodes` trên 1 child của nó sẽ không giữ được vị trí mong muốn (auto-layout tính lại ngay). Chỉ dùng `move_nodes` để di chuyển giữa các frame không-auto-layout (vd Sidebar khi cần dịch cả loạt nav item xuống sau khi Logo Zone đổi chiều cao).

**4. `SPACE_BETWEEN` dàn đều TẤT CẢ children, không tách "cụm trái/cụm phải".** Nếu 1 frame auto-layout có primaryAxisAlignItems=SPACE_BETWEEN và > 2 children trực tiếp, tất cả sẽ bị dàn cách đều nhau trên toàn bộ chiều rộng — không tạo được hiệu ứng "1 icon bên trái, 1 cụm nhiều icon bên phải". Cách đúng: gom toàn bộ cụm bên phải (hoặc trái) vào **1 frame con auto-layout riêng** (itemSpacing nhỏ, counterAxisAlignItems CENTER) rồi mới đặt frame con đó làm **child duy nhất còn lại** — để cha chỉ có đúng 2 children, SPACE_BETWEEN lúc đó mới neo đúng 2 đầu.

**5. Frame hug-sizing (`primaryAxisSizingMode: AUTO`) tự co lại khi thêm children mới — phá vỡ chiều cao đồng nhất với sibling.** Ví dụ: 1 tab đang cao 48px (bằng các tab khác) nhưng đang ở chế độ hug — thêm 1 rectangle underline vào sẽ làm tab đó co lại còn đúng bằng nội dung (vd 33px), lệch hẳn so với các tab 48px bên cạnh. Trước khi thêm children mới vào 1 frame cần khớp kích thước với sibling (tab, nav item, card...): `set_auto_layout` với `primaryAxisSizingMode: FIXED` rồi `resize_nodes` về đúng kích thước target TRƯỚC, sau đó mới thêm children.

**6. `resize_nodes` KHÔNG scale children bên trong component/instance có nhiều vector con.** Dùng để thu nhỏ 1 component phức tạp (vd logo nhiều vector) sẽ chỉ resize khung bao ngoài — nội dung bên trong giữ nguyên kích thước gốc và bị cắt (clip) nếu nhỏ hơn. KHÔNG dùng `resize_nodes` để scale logo/icon phức tạp; dùng đúng kích thước gốc của component, nếu cần size khác phải xin asset đã đúng size hoặc mở rộng vùng chứa cho vừa (không thu nhỏ nội dung).

**7. Border đôi khi 2 khối liền kề đều có stroke 4 cạnh.** Xem bảng quy ước trong `.claude/skills/design-instruction/brand-portal/design-tokens.md` mục "Border giữa các khối liền kề" — chỉ 1 trong 2 khối chạm nhau được giữ stroke.

**8. Không thể "chèn overlay" (label nổi đè viền, badge đè góc...) vào 1 frame auto-layout bằng cách thêm child mới ở toạ độ tự do.** Con mới luôn bị đẩy vào cuối flow theo hướng layoutMode (HORIZONTAL → dạt sang phải; VERTICAL → dạt xuống dưới), bất kể truyền x/y gì (xem thêm mục 1). Gặp khi vẽ floating label cho Filter Bar field: text label mới bị đẩy ra ngoài field, tràn hẳn sang field bên cạnh. Cách đúng khi cần overlay/notch thật đè lên viền của 1 frame vốn auto-layout: **bọc frame đó trong 1 Wrapper frame KHÔNG auto-layout** (`layoutMode: NONE`) — bên trong Wrapper, x/y thủ công hoạt động bình thường (đúng mục 1), nên đặt được Label + Notch BG đè lên viền của field gốc (giờ là 1 child của Wrapper, dịch xuống vài px để chừa chỗ cho label ở phía trên). KHÔNG dùng cách "tái cấu trúc field thành 2 tier xếp chồng bên trong" (label nằm gọn trong field, VERTICAL layout) — đã thử và **sai**, không đúng hiệu ứng label đè viền thật (verify từ HTML `mat-select` Angular Material do user cung cấp). Chi tiết kỹ thuật Wrapper đầy đủ: `.claude/skills/design-instruction/brand-portal/design-tokens.md` mục "Floating Label (Material outline pattern)".

**9. `Spacer`/filler layer trong auto-layout dễ vô tình lộ màu nếu set fill.** Layer dùng để đẩy sibling khác sang 1 phía (vd đẩy chevron sang phải trong nav item) chỉ nên có `fill=NONE`. Lỗi thật (SCR-01, 2026-07-04): Spacer bị set `background=#FFFFFF` → hiện thành 1 sợi line trắng nối Label→Chevron trong mọi nav item. Luôn kiểm tra Spacer/filler không có `fills` nào trước khi coi frame đã xong.

**10. Tổng width các cột trong 1 table row PHẢI khớp đúng content width của row (row width − padding trái/phải), nếu không cột cuối phải `layoutGrow=1`/width=auto.** Lỗi thật (SCR-01, 2026-07-04): 6 cột cộng lại 1030px nhưng row content width là 1080px → dư 50px cuối row không thuộc cột nào. Vì mỗi Cell tự set `fill=#FFFFFF` riêng còn Row nền zebra `#F9FAFB` cho hàng chẵn, phần dư 50px lộ ra màu `#F9FAFB` của Row → nhìn như "màu sẫm hơn ở cuối hàng", không fill hết bảng. Fix: `set_auto_layout` cột cuối cùng `layoutGrow=1` hoặc set width bằng đúng phần còn thiếu.

**11. (Nice-to-have) Heuristic phân bổ width giữa các cột theo loại nội dung — bổ sung cho #10, không thay thế.** #10 đảm bảo TỔNG width cột khớp full content width; #11 quyết định cách chia HỢP LÝ bên trong cái tổng đó (tránh cột ngày quá rộng còn cột tên biến thiên quá hẹp):
```
- Cột ngày tháng (DD/MM/YYYY): 120–160px
- Cột số/đếm (badge, count):   100–140px
- Cột action (icon buttons):   ~72px (1 icon more_vert — xem control-type-library §9) / 80–120px nếu nhiều icon
- Cột tên/text biến thiên:     phần width CÒN LẠI (layoutGrow=1, không hard-code)
```
Cột biến thiên `layoutGrow=1` hút hết phần dư sau khi trừ các cột cố định → tổng luôn tự khớp full width, không tạo khoảng trắng thừa cũng không tràn.

**12. Clone/copy-paste 1 template dùng chung (Sidebar/Topbar/Popup/Dialog) sang nhiều screen để lại toạ độ x/y "mồ côi" (stale) ở nhiều tầng lồng nhau — không chỉ tầng con trực tiếp — gây frame hiện trắng/trống dù data vẫn còn.** Lỗi thật (CR-20260714--nhan-dien-san-pham-tinviet, section `351:7305`, 8 screens SCR-01→SCR-07): popup/dialog template được nhân bản (copy-paste hoặc `clone_node`, không phải Component Instance) sang từng screen. Vì x/y trong Figma luôn **relative-to-immediate-parent** (không phải canvas-absolute — đã confirm qua `move_nodes`/`get_node`), một số node con vẫn giữ nguyên x/y tính theo parent CŨ (bản gốc) thay vì được tính lại theo parent MỚI khi reparent/clone → node bị đặt ra ngoài bounds của parent mới → nội dung "biến mất" khỏi vùng nhìn thấy, screenshot hiện trắng/trống dù layer vẫn tồn tại trong document.
   - **Lỗi rải rác, không đều:** không phải mọi node trong subtree đều lỗi — ví dụ trong 1 popup 480×320 có ~12 children, chỉ 3 node bị (Popup Title, Icon/Close, Divider/Header), phần còn lại đã đúng sẵn. Không được suy diễn 1 công thức áp cho cả subtree — phải kiểm tra TỪNG node.
   - **Công thức sửa khác nhau theo độ sâu:** tầng 2 (con trực tiếp của container lỗi) thường lệch theo 1 offset cố định — đo qua ít nhất 1 node landmark full-width (vd Divider, x phải =0) để xác nhận offset trước khi áp dụng hàng loạt. Tầng 3 trở lên (element lồng trong 1 component con, vd label lồng trong Button) **không** theo cùng offset đó — mỗi node ở tầng này có toạ độ độc lập, phải tự suy công thức riêng (vd centering: `local_x = (container_w - content_w)/2`) và bắt buộc verify bằng ràng buộc vật lý (label phải nhỏ hơn button chứa nó) trước khi tin kết quả — một offset áp nhầm tầng có thể ra giá trị "hợp lý nhìn thoáng qua" nhưng vật lý sai (label rộng hơn button).
   - **Cách phòng từ gốc:** ưu tiên Component Instance (tạo 1 component gốc rồi `clone_node`/instance nó) hoặc Auto Layout cho mọi template dùng chung nhiều screen (Sidebar/Topbar/Popup) — cả 2 cách này khiến Figma tự tính lại vị trí con theo parent mới, loại bỏ hoàn toàn class lỗi "toạ độ mồ côi". Copy-paste raw frame (không phải instance) là nguyên nhân gốc của lỗi này.
   - **Cách phát hiện:** KHÔNG chỉ tin `save_screenshots`/`get_screenshot` — nội dung có thể invisible (off-canvas) mà ảnh vẫn chỉ hiện "trắng" chung chung, không tự giải thích nguyên nhân. Chạy **Recursive Bounds-Check** sau mỗi lần clone/duplicate template dùng chung:
```
get_node(cloned_frame_id) → lấy toàn bộ descendant tree (mọi tầng, không chỉ depth-1)
Với MỖI node ở MỌI tầng, kiểm tra so với immediate parent của nó:
  node.x + node.width  ≤  parent.width
  node.y + node.height ≤  parent.height
  node.x ≥ 0  và  node.y ≥ 0
→ Node nào vi phạm = ứng viên "toạ độ mồ côi" → dừng lại, xác định offset đúng (qua landmark node) rồi move_nodes sửa, RE-VERIFY bằng get_node trước khi coi là xong.
```

## Quality Control

| Check | Criteria |
|---|---|
| **Sidebar present** | Mọi màn hình chính ĐỀU có sidebar 280px navy (`#162040`) — không có sidebar = fail |
| **Topbar không chứa Page Title** | Topbar chỉ có hamburger/language/fullscreen/user — Page Title phải nằm trong Page Header (Content Body). Title lọt vào Topbar = fail |
| **Nav item cấp 1 hợp lệ** | Item mới thêm vào Sidebar phải thuộc nhóm cấp 1 đã tồn tại trong IA thật (xem `.claude/skills/design-instruction/brand-portal/screenshots/`) hoặc PRD Section 4 allowlist — tự bịa flat item mới = `MENU_ITEM_GAP`, fail |
| **Tab bar consistent** | Nếu PRD define tabs: tất cả frames ĐỀU có tab bar, đúng labels, đúng active tab — thiếu hoặc sai tab = fail |
| **Borders present** | Cards, inputs, filter chips, tables, dialogs ĐỀU có stroke 1px — không có border = fail |
| **Cards Container** | Cards/charts/tables KHÔNG được đặt trực tiếp vào Content Body — phải có `Cards Container` wrapper với paddingL:24, paddingR:24 |
| Screen coverage | Every screen in Mermaid diagram có frame trong Figma |
| Story grouping | Screens grouped theo user story, mỗi group là 1 Section frame |
| Component mapping | Every field in spec maps to a Brand Portal component |
| States complete | List: Default + Empty. Form/Detail: Default only. Dialog: popup default only. KHÔNG cần error/loading/disabled state. |
| Brand tokens | Primary #071640, sidebar #162040, Body/sm 14px Inter, IBM Plex Mono cho số |
| VN formatting | Số tiền `482,500,000 ₫`, ngày `DD/MM/YYYY` trong tất cả mock data |
| Layout | Mỗi frame đúng dimensions, auto layout, spacing theo 4px grid |
| Nav Spacer/filler | KHÔNG có fill màu — có thì hiện line lạ nối Label→Chevron |
| Table column width | Tổng width cột = content width row (hoặc cột cuối layoutGrow=1) — thiếu sẽ lộ màu zebra ở cuối hàng |
| Table header/data alignment | Header cell và Data cell tại cùng vị trí (index) phải cùng width, cùng thứ tự — verify bằng cách liệt kê width từng cột Header rồi so với width từng cột ở Data row đầu tiên trước khi coi Table là xong. Chỉ khớp tổng (rule trên) KHÔNG đủ — từng cột phải khớp positional. |
| Empty State layout | Glyph/Title/Description phải xếp VERTICAL, center-center (gap 16px sau glyph, gap 8px sau title) — KHÔNG được dùng `position: absolute` cùng toạ độ cho 3 layer này (gây chồng đè, vỡ chữ). Xem `design-tokens.md` §Empty State. |
| Layer naming khớp nội dung | Tên layer (đặc biệt cột table, field) phải phản ánh đúng nội dung hiển thị thật (vd cột hiển thị "TRẠNG THÁI" phải đặt tên `col-trang-thai`, không giữ tên copy từ bảng khác như `col-ma-ke-hoach` cho cột "MÃ LƯỢT") và KHÔNG được trùng tên với layer khác trong cùng row — trùng tên = dấu hiệu copy-paste chưa rename. |
| Table cell content vertical-align | Nội dung mọi cell (text/badge/toggle) căn giữa theo chiều dọc trong row height 64px — không neo top/bottom lệch nhau giữa các cell trong cùng row. Xem `design-tokens.md` §Table — Data Row Content. |
| Table cell padding khớp Header | Data Row container dùng đúng `padding 0px 16px` như Header Row — width cột khớp (rule trên) nhưng padding gốc lệch vẫn làm nội dung lệch cột. |
| Table cell truncation | Cột fixed-width bị tràn nội dung → truncate 1 dòng (ellipsis), KHÔNG wrap 2 dòng (phá row height 64px đồng nhất). Ưu tiên để cột nội dung dài flexible/auto thay vì fixed. |
| Table action column | Cột action = 1 icon `more_vert` mở menu, width ~72px — không nhiều icon riêng lẻ trừ khi spec yêu cầu |
| Pagination controls | Render đủ 4 nút First/Previous/Next/Last — disabled khi không áp dụng, KHÔNG được ẩn nút nào |
| Full-width stretch | Page Header / Filter Bar / Cards Container đều stretch = Content width (verify get_node) — không hard-code px nhỏ hơn |
| Topbar right cluster | icon/language (globe) → fullscreen → user, gap 8px, KHÔNG có Divider. KHÔNG dùng `icon/flag`/`flag-vi` |
| Recursive bounds-check sau clone | Sau clone/duplicate Sidebar/Topbar/Popup/Dialog sang screen khác: mọi descendant ở MỌI tầng có `x+width ≤ parent.width` và `y+height ≤ parent.height` — verify bằng `get_node`, không chỉ tin screenshot. Vi phạm ở bất kỳ tầng nào = FAIL (xem gotcha #12) |

## HITL Checklist

Sau khi vẽ xong trên Figma, BA validate:
- [ ] Tất cả màn hình trong spec flow đều có frame trong Figma?
- [ ] Tab bar (nếu có) nhất quán qua tất cả frames? Active tab đúng với từng màn?
- [ ] Component names đúng với Brand Portal design system?
- [ ] List screens có Default + Empty state? Form/Detail có Default? Dialog có popup?
- [ ] Brand tokens (màu, font, spacing) áp dụng đúng?
- [ ] VN formatting (số tiền, ngày tháng) đúng?
- [ ] Component nào cần build từ đầu đã được flagged rõ?
- [ ] Topbar KHÔNG chứa Page Title (chỉ hamburger/language/fullscreen/user)?
- [ ] Nav item mới (nếu có) đã confirm đúng nhóm cấp 1 có sẵn trong IA thật, không tự bịa flat item?
- [ ] Đã đọc `.claude/skills/design-instruction/brand-portal/design-tokens.md` mục "Nguồn xác nhận & mức độ tin cậy" và ghi rõ token nào còn `[cần verify]` trong log?
- [ ] Với mỗi loại popup/dialog dùng chung nhiều screen (nếu có clone/duplicate): đã `get_node` ít nhất 1 đại diện và xác nhận mọi children ở mọi tầng nằm trong bounds của parent — không chỉ dựa vào screenshot (xem gotcha #12, section 6.9)?

## v2 — Control Type Library, deviation & 2-tier nav authority

**Deviation model:** đọc `references/control-type-library.md` (behaviour/state mặc định + Terminology). Khi map field → component, kế thừa Default Behaviour/States; chỉ tả deviation. Không phát minh interaction đã có trong library.

**3 section bắt buộc (khớp write-spec v2):** Message Placement (surface + MSG-* + dismiss) · Control States (disabled/active/loading + điều kiện) · Overlay Context (Close Triggers × outcome × có lưu không) — lưu trong `05-design-state.md`.

**2-tier navigation authority:**
- **L1** (đổi portal / nav schema / shell / thêm pattern mới) = hard stop `DESIGN_GAP` → RAISE ba-lead, chỉ Lead BA + user quyết.
- **L2** (thêm menu item vào schema có sẵn) = soft gate `MENU_ITEM_GAP` → ghi log + tiếp tục, reviewer check.
- Active-menu phải nằm trong Menu Item List của PRD Section 4 (allowlist) — không tự bịa path.

**Wording:** mọi ghi chú hành vi dùng từ chuẩn (ấn/điền/chọn/mở...) theo control-type-library §Terminology.

## v2 — Message Zones legend + Overview (hoàn tất 7-block)
- Mỗi wireframe/state: đặt **marker message đúng vị trí field** (vd `⚠①` cạnh field lỗi) + cuối wireframe có **legend "Message Zones"** map marker → MSG-* + surface (inline/toast/banner). Reviewer đếm được, không cần gu thẩm mỹ.
- Overview mỗi screen khai **Portal ID / Nav Schema / Active menu** (khớp spec Overview block + shared-shell allowlist). Active menu ngoài Menu Item List PRD Section 4 → `MENU_ITEM_GAP`.
