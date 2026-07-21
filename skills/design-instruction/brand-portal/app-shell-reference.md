# App-Shell Reference — Sidebar / Topbar / Logo / Icon Library

> Trích xuất từ 2 lần DOM export thật của Portal (route `/visibility/evaluation-ai-plan` và `/product/product-ai`, 2026-07-05).
> **Phạm vi: TOÀN PORTAL** — Sidebar, Topbar, Logo, icon library là app-shell dùng chung cho MỌI màn hình, không riêng màn nào. Đọc file này ở Step 5 (bắt buộc), áp dụng cho bất kỳ screen nào có full app shell.
> Nguồn: `Angular 17.2.2`, framework `Fuse 19.0.0` (Material + Tailwind).

---

## 1. Sidebar — cấu trúc đầy đủ, đã verify 100%

Thứ tự nhóm cấp 1 từ trên xuống (tất cả đều `fuse-vertical-navigation-collapsable-item`, mặc định collapsed trừ khi ghi chú):

| # | Tên nhóm (label) | Icon namespace | Icon name | Trạng thái | Route pattern |
|---|---|---|---|---|---|
| 1 | Quản lý sản phẩm | feather | box | collapsed | — |
| 2 | Quản lý cửa hàng | (raw svg) | storefront | collapsed | — |
| 3 | Quản lý thông báo | (raw svg) | notifications_active | collapsed | — |
| 4 | Quản lý Trade marketing | (raw svg) | assessment | collapsed | — |
| 5 | **Báo cáo** | heroicons_mini | chart-pie | **expanded** | xem 1a |
| 6 | Cài đặt chung | mat_solid | settings | collapsed | — |
| 7 | Quản lý chụp ảnh | (raw svg) | add_a_photo | collapsed | — |
| 8 | **Quản lý Chấm hình** | (raw svg) | photo_library | **expanded** | xem 1b — ✅ khớp đúng quyết định SCR-04 |
| — | *(group label, không phải item)* "Quản lý" | — | — | — | `fuse-vertical-navigation-group-item`, chỉ là divider/label, không click được |
| 9 | Quản lý tài khoản | (raw svg) | person | collapsed | — |

Chevron dùng `heroicons_solid` "chevron-right" ở mọi item cấp 1 (kể cả khi expanded — DOM không đổi icon, chỉ đổi class `fuse-vertical-navigation-item-expanded`).

**Áp dụng khi vẽ:** đây là allowlist đầy đủ 9 nhóm cấp 1 hiện có trong Portal thật. Bất kỳ feature mới nào cần thêm nav item phải thuộc 1 trong 9 nhóm này (hoặc raise `MENU_ITEM_GAP`/`DESIGN_GAP` theo 2-tier navigation authority nếu cần nhóm cấp 1 mới).

### 1a. Sub-items nhóm "Báo cáo"

| Label | Route |
|---|---|
| Báo cáo chi tiết kết quả hình trưng bày | `/report/report-of-photo-result-product` |
| Báo cáo chi tiết kết quả lượt chụp | `/report/reports-of-capture-results` |
| Báo cáo chi tiết kết quả chấm hình thủ công | `/report/report-of-manual-evaluation` |

### 1b. Sub-items nhóm "Quản lý Chấm hình" — ✅ verified khớp SCR-04

| Label | Route | Trạng thái trong DOM |
|---|---|---|
| Danh sách kế hoạch đánh giá | `/visibility/evaluation-ai-plan` | **active** (`fuse-vertical-navigation-item-active`) |
| Danh sách chấm hình | `/visibility/evaluation-program` | — |
| Tiêu chí chấm hình | `/visibility/rule` | — |
| Danh sách nguyên tắc trưng bày | `/visibility/display-principle` | — |
| Cài đặt lý do | `/visibility/reason-reject` | — |
| Danh sách kế hoạch hậu kiểm | `/visibility/post-check-program` | — |
| Danh sách kế hoạch xét thưởng | `/visibility/reward-program` | — |

**Kết luận:** quyết định "mở rộng sidebar MASTER thêm nhóm Quản lý Chấm hình" (đã chọn khi vẽ SCR-04, 2026-07-04) là **đúng 100%** với ground truth thật.

### 1d. Sub-items nhóm "Quản lý sản phẩm" (verified 2026-07-05, DOM 2)

| Label | Route |
|---|---|
| Sản phẩm AI nhận diện | `/product/product-ai` |
| Nhóm sản phẩm trưng bày | `/product/product-ai-group` |

