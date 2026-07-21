# Brand Portal — Design Tokens

> Design reference for `design-instruction` skill.
> Values only — no framework code, no class names.

## Changelog

- **v7 (2026-07-06, cuối ngày):** Page title (H1 admin) đổi từ 30px/36px xuống **24px/32px, Medium (500), tracking-tight** — BA Lead điều chỉnh giảm size so với giá trị 30px vừa chốt tạm cùng ngày (DOM thật đo được `text-3xl`). Đây là quyết định thẩm mỹ có chủ đích, cùng tinh thần với quyết định Navy/Indigo ở v3: font-weight/style (Medium, tracking-tight) vẫn khớp DOM, nhưng size là lựa chọn riêng của BA Lead — KHÔNG nhân bản y hệt DOM thật. Đã đồng bộ 24px/32px trên `design-tokens.md` (note Headings + bảng Usage mapping), `SKILL.md` (Typography quick-ref, đồng thời fix path reference cũ bị rút gọn sai `brand-portal/...` → path đầy đủ), `design-reference.html` (Type Roles demo), và `dom-reference-list-screen-pattern.md` (ghi rõ 30px là DOM đo được, 24px mới là giá trị chuẩn áp dụng). Đã kiểm tra `.claude/agents/` — không có agent nào tham chiếu pixel value của Page title, chỉ có rule cấu trúc (Page Title không đặt trong Topbar) nên không cần sửa.
- **v6 (2026-07-05, tiếp):** BA cung cấp thêm 1 DOM export thật — màn "Danh sách sản phẩm AI nhận diện" (`/product/product-ai`). Bổ sung vào `app-shell-reference.md` (sub-items nhóm "Quản lý sản phẩm", thêm icon `file_upload` vào danh sách material-icons font ligature) và `dom-reference-list-screen-pattern.md` (3 biến thể quan trọng: (1) filter bar grid-cols KHÔNG cố định ở 10, tính theo số field; (2) không phải màn list nào cũng có nút "Tìm kiếm" — có màn chỉ có "Xóa" (live-filter); (3) cột "Trạng thái" có 2 kiểu render — Badge cho enum nhiều giá trị (vòng đời) vs Toggle switch cho boolean editable — chọn kiểu theo ngữ nghĩa field, không mặc định 1 kiểu duy nhất). Không có thay đổi token màu/spacing.
- **v5 (2026-07-05):** BA cung cấp DOM export thật của màn `Danh sách kế hoạch đánh giá` (`/visibility/evaluation-ai-plan`) — verify thêm nhiều điểm: (1) Sidebar IA nhóm "Quản lý Chấm hình" **xác nhận đúng 100%** với quyết định SCR-04 đã chọn trước đó (7 sub-item: Danh sách kế hoạch đánh giá, Danh sách chấm hình, Tiêu chí chấm hình, Danh sách nguyên tắc trưng bày, Cài đặt lý do, Danh sách kế hoạch hậu kiểm, Danh sách kế hoạch xét thưởng) — chi tiết đầy đủ trong `app-shell-reference.md`; (2) **Mâu thuẫn phát hiện, CHƯA áp dụng:** Topbar thật dùng ảnh cờ quốc gia thật (`assets/images/flags/VN.svg`) cho nút đổi ngôn ngữ, khác rule `icon/language` (globe) đang áp dụng (xem v4) — BA Lead quyết định **giữ nguyên `icon/language`**, coi việc Portal thật dùng cờ là điểm cần sửa trong tương lai chứ không phải chuẩn để nhân bản; (3) Xác nhận Portal thật **có sẵn asset raster `logo-text.png`** cho Logo zone — khác giả định "chưa có asset" ở v4 — BA Lead quyết định **giữ tạm kỹ thuật "colored-o"** cho đến khi có file export chính thức; (4) Icon thật trong Portal là **hỗn hợp nhiều thư viện SVG** (feather, heroicons_outline/solid/mini, mat_solid, font ligature material-icons chỉ dùng cho vài icon như `more_vert`) — KHÔNG phải thuần 1 font "Material Icons" như SKILL.md §Icon System đang giả định — xem ghi chú cập nhật trong SKILL.md; (5) Badge trạng thái **kế hoạch** ("Thực thi") dùng class `badge-blue` (xanh dương) — khác thang màu Status badge generic (Active=xanh lá) ở SKILL.md §6.6, cần phân biệt 2 bộ màu này khi vẽ màn Evaluation Plan. Toàn bộ facts chi tiết đã trích xuất 1 lần, tách theo đúng phạm vi: `app-shell-reference.md` (Sidebar/Topbar/Logo/Icon — dùng chung MỌI màn) và `dom-reference-list-screen-pattern.md` (Page Header/Filter/Table/Pagination — riêng loại màn list) — design-agent đọc 2 file này thay vì xin lại raw DOM mỗi lần vẽ.
- **v4 (2026-07-04, tối):** Fix 3 lỗi thật phát hiện khi user review SCR-01 (Báo cáo kết quả hình trưng bày): (1) Topbar dùng nhầm `icon/flag-vi` (Material Icon "flag" — lá cờ hiệu/pennant chung chung, KHÔNG phải quốc kỳ Việt Nam) thay vì `icon/language` (globe) như bản đã verify (SOS Dashboard) — đổi lại `icon/language`; (2) Sidebar nav item có layer `Spacer` bị set `background=#FFFFFF` → hiện thành 1 sợi line trắng nối liền Label→Chevron trên mọi nav item — giờ bắt buộc Spacer/filler layer `fill=NONE`; (3) Table row: 6 cột cộng lại chỉ 1030px trong khi content width row là 1080px → dư 50px cuối hàng lộ màu nền zebra của Row (`#F9FAFB`) vì mỗi Cell tự set fill trắng riêng — bắt buộc tổng width cột = content width row, cột cuối `layoutGrow=1` nếu thiếu. Đồng thời cập nhật Logo zone sang kỹ thuật "colored-o" 2 màu (khớp `app-layout.html`) thay vì chữ trắng phẳng — vẫn là bản xấp xỉ, CHƯA pixel-perfect so với vector wordmark đầy đủ thấy trên SOS Dashboard thật (cần asset SVG/PNG gốc để đạt fidelity cao hơn — `logo-wordmark.svg` hiện có trong assets là placeholder SAI brand, không dùng được).
- **v3 (2026-07-04, cuối ngày):** User (BA Lead) xem trực tiếp bản vẽ SCR-01 dùng theme v2 (indigo `#4F46E5` / sidebar gray-900 `#111827`, verify từ CSS thật) và qua 3 vòng feedback liên tiếp đều yêu cầu **đổi ngược lại navy** — Sidebar → `#162040`, Primary/CTA/Tab-active/KPI-accent → `#071640`. Đây là **quyết định thẩm mỹ có chủ đích của user, ghi đè lên finding "verified từ CSS thật"** của v2 — không phải revert do v2 sai, mà do user chọn hướng thiết kế khác cho Portal (navy) dù CSS đang chạy thực tế là indigo. Giữ lại phần **neutral scale gray (gray-50→900, đổi từ slate ở v1)** vì không bị phản hồi gì — kết quả v3 là bản **hybrid**: Primary/Sidebar = navy (như v1), Neutral = gray (như v2), Semantic (success/warning/danger/info) = không đổi qua các version.
- **v2 (2026-07-04, sáng):** Đổi toàn bộ sang Material/Fuse indigo — đã bị v3 ghi đè cho Primary/Sidebar, xem lý do trên.
- **v1 (trước 2026-07-04):** Theme navy gốc, neutral slate.

