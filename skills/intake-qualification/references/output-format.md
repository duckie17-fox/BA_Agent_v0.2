# Output Format

## Draft Output (trước khi BA approve)

```
---
artifact: Qualified Ticket
ticket-id: [Jira ID hoặc temp ID — ví dụ: VPR-001]
source-skill: intake-qualification
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: Draft
---

## Qualified Ticket: [Feature/Request Name]

**Ngày nhận:** [date]
**Requester:** [name/role/team]
**Channel:** [Jira / Email / Chat / Meeting]

---

### Phân loại
**Loại yêu cầu:** New Feature / Change Request / Ambiguous
**Lý do:** [1-2 câu giải thích tại sao classify như vậy]

---

### Mô tả yêu cầu (đã chuẩn hóa)
[2-3 câu tóm tắt yêu cầu bằng ngôn ngữ BA — không phải copy nguyên văn]

**Người dùng bị ảnh hưởng:** [user segment]
**Business impact:** [revenue / efficiency / risk / customer retention]
**Urgency signals:** [any deadline or escalation mentioned, or "Không có"]

---

### Tính năng liên đới

| Tính năng | Loại liên đới | Ghi chú |
|-----------|---------------|---------|
| [tên tính năng] | Trực tiếp / Gián tiếp / Dependency | [lý do liên đới] |

_Nếu không xác định được: ghi rõ lý do thiếu context._

---

### DoR Check

| Criterion | Status | Ghi chú |
|-----------|--------|---------|
| Requester identified | ✅ / ⚠️ / ❌ | |
| Problem described | ✅ / ⚠️ / ❌ | |
| Affected users | ✅ / ⚠️ / ❌ | |
| Business impact | ✅ / ⚠️ / ❌ | |
| Scope understood | ✅ / ⚠️ / ❌ | |
| Not duplicate | ✅ / ⚠️ / ❌ | |
| Not blocked | ✅ / ⚠️ / ❌ | |

**Kết quả DoR:** ✅ Ready / ⚠️ Conditional / ❌ Not Ready

**Nếu Not Ready — cần bổ sung:**
- [ ] [Thông tin cần hỏi requester]

---

### Duplicate Check
**Pipeline mode:** Xem kết quả Jira search thực tế tại `00-external-context.md` (section `## Jira duplicate search`).
**Standalone mode:** Chạy các query sau trong Jira/backlog trước khi tiếp tục:
1. `[search query 1]`
2. `[search query 2]`
3. `[search query 3]`

---

### Priority
**Đề xuất:** P0 / P1 / P2
**Lý do:** [signal cụ thể từ yêu cầu]

---

```

---

## Post-Approval Append (chỉ thêm vào SAU KHI BA approve)

Append đoạn sau vào cuối file, đổi `status: Draft` → `status: Approved`:

```
---

### Research Brief cho Search Agent

Dùng qualified ticket ở trên làm input, yêu cầu search agent research 3 trục:

1. **Validate vấn đề** — Vấn đề này có thật sự phổ biến không?
   - Search queries: `[2-3 search query cụ thể để validate vấn đề trên web/forums/communities]`
   - Tìm: số liệu, user complaints, survey data, forum threads

2. **Competitive landscape** — Competitor đang giải quyết vấn đề này thế nào?
   - Search queries: `[2-3 search query về competitor + feature tương tự]`
   - Tìm: feature comparison, pricing, UX approach, user reviews

3. **Precedents & best practices** — Ai đã giải quyết thành công, bằng cách nào?
   - Search queries: `[2-3 search query về solutions, case studies, patterns]`
   - Tìm: case studies, technical approaches, lessons learned, anti-patterns

### Bước tiếp theo
**Nếu New Feature:** → `5w2h-interview`
**Nếu Change Request:** → `root-cause-drill`

> **Gợi ý dùng `/brainstorm` trước khi vào discovery:**
> Nếu DoR = Conditional, priority chưa rõ, hoặc team chưa chắc đây có phải vấn đề đúng cần solve —
> dùng `/brainstorm` để think broadly trước khi đi vào 5w2h-interview hoặc root-cause-drill có cấu trúc.
```