> Sub-items của 6 nhóm còn lại (Quản lý cửa hàng, Quản lý thông báo, Quản lý Trade marketing, Cài đặt chung, Quản lý chụp ảnh, Quản lý tài khoản) **chưa được export** — nhóm này để collapsed trong cả 2 lần capture. Chỉ cần export thêm nếu BA sắp làm feature thuộc các nhóm đó; không urgent cho scope VisibilityPRO hiện tại (nhóm liên quan — "Quản lý Chấm hình" — đã full 100%).

### 1c. Logo zone (sidebar header)

```html
<img class="dark:hidden static w-30 h-auto" src="assets/images/logo/logo-text.png" alt="Logo image">
<img class="hidden dark:flex static w-30 h-auto" src="assets/images/logo/logo-text.png" alt="Logo image">
```
- 2 biến thể (light/dark mode) nhưng **cùng 1 file** `logo-text.png` — không phải wordmark vector dựng bằng text.
- Kích thước hiển thị trong DOM: width 198px, height 69px (trước khi Tailwind `w-30 h-auto` scale lại).
- Container: `flex items-center h-20 p-6 pb-0`.
- **Trạng thái áp dụng (cập nhật 2026-07-06):** asset vector thật đã có — user cung cấp trực tiếp 2 biến thể màu từ Figma component gốc (`Type=bonbonshop1` light, `Type=bonbonshop2` dark), lưu tại `.claude/skills/design-instruction/brand-portal/assets/logo-wordmark-light.svg` và `logo-wordmark-dark.svg`. Sidebar (nền navy #162040) dùng bản `-dark` (chữ trắng + cam #EF4429). SKILL.md §Logo zone đã cập nhật: `import_image` asset thật là mặc định, kỹ thuật "colored-o" 2 màu hạ xuống fallback cuối cùng. Các placeholder cũ `logo-mark.svg`/`logo-wordmark.svg` (generic "BP"/"Brand Portal", không phải branding BonBonShop thật) đã bị xoá khỏi `assets/`.

---

## 2. Topbar — cấu trúc đầy đủ (áp dụng mọi màn hình chính)

```
Topbar: h-16 (64px), flex items-center, px-4/px-6, shadow, bg-card, border-b (dark mode)
├── LEFT: hamburger button (heroicons_outline "bars-3")
└── RIGHT (flex items-center pl-2 ml-auto space-x-2):
    ├── <languages> component: button hình vuông w-6, bo góc sm, chứa
    │     <img src="assets/images/flags/VN.svg" alt="Flag image for vi">
    │   → ⚠️ ẢNH CỜ QUỐC GIA THẬT, không phải icon "globe"/"language" trừu tượng.
    │     (design-tokens.md Changelog v5 — BA Lead quyết định GIỮ NGUYÊN rule
    │     icon/language cho lần vẽ tới, không đổi theo finding này.)
    ├── <fuse-fullscreen>: heroicons_outline "arrows-pointing-out", tooltip "Toggle Fullscreen"
    └── <user>: mat-flat-button (KHÔNG phải icon-only) gồm:
          heroicons_outline "user-circle" + text email (span, max-w-40, truncate)
          → Nút có LABEL TEXT (email), không phải chỉ icon/avatar tròn.
```

**Không có divider `|` explicit trong DOM** giữa cụm icon phải — spacing đều `space-x-0.5 sm:space-x-2`, không phải 1 stroke/line phân cách.

---

## 3. Icon library thật — tham chiếu nhanh (toàn Portal)

| Icon dùng cho | Namespace/loại |
|---|---|
| Nav "Quản lý sản phẩm" | feather (svg path) |
| Nav "Báo cáo" | heroicons_mini (svg path) |
| Nav chevron | heroicons_solid |
| Nav "Cài đặt chung" | mat_solid (svg path, khác font ligature) |
| Topbar hamburger/fullscreen/user | heroicons_outline |
| Table row menu (3 chấm) | font ligature `material-icons` (`mat-ligature-font`), text = tên icon (`more_vert`) |
| Action button phụ (vd "Cập nhật danh sách cho AI") | font ligature `material-icons` (`mat-ligature-font`), text = tên icon (`file_upload`) — xác nhận thêm: material-icons font ligature dùng cho NHIỀU action button, không chỉ riêng menu 3 chấm |
| Đa số icon cấp 1 sidebar còn lại | raw inline SVG path riêng (không rõ thư viện gốc, không phải font) |

**Kết luận cho design-agent:** hầu hết icon KHÔNG phải font "Material Icons" ligature — chỉ 1 icon xác nhận dùng font ligature thật (`more_vert`). figma-mcp-go vẫn nên dùng quy tắc thực dụng ở §Icon System (font Material Icons nếu có, rectangle fallback nếu không) vì không thể import chính xác nhiều thư viện SVG khác nhau theo tên — đây là ghi chú tham chiếu, không phải hướng dẫn đổi cách vẽ.