**Ghi chú quan trọng cho lần dùng sau:** Nếu BA/design-agent thấy CSS/screenshot thật của Portal cho ra màu khác `#071640`/`#162040` (ví dụ đúng như v2 đã verify là indigo/gray-900) — **không tự ý đổi lại theo CSS**. Token trong file này phản ánh **hướng thiết kế user đã chọn**, có thể khác màu đang chạy thật trên production. Nếu nghi ngờ lệch, RAISE hỏi lại BA Lead thay vì tự sửa theo CSS.

---

## Colors

### Primary

| Name | Hex | Usage |
|---|---|---|
| Primary | #071640 | Buttons, CTAs, active nav, links, active tab underline+text, KPI accent number |
| Primary Hover | #15254d | Button hover, interactive hover states |
| Primary Soft | #EEF0F6 | Tinted surface, selected row background |
| Primary Tint | #D8DCE6 | Subtle accent background |

### Semantic

| Name | Hex | Soft | Usage |
|---|---|---|---|
| Success | #059669 | #ECFDF5 | Positive states, completed, approved |
| Warning | #D97706 | #FFFBEB | Caution, pending states |
| Danger | #DC2626 | #FEF2F2 | Destructive actions, errors, delete |
| Info | #2563EB | #EFF6FF | Informational, syncing states |

