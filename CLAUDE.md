# Project Context

> This file is automatically loaded by Claude at the start of every session.
> Keep it updated as the project evolves.

---

## Pipeline & Skill Routing

Trước khi bắt đầu bất kỳ task BA nào — bao gồm qualify ticket, viết spec/PRD, phân tích CR, brainstorm — **đọc file sau:**

```
.claude/CONNECTORS.md
```

> **BA Squad agents** (ba-lead, reviewer, discovery-agent, spec-agent, design-agent, delivery-agent): **bỏ qua instruction đọc CONNECTORS.md**. Routing, HITL gates, và artifact paths đã được định nghĩa đầy đủ trong từng agent's instructions. Không dùng AskUserQuestion ở bước trung gian — mọi review đều giao @reviewer agent.

---


## Company & Team

**Company:** NDS Vietnam (Công ty Cổ phần NDS Vietnam)
**Team:** BA (Business Analysis)
**Domain:** B2B SaaS — Công nghệ phân phối & bán lẻ (Distribution & Retail Tech)
**Target users:** Nhà sản xuất (brands/manufacturers), nhà phân phối, cửa hàng tạp hoá, nhà thuốc, hộ kinh doanh nhỏ

---

## Products

### BonBonShop
- **Mô tả:** Nền tảng kết nối trực tiếp nhà sản xuất với các điểm bán lẻ, hỗ trợ quản lý cửa hàng & nhà phân phối, quản lý sản phẩm, đơn hàng, trade marketing và báo cáo trên một nền tảng DMS tích hợp.
- **Giai đoạn:** Live
- **Người dùng chính:** Nhà sản xuất (brand), nhà phân phối, salesman, chủ cửa hàng tạp hoá / nhà thuốc

### VisibilityPRO
- **Mô tả:** Giải pháp AI giúp brand theo dõi trưng bày sản phẩm tại điểm bán bằng nhận diện hình ảnh (image recognition), đo độ phủ và kiểm tra compliance tại các retail point — tăng hiệu quả bán hàng và độ nhận diện thương hiệu.
- **Giai đoạn:** Live
- **Người dùng chính:** Brand Admin, Brand Manager, Trade Marketing Manager, Field Sales Team

#### Các tính năng chính của VisibilityPRO

| Tính năng | Mô tả |
|---|---|
| Nguyên tắc trưng bày (Display Rules) | Cấu hình quy tắc trưng bày: số tầng, số mặt sản phẩm, thứ tự, planogram. Áp dụng cho cửa hàng cụ thể hoặc theo nhóm khách hàng / chương trình TM. |
| Nhóm sản phẩm trưng bày (Display Product Group) | Nhóm các sản phẩm lại để áp dụng vào nguyên tắc trưng bày. |
| Kế hoạch đánh giá AI (AI Evaluation Plan) | Kế hoạch dùng AI nhận diện hình ảnh để đánh giá tuân thủ trưng bày. Liên kết với chương trình TM, cấu hình theo mức (level) và nguyên tắc trưng bày. |
| Kế hoạch đánh giá thủ công (Manual Evaluation Plan) | Kế hoạch do người dùng chấm hình thủ công, cấu hình tiêu chí + phân công nhân viên đánh giá & nhân viên hậu kiểm. |
| Tiêu chí chấm hình thủ công (Manual Evaluation Criteria) | Tiêu chí đánh giá hình ảnh thủ công với 2 loại dữ liệu: Đếm số lượng và Danh sách giá trị được định nghĩa. |
| Kế hoạch hậu kiểm (Post-Audit Plan) | Kế hoạch kiểm tra lại độ chính xác kết quả AI sau khi chạy. Nhân viên hậu kiểm điều chỉnh kết quả nếu AI sai. |
| Kế hoạch xét thưởng (Reward Plan) | Cấu hình tiêu chí xét thưởng theo chu kỳ (giai đoạn đánh giá + chỉ tiêu). Hệ thống tự tính kết quả Đạt/Không đạt cho từng cửa hàng. |
| Cài đặt lý do (Reason Settings) | Danh mục lý do dùng trong hậu kiểm / chấm hình thủ công / xét thưởng. |
| Báo cáo kết quả hình trưng bày | Báo cáo chi tiết từng hình AI đánh giá: compliance, purity, layer mismatch, empty space... |
| Báo cáo kết quả lượt chụp | Báo cáo theo từng lần salesman chụp ảnh tại cửa hàng. |
| Báo cáo chấm hình thủ công (Manual Photo Report) | Báo cáo chi tiết kết quả lượt chụp do nhân viên chấm thủ công: kết quả Đạt/Không đạt, tiêu chí đánh giá, lý do không đạt, ghi chú. |

