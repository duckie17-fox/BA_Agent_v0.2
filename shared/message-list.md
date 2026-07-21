# Message List — VisibilityPRO (product-level registry)

> Registry mọi thông điệp UI, có mã `MSG-*`. Spec/design **tham chiếu mã** thay vì viết text tự do.
> Owner: Lead BA. Nguồn cơ chế: ba-kit-solo `message-list-template.md` (verbatim 2026-07-03).
> **Seed** — bổ sung khi có spec/CR mới Approved.

## Metadata
| Field | Value |
|---|---|
| scope | VisibilityPRO (product-level) |
| location (runtime) | `artifacts/_shared/message-list.md` (seed từ file này) |
| owner | Lead BA |
| status | seed |

## Message List
| code | type | canonical_text | surface | applies_to | status | source |
|---|---|---|---|---|---|---|
| MSG-SUC-01 | SUC | Lưu thành công | toast (3s) | Mọi form save | active | seed |
| MSG-ERR-01 | ERR | Vui lòng điền [tên trường] | inline | Required field trống | active | seed |
| MSG-ERR-02 | ERR | [Tên trường] không hợp lệ | inline | Field sai format | active | seed |
| MSG-ERR-03 | ERR | Không tải được dữ liệu. Thử lại. | banner/toast (5s) | Table load fail | active | seed |
| MSG-WRN-01 | WRN | Bạn có chắc muốn [hành động] [đối tượng]? Không thể hoàn tác. | modal | Xóa / Dừng đánh giá / Dừng xét thưởng / Hoàn tất spot-check bulk (BOS-02, VISPRO-2241) | active | seed; extended by VISPRO-2241 (04-spec--spot-check-noibo.md) |
| MSG-INF-01 | INF | Chưa có dữ liệu | inline (empty state) | Table/list rỗng | active | seed |
| MSG-ERR-04 | ERR | Tên nhãn đã tồn tại | inline | Form tạo/sửa Nhãn (F-01) — trùng tên nhãn | active | store-tagging (04-spec--quan-ly-nhan.md) |
| MSG-ERR-05 | ERR | Không thể xoá nhãn đang được gắn cho [N] cửa hàng | inline/modal | Form xoá Nhãn (F-01) — nhãn còn cửa hàng tham chiếu | active | store-tagging (04-spec--quan-ly-nhan.md) |
| MSG-ERR-06 | ERR | Kế hoạch đã thay đổi trạng thái, không thể xóa. Vui lòng tải lại. | banner/toast (5s) | Xoá kế hoạch đánh giá AI — trạng thái đổi (vd New→Thực thi) giữa lúc Confirm Dialog đang mở và lúc xác nhận xoá (race condition đa phiên/đa tab) | active | CR-20260706 (04-spec--xoa-ke-hoach-danh-gia-ai.md, BR-XKHAI-04) |
| MSG-ERR-EMAIL-DUP | ERR | Email đã tồn tại trong danh sách | inline (dưới field) | Form/tab Cấu hình người nhận email (SCR-02) — điền địa chỉ email đã có sẵn trong danh sách người nhận của cùng 1 Kế hoạch xét thưởng | active | VISPRO-compliance-email (04-spec--compliance-cycle-email-summary.md, UC-EMAIL.EX.3) |
| MSG-ERR-ATT-01 | ERR | Chỉ được đính kèm tối đa 5 ảnh | inline (dưới control upload) | Khối bằng chứng bổ sung (SCR-02/SCR-04 hậu kiểm AI + thủ công) — tải ảnh vượt giới hạn 5 ảnh/quyết định | active | planogram-attachment-post-audit (04-spec--hau-kiem-ai.md, BR-HKAI-04) |
| MSG-ERR-ATT-02 | ERR | Vui lòng đính kèm ít nhất 1 ảnh bằng chứng cho quyết định điều chỉnh này | inline (dưới control upload) | Khối bằng chứng bổ sung (SCR-02/SCR-04 hậu kiểm AI + thủ công) — ấn Xác nhận/Chỉnh sửa kết quả khi điều kiện bắt buộc đính kèm ảnh kích hoạt (Phân loại lỗi="Khác" ở luồng AI; Lý do ảnh không đạt="Khác" hoặc điều chỉnh giá trị tiêu chí khác gốc ở luồng thủ công) nhưng chưa có ảnh đính kèm | active | planogram-attachment-post-audit (04-spec--hau-kiem-ai.md BR-HKAI-02, 04-spec--hau-kiem-thu-cong.md BR-F02-02) |
| MSG-ERR-ATT-03 | ERR | Không tải được ảnh | inline (tại vị trí thumbnail) | Popup "Hoạt động" (SCR-05, hậu kiểm AI + thủ công) — 1 thumbnail ảnh đính kèm lỗi tải, không chặn hiển thị các ảnh/log khác | active | planogram-attachment-post-audit (04-spec--tab-lich-su.md) |
| MSG-INF-SPOT-01 | INF | SLA spot-check đã hết, kết quả còn lại đã tự động xác nhận theo Kết quả AI | banner | BOS-02 (Spot-check/Override nội bộ, VISPRO-2241) — AI Engineer đang mở màn khi SLA của lượt upload về 0 (UC-SPOT-01.EX.1) | active | VISPRO-2241 (04-spec--spot-check-noibo.md, BR-SPOT-02) |
| MSG-ERR-PORTAL-01 | ERR | Mã OTP không đúng hoặc đã hết hạn | inline (dưới field OTP) | SCR-01 Đăng nhập (Portal TINVIET) — OTP sai/hết hạn (UC-PORTAL.EX.1) | active | VISPRO-2241 (04-spec--portal-tinviet.md) |
| MSG-ERR-PORTAL-02 | ERR | Chỉ được tải tối đa 20 ảnh | inline (dưới control upload) | SCR-02 Upload mới (Portal TINVIET) — vượt giới hạn ảnh trực tiếp (EC-02) | active | VISPRO-2241 (04-spec--portal-tinviet.md) |
| MSG-ERR-PORTAL-03 | ERR | Phiên đăng nhập đã hết hạn, vui lòng đăng nhập lại | banner (ấn X hoặc tự redirect) | SCR-02+ Portal TINVIET — phiên/token hết hạn (UC-PORTAL.EX.4) | active | VISPRO-2241 (04-spec--portal-tinviet.md) |
| MSG-ERR-PORTAL-04 | ERR | Không tạo được file xuất dữ liệu, vui lòng thử lại | toast (5s, tự tắt) | SCR-05 Export (Portal TINVIET) — tạo file export thất bại | active | VISPRO-2241 (04-spec--portal-tinviet.md) |
| MSG-SUC-PORTAL-01 | SUC | Xuất dữ liệu thành công | toast (3s, tự tắt) | SCR-05 Export (Portal TINVIET) — tải xuống hoàn tất | active | VISPRO-2241 (04-spec--portal-tinviet.md) |
| MSG-ERR-TVMS-01 | ERR | Vui lòng chọn [tên trường] | inline | Required dropdown/select trống (khác pattern "điền" của MSG-ERR-01 vốn dành cho text field) — vd SCR-02 Preview & Map, dòng sheet chưa chọn Rule/Cột chứa link ảnh | active | CR-20260714 (04-spec--nhan-dien-san-pham-excel-tinviet.md, EC-TVMS-02) |

## Usage Rules
- Spec/design tham chiếu mã `MSG-*` (kèm surface + dismiss theo control-type-library §Message Placement).
- Mã mới **đăng ký ở đây trước** khi màn hình dùng.
- **Cấm** reuse một mã với text khác. Message bỏ → `status: deprecated`, không xóa ngầm.
