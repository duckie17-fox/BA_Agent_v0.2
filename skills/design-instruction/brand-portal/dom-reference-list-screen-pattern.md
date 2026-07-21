# DOM Reference — List Screen Pattern (Page Header / Filter Bar / Table / Pagination)

> Trích xuất từ DOM export thật của 2 màn list khác nhau trong Portal (2026-07-05).
> **Chỉ ghi LAYOUT/STRUCTURE — không ghi nội dung cụ thể** (tên field, tiêu đề màn, dữ liệu mẫu đều thay đổi theo từng feature). Field/label ví dụ trong file này chỉ minh hoạ, KHÔNG phải giá trị cố định phải copy.
> Phạm vi: pattern cho loại màn "list/danh sách" — khác app-shell (xem `app-shell-reference.md` cho Sidebar/Topbar/Logo, dùng chung mọi màn).

---

## 1. Page Header + Action bar (layout)

```
Row: flex, page title bên trái (flex-1) + action button(s) bên phải (shrink-0)
  Title: text-3xl, font-medium, tracking-tight
  Action area: 1 hoặc nhiều button (đã thấy 1 button và 2 button), luôn mat-raised-button primary
    → button có thể là menu-trigger (mở dropdown) HOẶC routerlink trực tiếp — tuỳ nghiệp vụ, không cố định 1 kiểu
```
- DOM thật đo được 30px (Tailwind text-3xl, font-medium) — nhưng **giá trị chuẩn đã chốt 2026-07-06 là 24px** (BA Lead điều chỉnh xuống, không nhân bản y hệt DOM — giữ font-weight Medium/tracking-tight, chỉ giảm size). Xem `design-tokens.md` §Headings để lấy giá trị chuẩn, KHÔNG dùng 30px trong file này làm token vẽ.
- Số lượng action button không cố định (1–2+ đã quan sát) — số lượng và loại action (menu-trigger vs direct link) phụ thuộc nghiệp vụ từng feature, lấy từ spec chứ không suy diễn từ ví dụ này.

---

## 2. Filter Bar (layout)

```
Form: grid layout, N cột (N tỷ lệ theo số field + vùng action, KHÔNG cố định ở 1 số cụ thể)
  Mỗi field: mat-form-field appearance="outline", chiếm 2–3 cột tuỳ độ rộng cần thiết
  Field types quan sát được: text input · select dropdown · date-range picker (start–end)
  Action area: 0–2 button (đã thấy "Xóa" luôn có; "Tìm kiếm" có thể có hoặc không)
```

**Biến thiên quan trọng — không phải lỗi, là pattern thật:**
- Số cột grid **KHÔNG cố định** — tính theo tổng col-span của field + phần dành cho action, không phải luôn 1 con số cụ thể.
- Nút "Tìm kiếm" **không phải lúc nào cũng có** — một số màn chỉ có "Xóa" (nhiều khả năng filter tự áp dụng khi thay đổi giá trị, tức live-filter). **Khi vẽ màn mới:** kiểm tra spec xem filter là submit-based hay live-filter để quyết định có vẽ nút Tìm kiếm hay không — đừng mặc định luôn có.
- Toàn bộ field quan sát được đều dùng `appearance="outline"` — khớp Floating Label pattern đã document trong design-tokens.md.
- Action button(s) luôn nằm **cùng hàng filter**, căn phải — không tách hàng riêng.

---

## 3. Table (layout)

### 3.1 Cấu trúc cột

```
Cột đầu tiên: cố định ~72px, dành cho menu hành động dạng icon-button (3 chấm)
Cột định danh (mã/code): có thể fixed width
Cột text dài (tên, mô tả): nên để flexible/auto — tránh cắt chữ khi nội dung dài
Cột control (badge/toggle/status): fixed width vừa đủ nội dung
Header: sticky (top: 0), table-layout: fixed
```
- Không phải mọi cột đều cần px cố định — chỉ cột định danh/action nên fixed, cột nội dung dài nên flexible.
- Tổng min-width của table = tổng các cột có width cố định; cột flexible tự giãn theo phần còn lại của container.

### 3.2 Cột "Trạng thái" — 2 kiểu render tuỳ ngữ nghĩa field (không phải 1 kiểu duy nhất)

**Kiểu 1 — Badge:** dùng cho field dạng enum nhiều giá trị, vòng đời (vd: Mới/Đang chạy/Dừng), read-only trong list. Badge màu theo semantic, class đặt tên khác nhau tuỳ context — không suy ra màu cụ thể nếu chưa verify riêng cho field đó.

**Kiểu 2 — Toggle switch:** dùng cho field boolean đơn giản (Bật/Tắt, Active/Inactive) mà user có thể thao tác ngay trong list, không cần mở form riêng. Cần kiểm tra thêm (`[cần verify]`) liệu toggle có thật sự interactive hay chỉ hiển thị read-only tuỳ role/permission — quan sát thực tế có thấy attribute gợi ý không cho phép thao tác trực tiếp trên 1 số row.

**Khi nào dùng kiểu nào:** field enum nhiều giá trị → Badge. Field boolean đơn giản có thể toggle ngay trong list → Toggle switch. Quyết định dựa theo Field table trong spec (loại "Switch" → Toggle theo SKILL.md Core extraction logic), không tự suy diễn từ tên cột.

### 3.3 Row structure

- Mỗi data row thường kèm 1 "detail-row" ẩn (height 0 mặc định) — cơ chế row-expand tiềm năng của framework, không nhất thiết cần dùng.
- Zebra row (màu nền xen kẽ) **chưa xác nhận rõ** qua DOM — `[cần verify thêm]` bằng CSS/ảnh thật trước khi khẳng định có hay không.

### 3.4 Pagination

```
mat-paginator:
  "Số dòng mỗi trang" [select]  ...  "[start] - [end] / [total]"
  [Trang đầu] [Trang trước] [Trang kế] [Trang cuối]  (icon-button, disabled khi ở đầu/cuối)
```
- Label tiếng Việt đầy đủ, đúng chuẩn Data Display Rules hiện có.
- Page size mặc định: **50** — đã chốt 2026-07-06 làm chuẩn chung, đã cập nhật `control-type-library.md` §Table/§Pagination theo giá trị này (thay 10/trang cũ).

---

## Khi nào dùng file này

Đọc khi vẽ bất kỳ màn **list/danh sách** nào (filter bar + table + pagination) để tham khảo cấu trúc layout — KHÔNG copy tên field/tiêu đề/dữ liệu mẫu từ đây, những cái đó lấy từ spec của feature đang vẽ. Nếu màn không phải list (form, dialog, detail) → không cần đọc file này, chỉ cần `app-shell-reference.md`.
