# Push-back Triggers — BA Squad (fail-closed shard)

> Bảng tín hiệu **PHẢI dừng và hỏi / escalate** dù đang autopilot. Agent đọc trước khi ra quyết định có rủi ro.
> Nguyên tắc: khi có trigger → dừng, KHÔNG điền bằng prose hợp lý.
> Nguồn cơ chế: ba-kit-solo `project-memory-hot-pushback-triggers-template.md` (verbatim 2026-07-03) — port sang ngữ cảnh BA Squad (giữ autopilot làm mặc định, chỉ dừng ở các trigger cụ thể).

## Trigger Table
| Trigger ID | Dấu hiệu phải dừng | Hành động bắt buộc | Áp dụng |
|---|---|---|---|
| MEM-PBK-01 | NDS-Knowledge báo **out-of-scope** (vd Production-BBS) | Dừng, báo BA cần admin cấp quyền — **KHÔNG kết luận "không có"** | discovery, spec, reviewer |
| MEM-PBK-02 | Số liệu / khẳng định **không có nguồn** | Gắn `[chưa verify]` / `[giả định]` — KHÔNG trình bày như đã xác định | mọi agent |
| MEM-PBK-03 | **Conflict** giữa NDS-Knowledge (wiki) và ticket/Jira | Escalate ba-lead → người phụ trách; KHÔNG tự chọn | discovery, spec |
| MEM-PBK-04 | Yêu cầu sửa **artifact đã Approved** | Route qua `impact-analysis` trước (KHÔNG overwrite trực tiếp) | ba-lead, spec, design |
| MEM-PBK-05 | Fact bắt buộc **thiếu** cho bước tiếp | Đánh dấu open question — KHÔNG đoán | spec, design, prd |
| MEM-PBK-06 | Hành động **không hoàn tác** (đẩy Jira, publish Figma) | Dừng, chờ người phụ trách duyệt Final Report | ba-lead, delivery, design |
| MEM-PBK-07 | Lặp > 3 vòng review không tiến triển | Escalate "Cần người quyết" (reviewer round 3) | reviewer, ba-lead |

## Fail-Closed Principles
- Có trigger → dừng và hỏi/escalate, KHÔNG điền bằng prose hợp lý.
- Fact bắt buộc thiếu → giả định/open question, không trình bày như đã chốt.
- Downstream cần đoán quyết định upstream → route về bước sở hữu quyết định.
- **Autopilot vẫn là mặc định** (Bước 2+ của ba-lead): các trigger trên là *ngoại lệ* để dừng, không phải quay lại hỏi-mọi-bước.

## Usage
- Thêm trigger mới → tăng ID `MEM-PBK-NN`, ghi nguồn (spec/CR phát hiện).
- Trigger bị vô hiệu (scope đổi) → ghi lý do, không xóa ngầm.
