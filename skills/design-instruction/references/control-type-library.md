# Control Type Library — VisibilityPRO Portal

> Single source of truth cho mọi UI control type + **default behaviour** của VisibilityPRO Portal.
> Mỗi spec / màn hình **kế thừa** behaviour + states từ đây. Chỉ mô tả thêm khi **KHÁC default**.
> Sở hữu: Lead BA. Nguồn token: `design-instruction/SKILL.md` §6.6/§6.7 + design-tokens.md.
> Nguồn cơ chế: ba-kit-solo `control-type-library-template.md` (đọc verbatim 2026-07-03) — port format, thay token + domain sang VisibilityPRO.

## Cách sử dụng (deviation model)

Trong spec (Section I — Bảng mô tả trường) và design-instruction, cột Behaviour/Ràng buộc:

| Cách ghi | Ý nghĩa |
|----------|---------|
| `(default)` | Kế thừa toàn bộ Default Display + Behaviour + States từ file này |
| `(default), thêm: {mô tả}` | Kế thừa + bổ sung edge case |
| `**Khác default:** {mô tả}` | Override hoàn toàn |

→ Mục tiêu: spec **không tả lại** focus/validation timing/disabled/toast mỗi field; chỉ ghi deviation. Ước giảm 50–70% khối lượng field table `[giả định — đo sau 3 spec đầu]`.

## Baseline

| Trường | Giá trị |
|--------|---------|
| Thư viện UI | `none` (VisibilityPRO Portal component set là SSOT đầy đủ) |
| Token nguồn | design-tokens.md + SKILL §6.6/§6.7 |
| Owner | Lead BA |

> Baseline = `none` → toàn bộ control type bên dưới là **source of truth đầy đủ**. Nếu sau này Portal chuẩn hoá sang 1 thư viện (vd Shadcn/Ant) → chuyển sang **deviation log** so với docs thư viện (xóa behaviour trùng default).

---

## Terminology (chuẩn toàn dự án — hard gate ngôn ngữ)

> Reviewer chặn Blocker nếu spec/story/design dùng **từ cấm** trong phần mô tả hành vi.

### Hành động người dùng
| Từ chuẩn | Từ cấm |
|----------|--------|
| ấn | bấm, nhấn, nhấp, click, tap |
| chọn | select, pick, lựa |
| điền | nhập, gõ, type, enter |
| kéo | drag, kéo thả |
| vuốt | swipe, scroll (mobile gesture) |
| giữ | hold, long-press |

### Điều hướng
| Từ chuẩn | Từ cấm |
|----------|--------|
| mở | open, navigate to, redirect, chuyển đến, đến, vào |
| đóng | close, dismiss |
| quay về | go back, return, back |

### Hiển thị
| Từ chuẩn | Từ cấm |
|----------|--------|
| hiện | show, display, xuất hiện, render |
| ẩn | hide, disappear |
| bật | on, enable, activate |
| tắt | off, disable, deactivate |

### Xử lý dữ liệu
| Từ chuẩn | Từ cấm |
|----------|--------|
| lưu | save, persist, store |
| xóa | delete, remove, clear |
| kiểm tra | validate, check |
| xác thực | authenticate, verify, auth |
| tìm | search, query, filter |
| tải lên / tải xuống | upload / download |
| gửi | send, submit |
| hủy | cancel, abort, discard |

### Business-language hard gate
Behaviour trong spec/design chứa **API path / HTTP method / tên hàm / tên bảng DB** → vi phạm `BEHAVIOUR_FORMAT_VIOLATION` (reviewer Blocker).
- ✅ `ấn Lưu → kiểm tra → hợp lệ: hiện toast MSG-SUC-01, mở màn Danh sách`
- ❌ `Click Save → POST /api/plans`

---

## Control Types (VisibilityPRO Portal)

> Token dùng chung: Primary `#071640` · Sidebar `#162040` · Border `#E5E7EB`/`#D1D5DB` · Error `#DC2626` · Success `#059669` · Font Inter, số IBM Plex Mono · h ô/nút 44px · radius nút/ô 8px, tag 4px, card 12px, dialog 16px, pill 100px.

