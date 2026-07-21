# Question Patterns — 5W2H

Generate questions by adapting these patterns to the specific context.
Never use these verbatim — always rewrite for the domain.

## WHO — Người dùng

Uncover who actually experiences the problem (may differ from requester).

Patterns:
- "Ai là người [thực hiện hành động X] hàng ngày?"
- "Có bao nhiêu người trong team [role] đang gặp vấn đề này?"
- "Người dùng có kinh nghiệm khác nhau (mới vs. lâu năm) gặp vấn đề theo cách khác nhau không?"
- "Ngoài [role đã biết], còn ai khác bị ảnh hưởng?"
- "Ai là người ra quyết định cuối cùng trong quy trình này?"

## WHAT — Vấn đề

Understand the exact problem, not the assumed solution.

Patterns:
- "Bạn mô tả cụ thể điều gì xảy ra khi [trigger situation]?"
- "Bước nào trong quy trình hiện tại tốn nhiều thời gian nhất?"
- "Điều gì thường xuyên sai hoặc phải làm lại?"
- "Nếu vấn đề này được giải quyết, cuộc sống hàng ngày của bạn thay đổi thế nào?"
- "Bạn đã thử cách nào để giải quyết vấn đề này chưa? Tại sao không hiệu quả?"

## WHERE — Bối cảnh

Context of where the problem occurs.

Patterns:
- "Vấn đề xảy ra ở màn hình/bước nào trong hệ thống?"
- "Trên thiết bị nào? Desktop, tablet, hay mobile?"
- "Ở giai đoạn nào của quy trình [tên quy trình] thì vấn đề xuất hiện?"
- "Có phụ thuộc vào data từ hệ thống nào khác không?"

## WHEN — Tần suất và thời điểm

Frequency and timing patterns.

Patterns:
- "Vấn đề này xảy ra bao nhiêu lần [một ngày / một tuần / một tháng]?"
- "Có thời điểm nào trong năm mà vấn đề trở nên nghiêm trọng hơn không?"
- "Lần gần nhất vấn đề này gây ra hậu quả nghiêm trọng là khi nào?"
- "Bao lâu thì đây trở thành vấn đề cần giải quyết?"

## WHY — Nguyên nhân và tầm quan trọng

Root cause and business impact.

Patterns:
- "Tại sao quy trình hiện tại được thiết kế như vậy? Có lý do lịch sử nào không?"
- "Nếu không giải quyết vấn đề này, hệ quả tệ nhất là gì?"
- "Vấn đề này ảnh hưởng đến KPI hoặc metric nào của team?"
- "Tại sao đây là ưu tiên bây giờ chứ không phải 6 tháng trước?"
- "Khách hàng phàn nàn về điều này không?"

## HOW — Quy trình hiện tại

Current state and workarounds.

Patterns:
- "Hiện tại bạn đang xử lý việc này như thế nào? Mô tả từng bước."
- "Có workaround nào đang được dùng không? Mất bao nhiêu thời gian?"
- "Công cụ nào đang được dùng trong quy trình này?"
- "Quy trình lý tưởng trông như thế nào theo bạn?"
- "Có team khác hoặc công ty khác xử lý vấn đề tương tự không? Họ làm thế nào?"

## HOW MUCH — Quy mô và tác động

Volume and impact quantification.

Patterns:
- "Mỗi lần xử lý vấn đề này mất bao nhiêu phút/giờ?"
- "Số lượng [đơn hàng / yêu cầu / khách hàng] bị ảnh hưởng mỗi [kỳ]?"
- "Nếu có tool tốt hơn, team tiết kiệm được bao nhiêu thời gian mỗi tuần?"
- "Chi phí (người, tiền, cơ hội) của vấn đề này là bao nhiêu?"
- "Có SLA hoặc target nào đang bị miss vì vấn đề này không?"

---

# Context Brief Format

## Output structure for BRIEF mode

```
---
artifact: Context Brief
ticket-id: [Jira ID hoặc temp ID]
source-skill: 5w2h-interview
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: Draft
handoff-to: problem-statement
---

## Context Brief: [Feature/Problem Name]

**Nguồn:** Interview với [roles], [date]
**Prepared by:** BA
**Confidence:** Cao / Trung bình / Thấp

---

### WHO — Người dùng bị ảnh hưởng
[Mô tả persona, số lượng, đặc điểm]
- Người dùng chính: [role, team, số lượng ước tính]
- Người dùng phụ: [nếu có]
- Điểm khác biệt giữa các nhóm: [nếu có]

### WHAT — Vấn đề cụ thể
[Mô tả vấn đề thuần — không phải solution]
- Hành động đang gặp khó khăn: [specific action]
- Triệu chứng rõ nhất: [what they see/experience]
- Workaround hiện tại: [what they do now]
- Tại sao workaround không đủ: [gap]

### WHERE — Bối cảnh xảy ra
- Màn hình/bước trong hệ thống: [specific location]
- Thiết bị: [desktop/mobile/tablet]
- Điều kiện trigger: [when does it happen]

### WHEN — Tần suất
- Tần suất: [N lần / ngày / tuần / tháng]
- Thời điểm đỉnh điểm: [if seasonal or periodic]
- Đã xảy ra từ: [how long has this been a problem]

### WHY — Nguyên nhân và tầm quan trọng
- Nguyên nhân gốc rễ (theo người dùng): [what they believe causes it]
- Business impact: [KPI / metric bị ảnh hưởng]
- Hậu quả nếu không giải quyết: [risk or cost]

### HOW — Quy trình hiện tại
[Step-by-step mô tả cách họ đang làm]
1. [Bước 1]
2. [Bước 2]
...

### HOW MUCH — Quy mô
- Số người bị ảnh hưởng: [N users / N% of user base]
- Thời gian lãng phí: [N phút/lần × N lần/tuần = N giờ/tuần]
- Volume: [N records / transactions affected]

---

### Open Questions (còn chưa rõ)
- [Câu hỏi 1] — cần hỏi [ai]
- [Câu hỏi 2] — cần data từ [nguồn]

---

### Key Quotes (trích dẫn đáng chú ý từ interview)
- "[Quote 1]" — [role]
- "[Quote 2]" — [role]

---

### Bước tiếp theo
→ Feed Context Brief này vào **problem-statement** skill để tạo HMW + User Story.
```