### Neutral (gray — Tailwind default, giữ nguyên từ v2)

| Hex | Usage |
|---|---|
| #F9FAFB | Alternate row background, input background (gray-50) |
| #F3F4F6 | App background (gray-100) |
| #E5E7EB | Border default, card edge (gray-200) |
| #D1D5DB | Border strong, input border (gray-300) |
| #9CA3AF | Muted text, placeholder (gray-400) |
| #6B7280 | Secondary text — medium (gray-500) |
| #4B5563 | Secondary text (gray-600) |
| #374151 | — (gray-700) |
| #1F2937 | Accent, dark text (gray-800) |
| #111827 | Deepest foreground text / dialog overlay backdrop (gray-900) — KHÔNG dùng cho Sidebar, xem Surfaces |

### Surfaces

| Name | Hex | Usage |
|---|---|---|
| App background | #F3F4F6 | Page background |
| Card | #FFFFFF | Card, panel, dialog surfaces |
| Alt surface | #F9FAFB | Alternate rows, input fill |
| Sunken | #F3F4F6 | Recessed areas, disabled input bg |
| Sidebar | #162040 | Sidebar shell (navy — đậm hơn Primary `#071640`, KHÔNG phải gray-900) |

### Borders

| Name | Hex | Usage |
|---|---|---|
| Border | #E5E7EB | Default divider, card edge, table border |
| Border Strong | #D1D5DB | Input border, stronger separator |
| Divider | #F3F4F6 | Subtle section divider |

### Category / Accent Colors

Dùng cho tag, pill, data series, theme phụ. 1 accent / surface. Lưu ý: Indigo ở đây là **accent color tuỳ chọn**, không phải Primary (Primary luôn là navy `#071640`).

| Name | Hex |
|---|---|
| Teal | #0D9488 |
| Rose | #E11D48 |
| Purple | #9333EA |
| Amber | #F59E0B |
| Indigo | #4F46E5 |

---

## Typography

### Font Family

| Token | Value |
|---|---|
| Font Family/Headings | Inter |
| Font Family/Body | Inter |
| Font Family/Mono | IBM Plex Mono (số, ID, code) |

### Font Weight

| Token | Value |
|---|---|
| Regular | 400 |
| Medium | 500 |
| Semi Bold | 600 |
| Bold | 700 |

### Headings

| Style | Size | Line Height | Weight |
|---|---|---|---|
| Headings/H1 | 60px | 72px | Semi Bold (600) |
| Headings/H2 | 48px | 56px | Semi Bold (600) |
| Headings/H3 | 40px | 48px | Semi Bold (600) |
| Headings/H4 | 32px | 40px | Semi Bold (600) |
| Headings/H5 | 24px | 28px | Semi Bold (600) |
| Headings/H6 | 20px | 24px | Semi Bold (600) |

> Page title (H1 admin) = 24px / 32px / Medium (500), tracking-tight — chốt 2026-07-06 (BA Lead điều chỉnh xuống từ 30px DOM thật, giống tinh thần quyết định Navy/Indigo: giữ font-weight/style theo DOM nhưng size là lựa chọn thẩm mỹ có chủ đích, không nhân bản y hệt DOM). Thay thế cả giá trị Body/lg-semibold (20px) cũ VÀ giá trị 30px tạm chốt trước đó cùng ngày. H2 admin = Body/md-semibold (16px) — giữ nguyên, không đổi.

### Body

| Style | Size | Line Height | Weight | Decoration |
|---|---|---|---|---|
| Body/lg | 20px | 24px | Regular (400) | — |
| Body/lg-semibold | 20px | 24px | Semi Bold (600) | — |
| Body/md | 16px | 20px | Regular (400) | — |
| Body/md-semibold | 16px | 20px | Semi Bold (600) | — |
| Body/sm | 14px | 16px | Regular (400) | — |
| Body/sm-semibold | 14px | 16px | Semi Bold (600) | — |
| Body/xsm | 12px | 16px | Regular (400) | — |
| Body/xsm-semibold | 12px | 16px | Semi Bold (600) | — |

**Default body text:** Body/sm (14px). Body/xsm (12px) cho meta/label/helper.

### Usage mapping trong admin UI