### 1. Text Input (`text_input`)
**Default Display:** Floating label kiểu Material outline (xem `design-tokens.md` §Floating Label) — label nằm lồng trong field khi trống, nổi đè viền trên khi có giá trị/focus. Viền `#D1D5DB`, h44, radius8.
**Default Behaviour:** Ấn vào → viền focus `#071640`. Rời ô → tự kiểm tra, lỗi hiện **inline dưới field** (không toast). Nhiều lỗi → field gần đầu form báo trước.
**Default States:** default (viền xám, trống) · focused (viền navy) · filled · disabled (`#F3F4F6`, mờ) · error (viền `#DC2626` + message dưới ô) · read-only (không viền, như label).
**Edge Case cần mô tả thêm:** format đặc biệt (SĐT, mã số thuế); auto-complete/suggest; giới hạn ký tự (chỉ số, không dấu); prefix/suffix cố định (vd `₫`, `%`).

### 2. Text Area (`textarea`)
Kế thừa `text_input`. **Khác:** nhiều dòng, resize dọc; hiện đếm ký tự khi có giới hạn.
**Edge Case:** giới hạn ký tự cụ thể; không cho resize; rich text (xem #16).

### 3. Button (`button`)
**Sub-types (variant → Dùng cho):**
| Variant | Token | Dùng cho |
|---|---|---|
| primary | fill `#071640`, text `#fff` | Lưu, Gửi, Xác nhận, Áp dụng |
| secondary/ghost | fill `#fff`, border `#E5E7EB`, text `#4B5563` | Hủy, Quay lại, Chỉnh sửa |
| danger | fill `#DC2626`, text `#fff` | Xóa, Dừng đánh giá |
| icon | icon-only | X đóng, cài đặt |
**Sizing:** Width = Hug contents (auto, KHÔNG fix cứng số). Padding ngang 16px, gap icon↔label 8px (`spacing/md`), icon 16px. Height theo ngữ cảnh: Standard 44px (bằng Standard Input) · Filter-bar 36px (bằng Dense Input, khi Button đứng cùng hàng filter) · `.btn-sm` 30px (bảng/toolbar dày đặc). Chi tiết + lịch sử quyết định xem `design-tokens.md` §Button.
**Default Behaviour:** Ấn 1 lần → **disable ngay** (chống double-click). Hành động chờ → loading state. Thành công → thực hiện outcome (mở màn / toast / đóng popup). Thất bại trong form → lỗi inline dưới field (không toast).
**Default States:** active · disabled (mờ) · loading (spinner + "Đang xử lý...") · hidden.
**Default disabled của button (primary) trong form:** required chưa điền đủ **hoặc** đang có lỗi kiểm tra → disabled; đủ + không lỗi → active.
**Edge Case:** button ấn nhiều lần (nút +/− số lượng); hành động tức thời không loading; button có confirm step (Xóa → popup "Bạn có chắc?"); button-as-link.

### 4. Dropdown / Select (`dropdown`)
**Default Display:** Floating label kiểu Material outline (xem `design-tokens.md` §Floating Label) — label nổi đè viền trên khi đã chọn giá trị, placeholder "Chọn..." khi trống, mũi tên xuống phải.
**Default Behaviour:** Ấn → mở list; chọn 1 option → đóng, hiện giá trị; ấn ra ngoài → đóng giữ giá trị cũ.
**Default States:** default · open · selected · disabled · error (viền đỏ + inline).
**Edge Case:** có search (gõ lọc); multi-select; load động; phân cấp (Vùng → Khu vực → NPP → Tuyến → Cửa hàng); option group/divider.

**Sub-pattern — Multi-select checkbox dropdown** (vd filter "Nhà phân phối"): kết hợp search + "Tất cả" + checkbox list trong 1 control.
- **Cấu trúc panel (trên → xuống):** Search input (lọc realtime theo tên) → checkbox "Tất cả" (chọn/bỏ tất cả option hiện có, có thể kèm divider bên dưới) → danh sách checkbox từng option (scroll nếu dài).
- **Trigger (đóng):** hiện số lượng đã chọn (vd "Đã chọn 3") thay vì liệt kê hết tên khi chọn >1.
- **Behaviour:** check/uncheck option → cập nhật ngay, không cần nút "Áp dụng" riêng (trừ khi spec yêu cầu) — ấn ra ngoài để đóng, giữ lựa chọn.
- `[cần verify với DOM thật]` behaviour "đóng có tự commit không" — hiện suy đoán từ screenshot, chưa có outerHTML riêng của dropdown này.

### 5. Checkbox (`checkbox`)
**Behaviour:** Ấn → toggle; trong group mỗi cái độc lập.
**States:** unchecked · checked (✓) · indeterminate (—) · disabled.
**Edge Case:** "Chọn tất cả" (parent-child); min/max chọn trong group; disable từng option.

### 6. Radio (`radio`)
**Behaviour:** Ấn 1 option → chọn, bỏ chọn cái khác; **luôn có 1 option chọn mặc định**.
**States:** unselected · selected · disabled.
**Edge Case:** KHÔNG có default selection (cho bỏ trống); option đổi form bên dưới; hiển thị dạng button group.

### 7. Date Picker (`date_picker`)
**Default Display:** Floating label kiểu Material outline (xem `design-tokens.md` §Floating Label) khi đã chọn ngày; placeholder `DD/MM/YYYY` khi trống, icon lịch phải.
**Behaviour:** Ấn ô/icon → mở calendar; chọn → đóng, hiện ngày; điền tay tự format.
**States:** default · open · selected · disabled · error.
**Edge Case:** min/max date (vd kỳ đánh giá); range từ→đến (chu kỳ xét thưởng); disable ngày cụ thể; chọn tháng/năm.

### 8. Toggle / Switch (`toggle`)
**Behaviour:** Ấn → bật/tắt **ngay** (không confirm). off = xám, on = `#059669`/navy.
**States:** off · on · disabled.
**Edge Case:** cần confirm trước khi đổi (vd Áp dụng/Dừng nguyên tắc); đổi hiển thị form bên dưới; label đổi theo trạng thái.

### 9. Table / Data Grid (`table`)
**Default Display:** header row (`#F9FAFB`, 12px/600/UPPERCASE), data rows zebra, border `#E5E7EB`, radius12; checkbox chọn dòng khi có bulk action. **Cột action mặc định = 1 icon `more_vert` (kebab) mở menu (Sửa/Xóa/...), width cố định ~72px** — không dùng nhiều icon riêng lẻ dàn hàng ngang trừ khi spec yêu cầu rõ hiển thị action trực tiếp trên row (verify DOM thật `dom-reference-list-screen-pattern.md` §3.1).
**Default Behaviour:** Danh sách **nhiều hơn page size → tự hiện pagination** (mặc định 50/trang, chốt 2026-07-06 theo DOM thật — xem `dom-reference-list-screen-pattern.md` §3.4). Ấn header cột sortable → sắp tăng/giảm. Ấn dòng → mở chi tiết. Chọn dòng → hiện bulk action bar. **Pagination luôn render đủ 4 icon-button First/Previous/Next/Last — không áp dụng được (trang đầu/cuối, chỉ 1 trang) → set Disabled (xám), KHÔNG ẩn/xóa khỏi layout.**
**Default States:** populated · loading (skeleton) · empty ("Chưa có dữ liệu" + CTA khi có hành động phù hợp) · error ("Không tải được" + Thử lại) · with-selection.
**Edge Case:** luôn ít hơn page size (không pagination); expandable row; drag sắp xếp; filter theo cột; ẩn/hiện cột; items/trang khác (10/20/100); cho user chọn số items/trang.

### 10. File Upload (`file_upload`)
**Behaviour:** Ấn khu vực → file picker; kéo thả → tự tải; đang tải → progress + tên; xong → tên + kích thước + X xóa. Mặc định 1 file, ≤10MB.
**States:** default · dragging · uploading · done · error ("File quá lớn" / "Sai định dạng").
**Edge Case:** nhiều file; dung lượng khác; định dạng đặc biệt (chỉ ảnh — dùng cho lượt chụp / planogram); preview ảnh.

### 11. Search / Autocomplete (`search`)
**Behaviour:** Gõ → debounce 300ms → tìm; kết quả dropdown; ấn kết quả → chọn; Enter → tìm; X → xóa; ấn ngoài → đóng.
**States:** default · focused · searching (spinner) · results-shown · no-results ("Không tìm thấy") · selected.
**Edge Case:** debounce khác; giới hạn số kết quả; kết quả theo group; recent searches.

### 12. Modal / Dialog (`modal`)
**Default Display:** giữa màn, backdrop `#111827` opacity50, radius16, nút X góc phải; footer: Hủy (ghost) + Xác nhận/Lưu (navy) phải.
**Default Behaviour (Overlay Context — bắt buộc khai per modal):** scroll lock. **Ấn X / ấn backdrop / Esc / Hủy → đóng, quay về màn cha, KHÔNG lưu.** Nút chính → kiểm tra → lưu → đóng → outcome.
**States:** open · closing.
**Edge Case:** KHÔNG đóng khi ấn ngoài (bắt buộc chọn action); nhiều bước (wizard); danger variant (Xóa/Dừng, nút đỏ); full màn mobile; không có X.

### 13. Drawer (`drawer`)
Kế thừa Overlay Context của Modal. **Khác:** trượt từ phải, che 1 phần.
**Edge Case:** trượt trái/dưới; full màn; không backdrop.

### 14. Toast (`toast`)
**Behaviour:** góc phải dưới; fade in 0.3s → hiện → fade out. Tự tắt: Success `MSG-SUC-*` 3s · Error `MSG-ERR-*` 5s · Warning `MSG-WRN-*` 5s · Info `MSG-INF-*` 3s. Nhiều toast xếp chồng.
**Edge Case:** có action ("Hoàn tác"); không tự tắt (phải ấn X); vị trí khác; stack tối đa.

### 15. Banner (`banner`)
**Behaviour:** thanh ngang trên cùng; **KHÔNG tự tắt** — ấn X để đóng. Màu theo loại (error/warning/info/success).
**Edge Case:** có action; đếm ngược tự tắt; sticky khi cuộn.

### 16. Pagination (`pagination`)
**Behaviour:** chỉ hiện khi nhiều hơn page size hiện tại; mặc định 50/trang (chốt 2026-07-06 theo DOM thật); ấn số/`<`/`>`; trang hiện tại tô đậm không ấn; đang tải → disable.
**Default Display (bắt buộc):** luôn render đủ 4 nút điều hướng First `|<` / Previous `<` / Next `>` / Last `>|` — kể cả khi chỉ có 1 trang. Nút không áp dụng được → state Disabled (xám, opacity thấp), **KHÔNG ẩn (`set_visible(false)`) hay xóa khỏi layout**. Kèm "Số dòng mỗi trang [select]" + "[start] - [end] / [total]" (verify DOM thật `dom-reference-list-screen-pattern.md` §3.4).
**Edge Case:** items/trang khác; cho chọn số/trang; "Xem thêm"/infinite scroll; jump-to-page.

### 17. Tabs (`tabs`)
**Behaviour:** ấn tab → đổi nội dung (không reload); tab đầu active mặc định. Active = gạch chân full-width theo tab cell (KHÔNG theo width text), flush đáy tab, `#071640` + text `#071640` 600.
**Platform rule:** Tabs cho **SFA mobile**. **Portal web admin dùng Sidebar sub-nav** — nếu PRD define Tabs cho Portal screen → flag lại BA (khớp SKILL §6.3).
**Edge Case:** lazy load; badge số lượng; vertical tabs; icon+text; cuộn ngang.

### 18. Status Badge (`badge`) — text-only
Vocabulary trạng thái **verify từ NDS-Knowledge** (không tự đặt tên trạng thái):
| Nhóm (màu) | Trạng thái thực tế VisibilityPRO | Token |
|---|---|---|
| Tích cực (green) | Đã áp dụng · Thực thi · Đã phê duyệt · Đạt | text `#059669`, bg `#DCFCE7` |
| Phá hủy/không đạt (red) | Dừng đánh giá · Dừng xét thưởng · Không đạt | text `#DC2626`, bg `#FEE2E2` |
| Chờ (amber) | Mới · Chờ xử lý · Cần đánh giá · Cần hậu kiểm | text `#D97706`, bg `#FEF9C3` |
| Trung tính (slate) | Chưa áp dụng · Kết thúc · Draft · **Bỏ qua** | text `#4B5563`, bg `#F3F4F6` |
| Tích cực (green) — bổ sung | **Thành công** (kết quả gửi email) | text `#059669`, bg `#DCFCE7` |
| Phá hủy/không đạt (red) — bổ sung | **Thất bại** (kết quả gửi email) | text `#DC2626`, bg `#FEE2E2` |
radius4, pad `2 8`. **Edge Case:** badge có icon; badge đếm số; badge click mở filter.
> Nguồn verify: `concept/quan-ly-nguyen-tac-trung-bay` (Chưa/Đã áp dụng), `concept/quan-ly-ke-hoach-danh-gia` (Mới→Thực thi→Dừng đánh giá/Kết thúc), `concept/quan-ly-ke-hoach-xet-thuong` (Chờ xử lý→Đã phê duyệt), NDS-Knowledge (đọc 2026-07-03).
> Bổ sung 2026-07-06: Thành công/Thất bại/Bỏ qua (trạng thái gửi email tổng kết chu kỳ) — nguồn Spec `VISPRO-compliance-email` (`04-spec--compliance-cycle-email-summary.md`, Open Question #7, mapping do design-agent quyết định theo đề xuất Spec, chưa qua NDS-Knowledge vì đây là khái niệm mới của feature email summary, không thuộc entity đã có trong KB).

---

## Message Placement (bắt buộc khai per screen có message)

| Trường | Giá trị hợp lệ |
|---|---|
| surface | inline (dưới field) / toast (góc phải dưới) / banner (thanh trên) / modal |
| MSG code | trỏ `_shared/message-list.md` (`MSG-ERR/SUC/WRN/INF-NN`) — cấm viết text tự do |
| dismiss | toast: tự tắt sau Ns (không cần nút X) · banner/modal: user ấn X hoặc nút hành động để đóng, không tự tắt |

---

## Mobile Controls (SFA app) — bổ sung cho mobile-design-instruction

> Kế thừa toàn bộ Terminology + deviation model ở trên. Dưới đây là control **đặc thù mobile** + khác biệt so với web Portal.

**Touch rule chung:** vùng chạm tối thiểu **44×44px**; primary action đặt trong tầm ngón cái; spacing ≥8px giữa control chạm.

### M1. Bottom Tab Bar (`bottom_tab_bar`)
**Default:** thanh tab dưới cùng, 3–5 tab, icon + label; tab active tô màu `#071640`. **Behaviour:** ấn tab → đổi màn gốc (không stack). **Allowlist:** tab từ nav schema PRD; ngoài list → `MENU_ITEM_GAP`. **Edge:** badge số; >5 tab → "Thêm".

### M2. Bottom Sheet (`bottom_sheet`) — thay Modal trên mobile
**Overlay Context:** trượt từ dưới; **vuốt xuống / ấn backdrop / nút Đóng → đóng, không lưu**; nút chính → kiểm tra → lưu → đóng. **States:** peek (nửa) / expanded / closing. **Edge:** không cho đóng khi bắt buộc chọn; nhiều bước.

### M3. Swipe Actions (`swipe_action`)
**Default:** vuốt trái/phải trên row list → lộ nút (Xóa/Sửa). **Behaviour:** vuốt xóa → confirm (danger) trước khi thực thi (CR-BEH-03). **Edge:** vuốt full = thực thi luôn.

### M4. Pull-to-refresh (`pull_refresh`)
Kéo đầu list xuống → spinner → tải lại. States: idle / pulling / refreshing.

### M5. FAB — Floating Action Button (`fab`)
Nút tròn nổi góc phải dưới cho hành động chính (Thêm/Chụp). Kế thừa Button behaviour (ấn 1 lần → disable). **Edge:** FAB menu (mở nhiều action).

### M6. Segmented Control (`segmented`)
Chọn 1 trong 2–4 lựa chọn dạng pill ngang (thay tabs khi ít mục / trong form). Luôn có 1 active mặc định.

> Message trên mobile: lỗi field = inline; thông báo hệ thống = toast trên/dưới (không banner sticky nếu che tab bar). Overlay = **bottom sheet** (không dùng modal giữa màn như web).