---

## Glossary

<!-- Thuật ngữ nội bộ để Claude hiểu đúng ngữ cảnh khi đọc ticket, spec, PRD... -->

### Tổ chức địa lý & kênh bán hàng

| Thuật ngữ | Ý nghĩa |
|---|---|
| Region / Vùng | Cấp địa lý lớn nhất trong cơ cấu bán hàng |
| Area / Khu vực | Cấp địa lý nhỏ hơn Region |
| Nhà phân phối / NPP / Distributor | Nhà phân phối chính thức của brand |
| Tuyến bán hàng / Sales Route | Tuyến viếng thăm của salesman, thuộc nhà phân phối |
| Cửa hàng / Outlet / Điểm bán | Điểm bán lẻ (tạp hoá, nhà thuốc, hộ kinh doanh nhỏ). Trong hệ thống gọi là Customer hoặc Outlet. |
| Khách hàng / Customer | Trong ngữ cảnh VisibilityPRO thường chỉ cửa hàng (B2B), không phải người mua lẻ |

Hierarchy địa lý: `Region > Area > Nhà phân phối > Tuyến bán hàng > Cửa hàng`

### Trưng bày sản phẩm

| Thuật ngữ | Ý nghĩa |
|---|---|
| Trưng bày / Display | Việc sắp xếp sản phẩm lên kệ/CTTB theo đúng quy chuẩn |
| CTTB | Công cụ trưng bày — thiết bị vật lý tại điểm bán (cooler, kệ, tủ...) |
| Tài sản / Asset | Một đơn vị CTTB cụ thể, có mã tài sản (Asset Code) |
| Tầng / Shelf Level | Một tầng trên CTTB. Đánh số từ 1 trở lên. |
| Số mặt / Facing | Số lượng mặt sản phẩm xuất hiện nhìn từ phía trước kệ |
| Planogram / Hình mẫu | Ảnh chuẩn thể hiện cách trưng bày đúng, dùng làm reference |
| Nguyên tắc trưng bày / Display Rule | Bộ quy tắc định nghĩa sản phẩm nào, tầng nào, số mặt bao nhiêu, thứ tự ra sao |
| Nhóm sản phẩm trưng bày / Display Product Group | Tập hợp sản phẩm được nhóm lại để áp dụng vào Display Rule |

### Đánh giá & AI

| Thuật ngữ | Ý nghĩa |
|---|---|
| Kế hoạch đánh giá / Evaluation Plan | Kế hoạch cấu hình điều kiện & tiêu chí để đánh giá hình trưng bày (AI hoặc thủ công) |
| Mức / Level | Cấp độ trong kế hoạch đánh giá, do người dùng tự định nghĩa. Mỗi mức gắn với một bộ nguyên tắc trưng bày khác nhau. |
| Kỳ đánh giá / Evaluation Cycle | Chu kỳ đánh giá, do người dùng tự cài đặt |
| Lượt chụp / Capture / Shot | Một lần salesman chụp ảnh tại điểm bán |
| Hình trưng bày / Display Image | Ảnh chụp kệ/CTTB tại cửa hàng, được AI phân tích |
| Compliance | Hình đạt chuẩn trưng bày — sản phẩm đúng tầng, đúng số mặt, đúng thứ tự |
| Non-compliance | Hình không đạt chuẩn trưng bày |
| Purity | Không có sản phẩm đối thủ xuất hiện trên kệ trưng bày |
| Layer Mismatch | Số tầng AI nhận diện được không khớp với số tầng tiêu chuẩn |
| Empty Space | Có khoảng trống trên kệ (thiếu sản phẩm) |
| Invalid Image / Ảnh không chuẩn | Ảnh chụp không đạt yêu cầu kỹ thuật để AI xử lý |
| Hậu kiểm / Post-Audit | Quy trình kiểm tra lại độ chính xác của kết quả AI sau khi chạy |
| Chấm hình thủ công / Manual Photo Evaluation | Người dùng tự xem và đánh giá từng hình thay vì dùng AI |
| Cài đặt lý do / Reason Settings | Danh mục lý do dùng khi từ chối / điều chỉnh kết quả trong hậu kiểm hoặc xét thưởng |

### Chương trình & trạng thái

