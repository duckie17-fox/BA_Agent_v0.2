# Research Brief Format — RESEARCH mode

Output structure khi không thể interview, dùng làm input cho search agent.

```
---
artifact: Research Brief
ticket-id: [Jira ID hoặc temp ID]
source-skill: 5w2h-interview
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: Draft
handoff-to: search-agent
---

## Research Brief: [Feature/Problem Name]

**Nguồn ticket:** [Qualified ticket reference]
**Lý do không interview:** [Không tiếp cận được stakeholder / Gấp / Early exploration / Khác]
**Prepared by:** BA
**Ngày:** [date]

---

### WHO — Tìm hiểu người dùng bị ảnh hưởng

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query tìm user persona / demographics trong domain này]`
2. `[query tìm user complaints / pain points liên quan]`
3. `[query tìm user research / surveys trong ngành]`

**Target sources:** UX research blogs, industry reports, Reddit/forums, G2/Capterra reviews

---

### WHAT — Tìm hiểu vấn đề cụ thể

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query validate vấn đề có phổ biến không]`
2. `[query tìm root causes phổ biến của vấn đề này]`
3. `[query tìm workarounds mà người dùng đang dùng]`

**Target sources:** Support forums, Stack Overflow, community discussions, blog posts

---

### WHERE — Tìm hiểu bối cảnh xảy ra

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query về workflow / process context]`
2. `[query về technical environment liên quan]`

**Target sources:** Product documentation, workflow diagrams, industry process guides

---

### WHEN — Tìm hiểu tần suất và thời điểm

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query về frequency / volume data trong ngành]`
2. `[query về seasonal patterns nếu relevant]`

**Target sources:** Industry benchmarks, analytics reports, case studies

---

### WHY — Tìm hiểu nguyên nhân và tầm quan trọng

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query về business impact / cost of problem]`
2. `[query về industry trends driving this need]`
3. `[query về regulatory / compliance factors nếu có]`

**Target sources:** Industry reports, analyst publications, news articles

---

### HOW — Tìm hiểu cách giải quyết hiện tại và alternatives

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query về competitor solutions / features]`
2. `[query về best practices trong domain]`
3. `[query về technical approaches / patterns]`

**Target sources:** Competitor websites, product comparison sites, technical blogs, case studies

---

### HOW MUCH — Tìm hiểu quy mô và tác động

**Đã biết từ ticket:**
- [Thông tin đã có]

**Cần tìm thêm:**
- [Gap cụ thể]

**Search queries cho agent:**
1. `[query về market size / TAM nếu relevant]`
2. `[query về ROI / cost-benefit data của solutions tương tự]`

**Target sources:** Market research, ROI calculators, vendor case studies

---

### ⚠️ Must-Interview Questions (không thể research thay)

Những thông tin sau CHỈ có thể lấy từ interview nội bộ, search agent không thể tìm được:

1. [Câu hỏi về internal process cụ thể]
2. [Câu hỏi về internal KPIs / metrics]
3. [Câu hỏi về internal politics / priorities]
4. [Câu hỏi về specific user behavior trong hệ thống hiện tại]

→ BA cần follow up những câu này dù có research results hay chưa.

---

### Bước tiếp theo
→ Chuyển research brief này cho **search agent** để research.
→ Sau khi có kết quả research, kết hợp với must-interview answers để tạo **Context Brief** (BRIEF mode).
→ Feed Context Brief vào **problem-statement** skill.
```