| Context | Style |
|---|---|
| Page title | 24px / 32px / Medium (500), tracking-tight — nằm trong Page Header (Content Body), KHÔNG nằm trong Topbar. Chốt 2026-07-06 (BA Lead điều chỉnh xuống từ 30px DOM thật) — xem note đầu mục Headings. |
| Section header | Body/md-semibold (16px 600) |
| Card title | Body/md-semibold (16px 600) |
| Body text, table cell | Body/sm (14px 400) |
| Form label (uppercase) | Body/xsm-semibold (12px 600) |
| Caption, meta, timestamp | Body/xsm (12px 400) |
| KPI / stat number | Body/lg-semibold + IBM Plex Mono, màu Primary `#071640` |
| ID / code | IBM Plex Mono 12px |

---

## Spacing

4px base grid — mọi padding/gap/margin phải là bội số 4px.

| Token | Value |
|---|---|
| s-1 | 4px |
| s-2 | 8px |
| s-3 | 12px |
| s-4 | 16px |
| s-5 | 20px |
| s-6 | 24px |
| s-8 | 32px |
| s-10 | 40px |
| s-12 | 48px |
| s-16 | 64px — Topbar height |
| s-20 | 80px |

---

## Border Radius

| Token | Value | Usage |
|---|---|---|
| none | 0px | Table cells, dividers |
| xs | 2px | Micro elements |
| sm | 4px | Tags, badges |
| md | 8px | Buttons, inputs, nav items |
| lg | 12px | Cards |
| xl | 16px | Dialogs |
| 2xl | 24px | Large modals |
| pill | 100px | Pills, toggles |

---

## Shadows / Elevation

| Level | Value | Usage |
|---|---|---|
| xs | `0 1px 2px rgba(15,23,42,.05)` | Subtle row hover |
| sm | `0 1px 3px rgba(15,23,42,.06), 0 1px 2px rgba(15,23,42,.04)` | Card default |
| md | `0 4px 12px rgba(15,23,42,.08)` | Card hover |
| lg | `0 10px 28px rgba(15,23,42,.10)` | Dropdown/popover |
| xl | `0 20px 50px rgba(15,23,42,.14)` | Modal/dialog |
| brand | `0 6px 18px rgba(7,22,64,.28)` | Primary CTA button (navy shadow) |

---

## Layout

| Element | Value |
|---|---|
| Sidebar width | 280px |
| Sidebar (compact) | 112px |
| Sidebar (thin) | 80px |
| Topbar height | 64px |
| Content max-width | 1440px |

---

## Icons

- Style: Material Outlined / Feather-style SVG, 24×24, stroke-weight 2
- Color: inherits currentColor. Không dùng emoji.

---

## Component Visual Specs

### Card
- Bg #FFFFFF, border 1px #E5E7EB, radius 12px, padding 20px
- Optional 3px top-border accent (category color)
- Dark variant: navy `#071640` cho hero card
- Hover: shadow sm→md

### Button
- Radius: 8px default, 6px square, 100px pill
- Hover: darken only, không scale/lift
- Primary CTA shadow: `0 6px 18px rgba(7,22,64,.28)`
- **Sizing — concept chung, áp dụng MỌI Button có icon+label (chốt 2026-07-06):**
  - **Width = Hug contents (auto) — KHÔNG BAO GIỜ fix cứng width bằng số tay.** Lỗi thực tế đã xảy ra: 1 Button fix width=110px trong khi icon(16) + gap + label(62) + padding ngang(16×2) cộng lại = 116px → lệch đúng 6px, hỏng layout khi label đổi text. Luôn để auto-layout tự tính theo nội dung.
  - Padding ngang (trái/phải): 16px — mọi variant, mọi height.
  - Gap giữa icon và label: **8px** (token `spacing/md` — xem §Spacing, dùng đúng scale hệ thống 2/4/8/12/16). KHÔNG dùng giá trị tự do như 6px (lỗi thực tế đã gặp).
  - Icon size: 16px.
- **Height theo ngữ cảnh — 3 variant đã chốt:**
  - **Standard = 44px** — mọi nút hành động chính, đứng riêng hoặc trong toolbar không kèm filter dense. Bằng Standard Input (Form Fields §Standard).
  - **Filter-bar = 36px** (chốt 2026-07-06) — dùng khi Button nằm cùng hàng Filter Bar, cạnh input Dense — khớp `Dense filter: 36px height` (Form Fields) để canh hàng đều với input.
  - **`.btn-sm` = 30px** — nút nhỏ trong bảng/toolbar dày đặc, không đổi.
  - Đã sửa đồng bộ theo các quyết định trên: `design-reference.html` `.btn` height 36px→44px và gap 6px→8px (gap cũ off-scale). Screen SCR-02 đã vẽ dùng 40px vẫn là **sai theo chuẩn Standard 44px**, cần sửa lại khi có dịp chỉnh sửa screen đó (chưa retroactive fix ngay).

