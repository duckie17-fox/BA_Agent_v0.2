# Common Rules — VisibilityPRO (product-level registry)

> Registry quy tắc dùng chung, có mã `CR-*`. Spec/design **tham chiếu mã** thay vì viết lại rule.
> Owner: Lead BA (reviewer là approver cho thay đổi product-level). Nguồn cơ chế: ba-kit-solo `common-rules-template.md` (verbatim 2026-07-03).
> **Seed** từ pattern VisibilityPRO đã biết (`CLAUDE.md` §Glossary — Chương trình & trạng thái). Bổ sung khi có spec/CR mới Approved.

## Metadata
| Field | Value |
|---|---|
| scope | VisibilityPRO (product-level) |
| location (runtime) | `artifacts/_shared/common-rules.md` (seed từ file này) |
| owner | Lead BA |
| status | seed |

## Common Rules
| code | type | rule_statement | applies_to | edge_cases | owner | status | source |
|---|---|---|---|---|---|---|---|
| CR-DIS-01 | DIS | Hiện pagination khi danh sách > 10 items | Mọi màn có table/danh sách | = 10: không pagination, hiện hết. 0: empty state + CTA. | Lead BA | active | control-type-library #9 |
| CR-BEH-01 | BEH | Button (primary) của form disabled khi required chưa điền đủ hoặc đang có lỗi kiểm tra | Form create/edit | Form không required → button luôn active. | Lead BA | active | control-type-library #3 |
| CR-BEH-02 | BEH | Ấn button hành động 1 lần → disable ngay (chống double-click) | Mọi button primary/danger | Nút +/− số lượng: cho ấn nhiều lần. | Lead BA | active | control-type-library #3 |
| CR-VAL-01 | VAL | Lỗi kiểm tra field hiện inline dưới field, không dùng toast | Mọi form | Lỗi hệ thống (mất kết nối) → toast/banner. | Lead BA | active | control-type-library #1 |
| CR-DIS-02 | DIS | Bản ghi ở trạng thái khóa (vd chu kỳ "Đã phê duyệt") → ẩn/disable nút Sửa/Xóa | Xét thưởng, chu kỳ, kế hoạch đã chốt | Ghi rõ trạng thái nào khóa trong spec. | Lead BA | active | CLAUDE.md §Trạng thái phê duyệt chu kỳ |
| CR-BEH-03 | BEH | Thao tác phá hủy (Xóa / Dừng đánh giá / Dừng xét thưởng) → hiện modal confirm (danger) trước khi thực thi | Mọi hành động không hoàn tác | — | Lead BA | active | control-type-library #12 |

## Usage Rules
- Spec/design phải **tham chiếu mã** `CR-*` thay vì định nghĩa lại rule dùng chung.
- Mã `CR-*` mới **phải đăng ký ở đây trước** khi spec/màn hình dùng.
- **Cấm** reuse một mã với nghĩa khác. Rule bị bỏ → đánh `status: deprecated` + lý do, không xóa ngầm.
- Mỗi dòng thêm mới ghi `source` = spec/CR đã Approved (governance nhẹ: nguồn + đã duyệt).

## VisibilityPRO Domain Business Rules — verified NDS-Knowledge (2026-07-03)
> Seed rule nghiệp vụ thật, **cite slug** để BA verify. Bổ sung khi đọc thêm module khác.

| code | type | rule_statement | applies_to | source (slug) |
|---|---|---|---|---|
| CR-DOM-01 | VAL | Mã nguyên tắc trưng bày **tự sinh** `NTTBYYYYMMDD` + 4 số thứ tự tăng/ngày (vd `NTTB202412110001`); không nhập tay | Nguyên tắc trưng bày | concept/quan-ly-nguyen-tac-trung-bay |
| CR-DOM-02 | BEH | Nguyên tắc mặc định **Chưa áp dụng**; thành **Đã áp dụng** khi gán vào kế hoạch. Đã gán → tab Thông tin chung bị khóa sửa (tab Cửa hàng áp dụng sửa theo quy tắc). Switch hoạt động chỉ bật khi đã lưu cả 2 tab | Nguyên tắc trưng bày | concept/quan-ly-nguyen-tac-trung-bay |
| CR-DOM-03 | VAL | Tầng trưng bày 1–10, mỗi tầng dùng 1 lần/nguyên tắc; số mặt mặc định 1 | Nguyên tắc trưng bày | concept/quan-ly-nguyen-tac-trung-bay |
| CR-DOM-04 | VAL | Mã kế hoạch đánh giá: bắt buộc, ≤100 ký tự, **unique**, không space/ký tự đặc biệt (trừ `_ - \ /`) | Kế hoạch đánh giá | concept/quan-ly-ke-hoach-danh-gia |
| CR-DOM-05 | BEH | Vòng đời kế hoạch: **Mới → Thực thi → Dừng đánh giá / Kết thúc**. Mới: sửa/xóa/duyệt. Thực thi: khóa 1 số trường. Kết thúc: không sửa | Kế hoạch đánh giá | concept/quan-ly-ke-hoach-danh-gia |
| CR-DOM-06 | VAL | Chương trình gán vào KH đánh giá chỉ chọn 1 chương trình trạng thái **Đã duyệt** hoặc **Kết thúc** | Kế hoạch đánh giá | concept/quan-ly-ke-hoach-danh-gia |
| CR-DOM-07 | BEH | KH đánh giá AI có tỉ lệ hậu kiểm >0% → hệ thống **tự tạo** kế hoạch hậu kiểm | Kế hoạch đánh giá AI | concept/quan-ly-ke-hoach-danh-gia |
| CR-DOM-08 | BEH | Kế hoạch xét thưởng chỉ **tính toán khi đã duyệt + trạng thái Thực thi**; điều chỉnh kết quả chu kỳ **bắt buộc popup lý do** (Cài đặt lý do) | Xét thưởng | concept/quan-ly-ke-hoach-xet-thuong |
| CR-DOM-09 | VAL | Tên (nguyên tắc / kế hoạch) bắt buộc, ≤255 ký tự | Nhiều module | concept/quan-ly-nguyen-tac-trung-bay · concept/quan-ly-ke-hoach-danh-gia |
| CR-DOM-10 | BEH | Hậu kiểm (AI/thủ công): field chỉnh sửa chỉ enable khi kế hoạch hậu kiểm = "Đang hậu kiểm" và lượt ảnh ở trạng thái Edit; disable toàn bộ (view-only) khi lượt ảnh đã Hoàn thành, hoặc đã quá "số ngày tối đa cập nhật kết quả chu kỳ cũ" | Kế hoạch hậu kiểm AI + thủ công | pageId 34014134 (wiki.ndsvn.vn) |
| CR-DOM-11 | VAL | Hậu kiểm AI: Phân loại lỗi bắt buộc chọn ≥1 khi Kết quả đánh giá thủ công ≠ Kết quả AI (không bắt buộc khi giống nhau); Ghi chú bắt buộc điền khi Phân loại lỗi = "Khác" | Kế hoạch hậu kiểm AI | pageId 34014134 (wiki.ndsvn.vn) |

> **Verify note:** Production-BBS + GA có trang liên quan nhưng **out-of-scope** (cần admin cấp quyền) — rule của BonBonShop chưa seed được ở đây.