| Thuật ngữ | Ý nghĩa |
|---|---|
| Chương trình TM / Trade Marketing Program | Chương trình trade marketing gắn với kế hoạch đánh giá |
| Trạng thái áp dụng / Apply Status | Nguyên tắc trưng bày đã áp dụng (Applied) hay chưa (Not Applied) |
| Trạng thái kế hoạch / Plan Status | Mới (New) → Thực thi (In Progress) → Dừng đánh giá (Stopped) |
| Trạng thái kế hoạch xét thưởng | Mới → Thực thi → Dừng xét thưởng |
| Trạng thái phê duyệt chu kỳ | Chờ xử lý → Đã phê duyệt. Chu kỳ Đã phê duyệt không cập nhật/chỉnh sửa được. |
| Trạng thái đánh giá lượt chụp (thủ công) | Cần đánh giá → Cần hậu kiểm → Hoàn thành |
| Kết quả trưng bày / Display Result | Kết quả tổng hợp của 1 hình: Pass / Fail / các lý do cụ thể |
| Kết quả lượt chụp / Capture Result | Kết quả tổng hợp của 1 lần chụp (gồm nhiều hình) |
| Giai đoạn đánh giá / Evaluation Phase | Khoảng thời gian con trong chu kỳ xét thưởng, mỗi giai đoạn có 1 chỉ tiêu (số lượt Đạt tối thiểu) |
| Chỉ tiêu / Target | Số lượt Đạt tối thiểu trong một giai đoạn để cửa hàng đạt chuẩn xét thưởng |
| Nhân viên đánh giá | Nhân viên được phân công chấm hình thủ công trong kế hoạch đánh giá thủ công |
| Nhân viên hậu kiểm | Nhân viên được phân công kiểm tra lại kết quả của nhân viên đánh giá |
| Brand Admin | Vai trò quản trị phía brand trên VisibilityPRO, có quyền cấu hình toàn bộ hệ thống |

---

## VisibilityPRO Knowledge Base — nguồn: NDS-Knowledge (live)

> Knowledge VisibilityPRO **lấy trực tiếp từ NDS-Knowledge** qua MCP, không còn dùng file KB local.
> Quy tắc: **luôn `search_wiki` trước**, rồi `read_wiki_page(slug)` để đọc chi tiết.
> Scope hiện có: department **Production-AI** (43 trang wiki, synthesize từ spec v1.0 + PRD AI Roadmap 2026).

### Quy trình đọc knowledge

1. `search_wiki("<chủ đề cần tìm>")` — luôn là bước đầu tiên.
2. `read_wiki_page(slug)` — đọc full trang từ kết quả search.
3. `get_source_outline` / `get_source_pages` — chỉ khi cần trích dẫn chính xác từ tài liệu gốc.
4. Luôn cite slug trong output để BA verify được.

### Map task → trang NDS-Knowledge

| Task | search_wiki gợi ý | Slug chính |
|---|---|---|
| Kiến trúc hệ thống (OMS/Portal/AI Engine) | "OMS đồng bộ", "AI Engine", "Web Portal" | `concept/oms`, `concept/ai-engine`, `concept/web-portal`, `concept/visibilitypro-portal`, `system/vision-ai` |
| Nguyên tắc trưng bày (Display Rule) | "nguyên tắc trưng bày planogram" | `concept/quan-ly-nguyen-tac-trung-bay` |
| Nhóm sản phẩm trưng bày | "nhóm sản phẩm trưng bày" | `concept/quan-ly-nhom-san-pham-trung-bay`, `concept/quan-ly-nhom-san-pham-ai-nhan-dien` |
| Kế hoạch đánh giá AI | "kế hoạch đánh giá AI" | `concept/quan-ly-ke-hoach-danh-gia-ai`, `concept/quan-ly-ke-hoach-danh-gia` |
| Kế hoạch đánh giá thủ công | "kế hoạch đánh giá thủ công" | `concept/quan-ly-ke-hoach-danh-gia-thu-cong`, `concept/quan-ly-thuc-hien-danh-gia-hinh-anh` |
| Tiêu chí chấm hình thủ công | "tiêu chí chấm hình thủ công" | `concept/quan-ly-tieu-chi-cham-hinh-thu-cong` |
| Hậu kiểm | "kế hoạch hậu kiểm", "thực hiện hậu kiểm" | `concept/quan-ly-ke-hoach-hau-kiem`, `concept/quan-ly-thuc-hien-hau-kiem-hinh-anh`, `concept/quan-ly-ke-hoach-hau-kiem-xuat-du-lieu` |
| Xét thưởng | "kế hoạch xét thưởng", "chu kỳ giai đoạn chỉ tiêu" | `concept/quan-ly-ke-hoach-xet-thuong`, `concept/quan-ly-ke-hoach-xet-thuong-chi-tiet` |
| Cài đặt lý do | "cài đặt lý do" | `concept/quan-ly-cai-dat-ly-do` |
| Báo cáo AI | "báo cáo kết quả hình chụp AI" | `concept/bao-cao-chi-tiet-ket-qua-hinh-chup-ai` |
| Báo cáo chấm hình thủ công | "báo cáo chấm hình thủ công" | `concept/bao-cao-chi-tiet-ket-qua-cham-hinh-thu-cong` |
| Khách hàng / nhóm khách hàng (OMS sync) | "nhóm khách hàng OMS visibilityPRO" | `concept/quan-ly-nhom-khach-hang`, `concept/quan-ly-thong-tin-khach-hang-tich-hop-tu-oms` |
| Roadmap AI 2026 | "PRD AI Roadmap 2026", "lộ trình phát triển" | `source/prd-ai-roadmap-2026`, `concept/tinh-nang` |
| Entity / quan hệ dữ liệu | "thực thể tích hợp OMS" | `concept/incidental-entities`, `concept/oms` |