### App Shell
- Sidebar: full height 100vh, cột trái, logo+nav, KHÔNG bị topbar che
- Content: cột phải, topbar (trên) + body (scroll)

### Sidebar Navigation
- Background: navy `#162040` (đậm hơn Primary `#071640` — 2 tông navy tách biệt theo vai trò, không phải cùng 1 giá trị)
- Width 280px, full height
- Logo: bonbon shop wordmark trên cùng trong sidebar (KHÔNG phải trong topbar). **Cập nhật 2026-07-06 — đã có asset thật:** dùng `import_image` với `.claude/skills/design-instruction/brand-portal/assets/logo-wordmark-dark.svg` (chữ trắng + cam #EF4429, khớp nền navy #162040 của Sidebar) làm mặc định — xem SKILL.md §Logo zone. Kỹ thuật text 2 màu "colored-o" hạ xuống fallback cuối cùng, chỉ dùng khi `import_image` thất bại.
- Accordion: parent (icon+label+chevron) → sub-item thụt lề 48px
- Sub-item active: nền `rgba(255,255,255,.1)`, text trắng semi-bold
- Hover: `rgba(255,255,255,.05–.06)`
- **Không tự bịa item cấp 1 mới:** phải xác nhận feature thuộc nhóm cấp 1 nào đã tồn tại (vd VisibilityPRO → nhóm "Quản lý Chấm hình") qua `.claude/skills/design-instruction/brand-portal/screenshots/Quản lý chấm hình.png` hoặc ảnh thật BA cung cấp — sai = `MENU_ITEM_GAP`

### Topbar
- Height 64px, trắng, shadow-sm
- LEFT: hamburger. RIGHT: icon/language·fullscreen·user(icon+email) — gap đồng nhất 8px, **KHÔNG có Divider** (đã verify từ DOM thật, xem app-shell-reference.md §2; bản cũ ghi "divider" là sai, đã sửa 2026-07-06)
- ⚠️ **Sai lầm đã xảy ra thật (SCR-01, 2026-07-04):** dùng `icon/flag-vi` cho vị trí đổi ngôn ngữ — user xem ảnh thật xác nhận icon đó chỉ là Material Icon "flag" (lá cờ hiệu/pennant chung chung), KHÔNG phải quốc kỳ Việt Nam như tên layer ngụ ý, và cũng không khớp bản đã verify (SOS Dashboard dùng `icon/language` — globe). Luôn dùng `icon/language`, không dùng bất kỳ biến thể "flag" nào cho vị trí này.
- 🔍 **Update 2026-07-05 (đã ghi nhận, CHƯA đổi rule):** DOM export thật của Portal (màn Danh sách kế hoạch đánh giá) cho thấy vị trí này thực ra dùng ảnh cờ quốc gia thật (`assets/images/flags/VN.svg`), không phải icon globe. BA Lead quyết định giữ nguyên `icon/language` cho lần vẽ tới — coi đây là điểm Portal thật cần sửa trong tương lai, không phải chuẩn để nhân bản ngay. Nếu BA Lead đổi quyết định sau này, cập nhật lại rule này. Xem Changelog v5.
- **KHÔNG BAO GIỜ chứa Page Title** — title nằm ở Page Header (đầu Content Body)
- **Layout kỹ thuật:** nếu Topbar dùng auto-layout HORIZONTAL với `SPACE_BETWEEN`, PHẢI gom toàn bộ cụm bên phải (language+fullscreen+user, gap 8px, KHÔNG divider) vào **1 frame con** trước khi đặt vào Topbar — nếu để rời từng icon làm children trực tiếp, `SPACE_BETWEEN` sẽ dàn đều TẤT CẢ icon ra khắp chiều ngang thay vì neo gọn 1 cụm bên phải.

### Tabs
- Active: underline **full-width theo tab cell** (không theo width chữ), flush sát đáy tab, màu navy `#071640` + text navy 600
- Inactive: text gray, không border
- **Lỗi thường gặp cần tránh:** nếu Tab dùng auto-layout hug-height (tự co theo nội dung), thêm underline vào sẽ làm tab đó THẤP HƠN các tab khác (lệch hàng) — phải ép `primaryAxisSizingMode: FIXED` + đúng height bằng các tab khác (thường 48px) TRƯỚC khi thêm underline.

### Form Fields / Inputs
- Default: 1px #E5E7EB border, trắng, radius 8px
- Focus: navy border + inset navy shadow
- Error: `#DC2626` border + message đỏ dưới field
- Dense filter: 36px height. Standard: 44px height

### Floating Label (Material outline pattern) — BẮT BUỘC cho Text Input / Dropdown / Date Picker

Verify từ ảnh thật Portal (2026-07-04) — field KHÔNG dùng "label tĩnh phía trên" như quy ước cũ, mà dùng **floating label kiểu Material outline**: label nằm lồng trong field khi trống, nổi lên đè viền trên khi có giá trị/đang focus.

**2 state của LABEL (độc lập với state Default/Focus/Error/Disabled của field):**

| Label state | Khi nào | Vị trí | Size/màu |
|---|---|---|---|
| Resting (placeholder) | Field trống, không focus | Giữa field theo chiều dọc, x=12 | 14px, `#9CA3AF` |
| Floating | Field CÓ giá trị, hoặc đang focus (kể cả khi trống) | Đè lên cạnh viền trên field, x=12, y = -8 so với viền trên | 12px — focus: `#071640` (navy) · có giá trị không focus: `#6B7280` |

Khi label ở trạng thái Floating: value/nội dung field hiển thị bên trong như text bình thường (14px, `#111827`), nằm **dưới** vị trí label — không phải giữa field nữa.

**Cách vẽ trong Figma — kỹ thuật ĐÃ TEST THÀNH CÔNG trên SOS Dashboard SCR-01 (2026-07-04), cho đúng hiệu ứng "label cắt ngang viền trên" (verify từ HTML thật `mat-select` Angular Material — field KH dùng đúng chuẩn MDC outline notched-label):**

Field frame trong figma-mcp-go **luôn là auto-layout** (kể cả khi trông giống frame tĩnh) — thêm text con mới trực tiếp vào field bằng toạ độ x/y thủ công sẽ KHÔNG có tác dụng, node bị auto-layout tự động xếp lại vị trí theo flow (xem SKILL.md mục 6.9 #8). Không thể chèn label overlay thẳng vào field. Kỹ thuật đúng: bọc field trong 1 **Wrapper frame KHÔNG auto-layout** (`layoutMode: NONE`) — bên trong wrapper thì x/y thủ công hoạt động bình thường (theo mục 6.9 #1), nên có thể đặt label đè lên viền thật.

1. Tạo **Field Wrapper** (frame trong suốt, `layoutMode: NONE`, không border/fill), kích thước = rộng bằng field, **cao hơn field 8px** (vd field cao 44 → wrapper cao 52) — 8px dư này chừa chỗ phía trên để label "ngoi lên" khỏi field.
2. `reparent_nodes` field gốc (còn nguyên border, radius, Value+icon bên trong) vào Wrapper, rồi `move_nodes` field về `x=0, y=8` (dịch xuống 8px so với đỉnh wrapper) — viền trên của field giờ nằm đúng tại `y=8` trong toạ độ wrapper.
3. Tạo text Label (12px) làm con của Wrapper tại `x=12, y=1` — label cao ~15px nên trải từ y=1 đến y=16, **cắt ngang qua đường viền tại y=8** đúng hiệu ứng notched label.
4. Tạo 1 rectangle "Notch BG" cùng màu nền field (`#FFFFFF`) làm con của Wrapper, tại `x=8, y=7, height=2, width=label_width+8` — đè đúng lên đoạn viền nằm dưới label để "xoá" đoạn đó, tạo ảo giác viền bị ngắt quãng quanh label.
5. Thứ tự layer bắt buộc (dùng `reorder_nodes` nếu cần): Field (dưới cùng) → Notch BG (giữa) → Label (trên cùng). Field tạo trước sẽ tự ở dưới; Notch BG và Label tạo sau — nếu Notch BG bị tạo sau Label thì phải `sendBackward` 1 lần để nó xuống dưới Label nhưng vẫn trên Field.
6. Màu Label theo state: có giá trị + không focus → `#6B7280`; đang focus → `#071640` (navy). Value chính bên trong field giữ nguyên vị trí gốc (giữa field theo chiều dọc, 14px, `#111827`), KHÔNG cần dịch chuyển gì thêm vì Label nằm ở Wrapper (bên ngoài field), không chiếm chỗ bên trong field.
7. Nếu field trống/chưa chọn (Empty state): KHÔNG tạo Wrapper/Label/Notch — field render bình thường với placeholder `#9CA3AF` nằm giữa (label sẽ hiện dạng Resting — xem bảng State ở trên).

> Lưu ý quan trọng: kỹ thuật "gộp Label vào bên trong field, xếp chồng theo chiều dọc (VERTICAL layout)" — từng thử trước đó trong cùng ngày — là **kỹ thuật SAI**, không đúng hiệu ứng floating label thật (label nằm gọn trong field, không đè viền). Đã loại bỏ, thay bằng kỹ thuật Wrapper ở trên.

**Sai lầm đã xảy ra thật (SOS Dashboard SCR-01, 2026-07-04) — ĐÃ SỬA 2 lần:** (1) ban đầu gộp label + value thành 1 dòng text duy nhất trong field (vd `"TM Program: 2 đã chọn"`); (2) sau khi tách label ra, đặt label xếp chồng bên trong field (VERTICAL layout) thay vì đè lên viền — user xem trực tiếp ảnh Portal thật (HTML `mat-select`) và chỉ ra sai. Đã sửa đúng theo kỹ thuật Wrapper ở trên, áp dụng cho cả 4 field: TM Program, Date Range, Hierarchy, Brand.

---

## Border giữa các khối liền kề — tránh viền đôi

Nhiều component (Topbar, Tab Bar, Filter Bar, Card, Table) đều có stroke 4 cạnh mặc định (giới hạn `set_strokes` của figma-mcp-go — không tách được border 1 cạnh). Khi 2 khối xếp chồng trực tiếp (không có gap/padding ở giữa) và CẢ HAI đều có stroke, cạnh chung sẽ hiện thành viền dày gấp đôi (lỗi đã xảy ra thật giữa Tab Bar và Filter Bar, 2026-07-04).

**Quy ước bắt buộc — chỉ 1 trong 2 khối liền kề được giữ border tại cạnh chung:**

| Cặp khối liền kề | Ai giữ border | Ai bỏ border |
|---|---|---|
| Topbar → Page Header | Topbar (border-bottom) | Page Header: không border |
| Page Header → Tab Bar | — (thường không chạm trực tiếp) | — |
| Tab Bar → Filter Bar | Tab Bar (border-bottom, đóng vai trò divider dưới tab) | Filter Bar: **không border** |
| Filter Bar → Cards Container | — (Cards Container có padding, không chạm trực tiếp) | — |
| Card / Table trong Cards Container | Giữ border riêng bình thường | (không áp sát khối khác nên không bị đôi) |

Nếu thêm section mới xen giữa, luôn kiểm tra: 2 khối có border ở cạnh chạm nhau không? Nếu có → bỏ 1 bên theo nguyên tắc "khối phía trên giữ, khối phía dưới bỏ" trừ khi bảng trên ghi khác.

---

## Empty State

**Token cấp component** (đã có sẵn): Glyph 64×64px, radius 12px, bg `#F3F4F6`; icon color `#9CA3AF`; message tone operational (không marketing); CTA button nếu user có thể thực hiện hành động ngay.

**Layout structure (mới, chốt 2026-07-06 — lỗ hổng phát hiện từ CSS thật, xem lý do bên dưới):**
- Container Empty State: `direction VERTICAL`, `align-items CENTER`, `justify-content CENTER` — chiếm full width/height của khu vực dành cho Empty State (căn giữa cả ngang lẫn dọc). **TUYỆT ĐỐI KHÔNG** dùng `position: absolute` cho glyph/title/description — đây là lỗi thật đã xảy ra (xem ghi chú dưới).
- Thứ tự trên xuống: Glyph (64×64) → gap **16px** → Title → gap **8px** → Description → (nếu có CTA) gap **16px** → Button.
- Title: `text-align: center`.
- Description: `text-align: center`, `max-width: 320px` (để wrap tự nhiên thành 2 dòng thay vì tràn ngang hết container).

> **Lỗi thật đã xảy ra (2026-07-06):** CSS Figma export cho Empty State có cả 3 layer (glyph, title, description) đặt `position: absolute; left: 0; top: 0` — tức cùng 1 toạ độ, đè chồng lên nhau hoàn toàn → render ra chữ/icon vỡ, chồng chéo không đọc được (phát hiện qua screenshot thật). Nguyên nhân gốc: có token màu/kích thước cho Empty State nhưng **chưa có rule layout/structure** — đã bổ sung ở trên để chặn lặp lại lỗi này.

---

## Table — Data Row Content

> Bổ sung cho Table quick-ref ở `SKILL.md` §6.6 (row height 64px, zebra bg, border) và rule "Table header/data alignment" (Quality Control) — 2 chỗ đó lo phần khung (height/bg/width), phần này lo phần **nội dung bên trong cell**.

- **Vertical align:** Nội dung mỗi cell (text/badge/toggle/icon) căn giữa theo chiều dọc trong row height 64px — tương đương `vertical-align: middle` (đã dùng đúng kỹ thuật này trong `app-layout.html` demo, giờ chốt thành rule chính thức bắt buộc cho mọi Table mới vẽ).
- **Padding ngang:** Data Row container dùng đúng `padding: 0px 16px` — **giống hệt** Header Row (không đặt padding riêng theo từng cell). Đây là điều kiện cần để rule "Table header/data alignment" (width khớp theo index) thực sự cho ra kết quả thẳng hàng — width khớp mà padding gốc lệch vẫn bị lệch cột.
- **Truncation (cột fixed-width bị tràn nội dung):** dùng truncate 1 dòng (`text-overflow: ellipsis; white-space: nowrap; overflow: hidden`) — **KHÔNG wrap 2 dòng** (sẽ đẩy row cao hơn 64px, phá zebra/border đồng nhất). Ưu tiên giải pháp gốc là để cột nội dung dài (tên, mô tả) flexible/auto (xem `dom-reference-list-screen-pattern.md` §3.1) — chỉ dùng truncate khi cột buộc phải fixed-width (cột định danh/mã, cột control) mà nội dung vẫn dài hơn width.

---

## Data Display Rules

- Currency: `482,500,000 ₫`. Số lớn: `2.48 tỷ ₫`
- Date: `DD/MM/YYYY`
- Số liệu: IBM Plex Mono, tabular figures, right-aligned trong table
- Badge: pill radius 100px, màu theo semantic palette (không đổi qua các version)

---

## Design Principles

- Navy `#071640` là Primary duy nhất (không phải indigo — đã chốt lại ở v3)
- Sidebar dùng tông navy riêng `#162040`, đậm hơn Primary
- Red `#DC2626` chỉ cho destructive/warn
- 1 accent color / surface — không mix màu
- App bg #F3F4F6, card #FFFFFF, không gradient/photography trong core product
- 4px spacing grid xuyên suốt

---

## Nguồn xác nhận & mức độ tin cậy

| Token | Nguồn | Ghi chú |
|---|---|---|
| Primary #071640 / Sidebar #162040 | Quyết định thẩm mỹ trực tiếp của user (2026-07-04, 3 vòng feedback) | Ghi đè finding CSS thật (đo được indigo `#4F46E5`/gray-900 `#111827`) — xem Changelog v3 |
| Neutral gray-50→900 | Suy ra từ Tailwind/Fuse default, giữ nguyên từ v2 | [cần verify thêm nếu có ảnh thật mới] |
| Topbar layout (hamburger trái, cụm phải, không title) | Verify từ HTML/ảnh thật "Danh sách kế hoạch đánh giá" | Verified, không đổi qua các version |
| Sidebar IA (tên nhóm/sub-item) | Screenshots cũ + ảnh thật user cung cấp | Cấu trúc dùng được, màu không dùng được (ảnh cũ theme khác) |
| Sidebar IA nhóm "Quản lý Chấm hình" (7 sub-item) | DOM export thật, màn Danh sách kế hoạch đánh giá (2026-07-05) | **Verified 100%**, khớp đúng quyết định SCR-04 đã chọn — xem `app-shell-reference.md` |
| Badge màu trạng thái kế hoạch ("Thực thi"=`badge-blue`) | DOM export thật (2026-07-05) | Verified — khác thang màu Status badge generic ở SKILL.md §6.6, dùng riêng cho Evaluation Plan |

**Khi vẽ feature mới:** dùng đúng token navy trong file này làm chuẩn hiện hành. Nếu CSS/ảnh thật cho ra màu khác — RAISE hỏi BA Lead, không tự sửa theo CSS (xem cảnh báo đầu file).
