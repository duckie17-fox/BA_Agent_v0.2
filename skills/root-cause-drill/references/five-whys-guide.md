# 5 Whys Guide

## Technique

Start with the proposed change. Ask "Tại sao [change này] cần thiết?"
Each answer becomes the input for the next "why."
Stop when you reach a genuine user need or business outcome — not another solution.

## Red flags — stop and re-ask if answer is:
- Another solution ("vì cần thêm tính năng X")
- Circular ("vì user muốn button này")
- Too abstract ("vì trải nghiệm tốt hơn")

## Example

**CR:** "Thêm button Export Excel vào màn hình báo cáo doanh số"

| Level | Why? | Answer |
|-------|------|--------|
| Why 1 | Tại sao cần button Export Excel? | Vì Sales team cần lấy data ra ngoài |
| Why 2 | Tại sao cần lấy data ra ngoài? | Vì họ cần gửi báo cáo cho manager mỗi tuần |
| Why 3 | Tại sao phải gửi ra ngoài để báo cáo? | Vì manager không có tài khoản hệ thống |
| Why 4 | Tại sao manager không có tài khoản? | Vì chưa có role phù hợp cho manager view-only |
| Why 5 | Tại sao chưa có view-only role? | Chưa từng được request hoặc prioritize |

**Root cause:** Manager cần visibility vào doanh số nhưng không có quyền truy cập hệ thống.

**CR evaluation:**
- Export Excel giải quyết được vấn đề? Partially — nhưng tạo thêm manual work mỗi tuần.
- Better solution? View-only dashboard for manager — solves root cause permanently.
- Recommendation: "Build differently" — propose manager role before export feature.

---

# JTBD Framework

## Format
"Khi [situation/trigger], [persona] muốn [motivation], để có thể [outcome]."

## Examples

**Weak JTBD (too vague):**
"Khi làm việc, Sales team muốn export data, để có thể xem báo cáo."

**Strong JTBD (specific):**
"Khi chuẩn bị báo cáo tuần vào thứ Sáu, Sales Lead muốn chia sẻ số liệu doanh số cho Regional Manager mà không cần hỏi IT, để có thể gửi trước 5pm mà không bị delay."

## Test a JTBD

Ask:
1. Is the situation specific enough? (when exactly?)
2. Is the motivation a real user need? (not a solution)
3. Is the outcome measurable or observable?
4. Does solving the JTBD require exactly the CR as proposed — or could something else work better?

---

# Root Cause Brief Format

```
---
artifact: Root Cause Brief
ticket-id: [Jira ID hoặc temp ID]
source-skill: root-cause-drill
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: Draft
handoff-to: problem-statement | 5w2h-interview
---

## Root Cause Brief: [CR Name]

**Nguồn:** Change Request từ [requester], [date]
**Loại:** Change Request (Áp đặt)
**Prepared by:** BA
**Confidence:** Cao / Trung bình / Thấp

---

### Yêu cầu gốc (CR as stated)
[Mô tả lại CR một cách rõ ràng — không phán xét]
- Change: [what is being requested]
- Tính năng hiện tại bị ảnh hưởng: [existing feature/screen]
- Requester: [role/team]

---

### 5 Whys Analysis

| Level | Câu hỏi | Câu trả lời |
|-------|---------|-------------|
| Why 1 | Tại sao [CR] cần thiết? | |
| Why 2 | Tại sao [answer 1]? | |
| Why 3 | Tại sao [answer 2]? | |
| Why 4 | Tại sao [answer 3]? | |
| Why 5 | Tại sao [answer 4]? | |

**Root cause:** [1-2 câu mô tả vấn đề gốc rễ thực sự]

---

### JTBD Statement

"Khi [situation], [persona] muốn [motivation], để có thể [outcome]."

---

### Đánh giá CR

**Recommendation:** Build as requested / Build differently / Needs more discovery / Reject

**Lý do:**
[2-3 câu giải thích tại sao recommendation này]

**Nếu "Build differently":**
[Mô tả alternative solution được đề xuất, với lý do tại sao phù hợp hơn với root cause]

---

### Broader Impact Analysis

**Ảnh hưởng tới hệ thống (Cross-system)**
- [Tính năng/module nào bị ảnh hưởng? Data model, API, business rule nào dùng chung?]
- [Behavior nào hiện tại có thể thay đổi ngoài ý muốn?]

**Ảnh hưởng tới các khách hàng khác (Cross-customer)**
- [Có bao nhiêu khách khác đang gặp vấn đề tương tự?]
- [Ai được lợi / ai có thể bị ảnh hưởng tiêu cực nếu apply toàn bộ?]
- [Nên roll out cho tất cả hay chỉ per-customer config?]

**Ảnh hưởng tới các ngành hàng khác (Cross-vertical)**
- [Ngành nào được lợi đặc biệt từ change này?]
- [Đây có phải vấn đề phổ biến của ngành (industry-wide) không?]
- [Có nên nâng thành product feature thay vì custom change không?]

_Nếu không xác định được tác động ở chiều nào: ghi rõ "Không xác định — cần thêm context từ [ai/nguồn nào]."_

---

### Open Questions
- [Câu hỏi còn chưa rõ] — cần hỏi [ai]

---

### Bước tiếp theo
- Nếu **Build as requested**: → `problem-statement`
- Nếu **Build differently**: → `/brainstorm` trước để explore giải pháp tốt hơn, sau đó → `problem-statement`
- Nếu **Needs more discovery**: → `5w2h-interview`
- Nếu **Reject**: → Thông báo cho requester với lý do rõ ràng

> **Gợi ý dùng `/brainstorm` khi "Build differently":**
> Root cause đã xác định và Broader Impact cho thấy vấn đề có scope rộng hơn CR gốc.
> Brainstorm giúp explore "vậy thì nên build cái gì?" trước khi frame HMW trong problem-statement.
```