### Lưu ý khi phân tích

- **Status flow, business rules, ràng buộc** (mã tự sinh, locked states, deletion guards) hiện **nằm rải rác trong các trang concept** chứ chưa có 1 trang tổng hợp. Khi phân tích impact CR, đọc các trang module liên quan rồi tự tổng hợp; nếu cần, dùng `search_wiki` với từ khóa cụ thể ("trạng thái", "không được chỉnh sửa", "phê duyệt").
- **Impact analysis matrix** (đổi X → ảnh hưởng Y) chưa có trang riêng trên NDS-Knowledge — suy ra từ các trang module + quan hệ entity.
- Nếu `search_wiki` báo **out-of-scope** (vd Production-BBS): kiến thức có tồn tại nhưng cần admin cấp quyền — báo lại BA, đừng kết luận là không có.

---

## v2 — Shared Registries, Guards & Control Library (KitA_v2, 2026-07-03)

Khi làm spec/design/review, dùng các nguồn dùng chung (product-level):
- `.claude/shared/common-rules.md` (`CR-*`, có `CR-DOM-*` verified NDS-Knowledge) + `.claude/shared/message-list.md` (`MSG-*`) — tham chiếu mã, cấm viết lại rule/message trùng nghĩa.
- `.claude/shared/pushback-triggers.md` (`MEM-PBK-*`) — khi nào PHẢI dừng/escalate dù autopilot.
- `.claude/skills/design-instruction/references/control-type-library.md` — behaviour/state mặc định UI + chuẩn động-từ hành vi (ấn/điền/chọn...) + §Mobile Controls. Spec ghi `(default)`, chỉ tả deviation.
- `.claude/skills/impact-analysis/` — CR/correction → chạy TRƯỚC khi sửa artifact Approved.
- `.claude/skills/ba-status/` — xem nhanh pipeline (stat-only).

**Freshness cache:** `02-NDS-Knowledge-context.md` mang `stale_status` + `validated_at`; quá 5 ngày/unknown → `search_wiki` lại. Glossary ở trên là **danh-từ domain**; **động-từ hành vi** chuẩn ở control-type-library §Terminology.

---

## Context Kit — quy tắc checkpoint

### Đầu mỗi phiên (bắt buộc, làm trước khi nhận task)
1. Đọc STATE.md trước — đây là nguồn sự thật duy nhất về "đang làm gì / next step / blocker".
2. Nếu cần hiểu lý do đằng sau 1 quyết định cũ trước khi tiếp tục, đọc MEMORY.md.
3. Không cần đọc PLAN.md mỗi lần — chỉ đọc khi cần nhớ lại mục tiêu/scope tổng thể.

### Trong lúc làm việc — checkpoint SỚM, không đợi cuối phiên
Cập nhật STATE.md ngay khi (không đợi gộp lại cuối phiên):
- Vừa xong 1 subtask
- Chuẩn bị chuyển sang việc khác
- Cảm thấy context đang dài / sắp bị compact

Cập nhật MEMORY.md ngay khi:
- Vừa chốt 1 quyết định quan trọng (chọn approach A thay vì B)
- Vừa thử 1 cách không work — ghi lại lý do để không lặp lại ở phiên sau
- Vừa phát hiện 1 gotcha / constraint không rõ ràng ngay từ đầu

### Cuối phiên (bắt buộc, không được bỏ qua kể cả task nhỏ)
STATE.md phải phản ánh đúng thực tế hiện tại trước khi kết thúc phiên.

### Quy tắc viết từng file
- **STATE.md**: luôn ngắn, đọc được trong 5 giây, GHI ĐÈ (không append thêm dòng cũ)
- **MEMORY.md**: append đầy đủ, ghi nhớ toàn bộ — không tự xoá/archive
- **PLAN.md**: gần như bất biến, chỉ sửa khi scope thật sự đổi

### Không được làm
- Không note status vào PLAN.md
- Không note rải rác trong chat mà không đưa vào file
- Không gộp cả 3 file thành 1
