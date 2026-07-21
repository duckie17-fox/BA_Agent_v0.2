# Screenshots — ghi chú độ tin cậy (cập nhật 2026-07-06)

Các ảnh trong thư mục này được chụp từ Portal ở theme **navy #071640 / sidebar #162040**.
**Cập nhật 2026-07-06:** theme này đã được BA Lead **chốt lại là chuẩn hiện hành** (`design-tokens.md` Changelog v3 revert) — bản "Angular Material / Fuse Admin Template indigo #4F46E5 / sidebar gray-900 #111827" từng verify từ CSS thật ngày 2026-07-04 KHÔNG còn là chuẩn áp dụng. Nói cách khác: ảnh trong thư mục này giờ **khớp lại đúng** theme chuẩn, không còn là "ảnh theme cũ" nữa.

**Vẫn dùng được cho:**
- Cấu trúc IA (tên nhóm cấp 1, tên sub-item, thứ tự menu) — ví dụ `Quản lý chấm hình.png`, `Nav bar.png` vẫn phản ánh đúng tên nhóm/sub-item hiện có, dùng để xác nhận nav item mới thuộc nhóm nào.
- Layout tổng thể (sidebar trái/topbar trên/content phải, vị trí filter/table/pagination).
- Hex màu chủ đạo (sidebar/primary) — nay khớp `design-tokens.md`; vẫn nên đối chiếu `design-tokens.md` cho các token chi tiết hơn (state, shadow...).

**KHÔNG dùng được cho:**
- Icon set cụ thể — ảnh thật mới (Fuse) dùng SVG path (heroicons/feather-style), không phải font Material Icons ligature như ảnh cũ thể hiện.
- Font — ảnh thật dùng "Inter var" self-host, không phải font hệ thống mặc định.

**Khi có ảnh/HTML thật mới của 1 màn hình cụ thể** (BA đính kèm trong ticket) → luôn ưu tiên ảnh đó hơn mọi ảnh trong thư mục này. Ảnh trong thư mục này chỉ là fallback tham khảo cấu trúc.
