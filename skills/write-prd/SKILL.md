---
name: write-prd
description: >
  Writes a lightweight PRD (Product Requirements Document) at the epic level —
  covering problem, goals, feature list, use cases, success metrics, timeline,
  and open questions. Use when starting a new epic or feature group, aligning
  stakeholders on scope before going into detailed specs, or when someone says
  "write a PRD", "tạo PRD", "viết tài liệu tổng quan tính năng", "document epic này",
  or "mình cần PRD cho [feature/epic]".
  Output is in Vietnamese. Do NOT use for detailed screen-level specs — use write-spec for that.
version: 1.0.0
---

# Write PRD Skill

## Handoff Input
**Receives from:** `problem-statement`
**Expected artifact:** Problem Statement (`artifact: Problem Statement`, `status: Approved`)
**Required fields:** Context (2-3 câu, không có solution), HMW Question (có constraint), User Story (đủ 3 phần: As a / I want to / So that), Confidence ≠ Thấp — hoặc nếu Thấp thì HITL đã sign off
**Hard gate:** Nếu `status` ≠ Approved → yêu cầu BA hoàn thành HITL trước khi tiếp tục

Writes a lightweight PRD at the **epic level** — gives stakeholders a clear picture of
what we're building, why, and what success looks like. Not a screen-by-screen spec.

All output is in **Vietnamese**.

## Relationship to other skills

```
write-prd (this skill)     →  [HITL approve]  →  write-spec (per feature/screen)
Epic overview, use cases       BA reviews           Detailed field specs, diagrams, data model
```

## When to use

Use this skill **before** write-spec. It answers "what and why" at the epic level.
write-spec answers "how exactly" at the screen/feature level.

## Input to collect

Ask one at a time. Extract from existing documents if provided.

1. **Epic/feature name** — what is this group of features called?
2. **Problem** — what problem does this solve, for whom?
3. **Goals** — what outcomes do we want? (ask for 3-5)
4. **Non-goals** — what are we explicitly NOT doing in this version?
5. **Feature list** — what features are in scope? (high level names only)
6. **System Overview & Flow** — loại ticket là New Feature hay CR?
   - New Feature: hệ thống nào tham gia (BO / Portal / BBS App / AI Engine)? Luồng chính qua các hệ thống ra sao? Chức năng nào thuộc hệ thống nào?
   - CR: luồng hiện tại (As-Is) đang chạy như thế nào? Thay đổi ở bước nào (To-Be)?
7. **Screen Map & Navigation** — các features/views kết nối với nhau như nào? Navigation pattern là gì (tabs / sidebar sub-nav / drill-through)? UI element nào dùng chung qua tất cả views (shared filter bar, shared header, tab bar)?
8. **Use cases** — what can users do with this? (A, B, C level)
9. **Success metrics** — how will we know it worked?
10. **Timeline** — key milestones and target dates
11. **Open questions** — what's still unresolved?

## Workflow

### Step 1 — Understand scope
Confirm whether the input is a single feature or a group of related features (epic).
If it's a single small feature, suggest using write-spec directly instead.

### Step 2 — Gather missing information
Ask for what's missing. If a problem statement or brainstorm output is provided, extract from it.

### Step 3 — Write the PRD
Follow the output format below.
Reference `references/prd-guide.md` for section-by-section guidance.
Reference `assets/prd-template.md` for blank template.

### Step 3.5 — Generate & export System Overview diagram

After writing Section 3 of the PRD, generate the diagram as PNG and upload to Confluence (nếu BA có cung cấp page ID hoặc page URL).

**Diagram style — giữ đơn giản, không màu sắc:**

```
%%{init: {
  "theme": "default",
  "themeVariables": {
    "fontSize": "22px",
    "fontFamily": "arial"
  }
}}%%
flowchart LR
    ...
```

**Export command (resolution cao, font sắc nét) — chỉ chạy nếu có Node.js/npx:**
```
npx @mermaid-js/mermaid-cli -i [input.mmd] -o [output.png] -w 1600 -s 2
```
Nếu npx không có → bỏ qua bước export PNG, giữ diagram dạng Mermaid code trong PRD. BA có thể render thủ công sau.

**Upload lên Confluence:**
1. Dùng `confluence_upload_attachment` để lấy upload URL
2. Dùng `curl -X POST -F "file=@[path]" [upload_url]` để upload file
3. Dùng `confluence_update_page` để embed ảnh vào trang

