---
artifact: PRD
ticket-id: [Jira ID hoặc temp ID]
source-skill: write-prd
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: Draft
handoff-to: write-spec
confluence_page_id: null
---

# PRD: [Epic/Feature Name]

> **Version:** 1.0 | **Tác giả:** [Tên BA] | **Ngày:** [DD/MM/YYYY]
> **Trạng thái:** Draft / In Review / Approved

---

## 1. Problem Statement

[2-3 câu: Ai gặp vấn đề gì, hậu quả nếu không giải quyết]

---

## 2. Goals & Non-Goals

### Goals
- [Outcome 1]: [Target] — đo bằng [metric]
- [Outcome 2]: [Target] — đo bằng [metric]
- [Outcome 3]: [Target] — đo bằng [metric]

### Non-Goals
- **[Tính năng/scope X]**: [Lý do không làm trong phiên bản này]
- **[Tính năng/scope Y]**: [Lý do]

---

## 3. System Overview & Flow

> **New Feature** → Dùng 3a: System Interaction Diagram + Function List  
> **Change Request** → Dùng 3b: As-Is / To-Be Flow

---

### 3a. System Interaction Diagram *(New Feature)*

> Diagram thể hiện các hệ thống nào tham gia và luồng tương tác chính ở mức epic.  
> Giữ tối giản: 5–8 node, không đi vào detail — detail thuộc về Spec.

```mermaid
flowchart LR
    subgraph BO["Back Office (BO)"]
        BO1["[Chức năng BO 1]"]
        BO2["[Chức năng BO 2]"]
    end

    subgraph Portal["Portal"]
        P1["[Chức năng Portal 1]"]
        P2["[Chức năng Portal 2]"]
    end

    subgraph BBS["BBS App"]
        A1["[Chức năng App 1]"]
    end

    BO1 -->|"[mô tả tương tác]"| P1
    P1 -->|"[mô tả tương tác]"| A1
    A1 -->|"[mô tả tương tác]"| P2
```

**Function List:**

| Hệ thống | Chức năng | Mô tả ngắn |
|----------|-----------|------------|
| BO | | |
| Portal | | |
| BBS App | | |

---

### 3b. As-Is / To-Be Flow *(Change Request)*

> Diagram thể hiện luồng hiện tại và luồng sau khi thay đổi.  
> Highlight phần thay đổi bằng chú thích hoặc style khác biệt.

**As-Is (Hiện tại):**

```mermaid
flowchart LR
    A["[Bước 1]"] --> B["[Bước 2]"] --> C["[Bước 3]"]
```

**To-Be (Sau thay đổi):**

```mermaid
flowchart LR
    A["[Bước 1]"] --> B["[Bước 2 — thay đổi]"] --> C["[Bước 3]"] --> D["[Bước mới]"]
```

**Điểm thay đổi chính:**
- [Thay đổi 1]: [Mô tả ngắn — lý do]
- [Thay đổi 2]: [Mô tả ngắn — lý do]

---

## 4. Feature List

| # | Tính năng | Mô tả ngắn | Priority |
|---|-----------|------------|----------|
| 1 | | | P0 |
| 2 | | | P1 |
| 3 | | | P2 |

---

## 5. Screen Map & Navigation

**Navigation pattern:** [Tabs / Sidebar sub-nav / Drill-through / Hybrid]

**Shared shell elements** (persist qua tất cả views):
- [Ví dụ: Tab bar — Tab A │ Tab B │ Tab C]
- [Ví dụ: Filter bar với bộ lọc: TM Program, Date Range, ...]

**Screen Map & Navigation Flow:**

```mermaid
flowchart TD
    S1["[Tên màn 1]\n[mô tả 1 dòng]"]
    S2["[Tên màn 2]\n[mô tả 1 dòng]"]
    S3["[Tên màn con / sub-view]"]
    S4["[Tên màn 3]\n[mô tả 1 dòng]"]

    S1 -->|"[click tab / click row / button]"| S2
    S2 -->|"[click row]"| S3
    S3 -->|"[breadcrumb]"| S2
    S1 -->|"[cách navigate]"| S4
```

---

## 6. Use Cases

### [Persona 1]
- Người dùng có thể [hành động A]
- Người dùng có thể [hành động B]
- Người dùng có thể [hành động C]

### [Persona 2]
- Người dùng có thể [hành động A]
- Người dùng có thể [hành động B]

---

## 7. Success Metrics

| Metric | Baseline | Target | Đo bằng | Đánh giá sau |
|--------|----------|--------|---------|--------------|
| | | | | |
| | | | | |

---

## 8. Timeline & Milestones

| Milestone | Ngày dự kiến | Deliverables | Điều kiện hoàn thành |
|-----------|-------------|--------------|----------------------|
| Spec hoàn chỉnh | | | |
| Dev complete | | | |
| UAT | | | |
| Go-live | | | |

---

## 9. Open Questions

| # | Câu hỏi | Owner | Blocking? | Cần trả lời trước |
|---|---------|-------|-----------|-------------------|
| 1 | | | | |

---

## v2 — Template additions (ID-first)

### Block đầu file (ngay sau tiêu đề PRD)
```
> **Phạm vi đã chốt:**
> - Vấn đề: [1 dòng]
> - Kết quả mong muốn: [1 dòng]
> - Ngoài phạm vi: [1 dòng]
> - Quyết định đã chốt: [1 dòng]
```
### Feature List — thêm cột ID + "Cần vẽ Figma?"
| ID | Tính năng | Mô tả ngắn | Priority | Cần vẽ Figma? |
|---|---|---|---|---|
| F-01 | | | P0 | Yes / No / Critical-only |
### Goals / Screen Map / Risks / Assumptions dùng ID: `BG-01`, `SCR-01`, `R-01`, `A1` (zero-padded).

### v2 — Frontmatter trace/freshness (thêm vào block đầu file)
```
stale_status: current
linked_ids: [BG-01, F-01, SCR-01]
produces_for: [04-spec--*.md]
changelog:
  - [YYYY-MM-DD] | write-prd | initial
```