Nếu BA không cung cấp Confluence page → chỉ lưu file PNG vào folder `artifacts/[feature-id]/` và thông báo đường dẫn.

### Step 4 — Self-check
Run the Quality Control checklist before outputting.

## Output File

After BA validates and approves:

1. Set `status: Approved` in the metadata header
2. Save to: `artifacts/[folder]/03-planning--prd.md`
3. Save diagram PNG to: `artifacts/[folder]/03-planning--diagram.png` (nếu chưa upload Confluence)
4. Notify BA: `✓ Saved to artifacts/[folder]/03-planning--prd.md`

## Output format

All output in **Vietnamese**. Structure:

```
# PRD: [Epic/Feature Name]

## 1. Problem Statement
## 2. Goals & Non-Goals
## 3. System Overview & Flow          ← New Feature: system diagram + function list
                                       ← CR: As-Is / To-Be flow
## 4. Feature List
## 5. Screen Map & Navigation
## 6. Use Cases
## 7. Success Metrics
## 8. Timeline & Milestones
## 9. Open Questions
```

See `references/prd-guide.md` for what goes in each section.

## Quality Control

| Check | Criteria |
|---|---|
| Problem Statement | Mô tả rõ ai gặp vấn đề gì, tần suất, hậu quả nếu không giải quyết |
| Goals | Mỗi goal có thể đo được, không phải activity |
| Non-Goals | Ít nhất 2 non-goals cụ thể, có lý do |
| System Overview & Flow | New Feature: có diagram hệ thống + function list; CR: có cả As-Is và To-Be; diagram không quá 8 node |
| Feature List | Level cao — tên tính năng, không mô tả chi tiết màn hình |
| Screen Map & Navigation | Navigation pattern được define rõ (tabs/sidebar/drill-through); shared shell elements được liệt kê; screen map thể hiện quan hệ giữa các màn |
| Use Cases | Format "Người dùng có thể [hành động]", không phải steps |
| Success Metrics | Có target cụ thể, có phương pháp đo |
| Timeline | Có ít nhất 2 milestone, có ngày/tuần cụ thể |
| Open Questions | Có owner được gán, có flag blocking hay không |

## Confidence Level

```
Confidence: [High / Medium / Low]
Missing information: [List]
Assumptions made: [List — need user confirmation]
```

## HITL Checklist

After receiving draft, user validates:
- [ ] Problem Statement đúng với vấn đề thực tế không?
- [ ] System Overview diagram thể hiện đúng hệ thống tham gia và luồng chính không? (New Feature: đủ BO/Portal/BBS/AI? CR: As-Is và To-Be đúng chưa?)
- [ ] Feature List có đủ không? Có tính năng nào bị bỏ sót?
- [ ] Screen Map & Navigation define đúng pattern chưa? Shared elements (filter bar, tab bar) có được liệt kê không?
- [ ] Use Cases cover đủ các nhóm người dùng chính không?
- [ ] Non-Goals có ngăn scope creep đúng chỗ không?
- [ ] Success Metrics có thực sự đo được không?
- [ ] Timeline có realistic không?
- [ ] Approve để chuyển sang write-spec cho từng feature?

## v2 — ID-first PRD (tiền đề của impact-analysis)

Bổ sung KitA_v2. Mục tiêu: mọi node trace được bằng ID → impact-analysis map chính xác.
1. **Block "Phạm vi đã chốt"** (4 dòng đầu PRD, ngay sau tiêu đề): Vấn đề · Kết quả mong muốn · Ngoài phạm vi · Quyết định đã chốt.
2. **ID-first cho mọi bảng:** Goals `BG-01..`, Feature List `F-01..`, Screen Map `SCR-01..`, Risks `R-01..`, Assumptions `A1..` (zero-padded thống nhất). Downstream (spec/impact) trace bằng các ID này.
3. **Feature List thêm cột "Cần vẽ Figma?"** (Yes / No / Critical-only) → design-agent chỉ vẽ màn có chủ đích, CR nhỏ không lãng phí.
4. Khi CR đổi scope → route qua `impact-analysis` (ID cho biết F/SCR nào stale) trước khi sửa PRD.
> Fix v2: đã sửa lỗi đánh số trùng "7" trong Input list; `references/prd-guide.md` numbering đồng bộ với SKILL/template.
