---
name: impact-analysis
description: >
  Phân tích tác động (impact) của một Change Request / correction TRƯỚC khi sửa bất kỳ
  artifact nào. Analysis-only — KHÔNG mutate. Map thay đổi về artifact sở hữu hẹp nhất,
  liệt kê artifact bị stale + lệnh/agent rerun chính xác, ghi staleness registry.
  Dùng khi: CR mới vào, câu correction trần ("bỏ nhóm admin", "đổi default X"),
  hoặc trước khi spec-agent rewrite section. Trigger: "phân tích impact", "impact CR này",
  "đổi cái này ảnh hưởng gì", "cái gì stale nếu sửa X".
  Output tiếng Việt.
version: 1.0.0
---

# Impact Analysis Skill

> Nguồn cơ chế: ba-kit-solo `core/behavior/impact.md` + `core/workflows/impact.md` + `templates/impact-report-template.md` + `templates/shared-staleness-template.md` (đọc verbatim 2026-07-03). Port sang cấu trúc **flat SSOT** của Kit A (04-spec là SSOT) + multi-agent + NDS-Knowledge. KHÔNG port engine lifecycle/ASCII-canon của Kit B.

## Nguyên tắc cốt lõi (fail-closed)
- **Analysis-only.** Skill này KHÔNG sửa artifact. Chỉ phân tích → xuất báo cáo → đề xuất rerun.
- **Cấm mutate từ câu correction trần.** Một câu như "Không có nhóm admin" → route vào đây TRƯỚC, không sửa spec trực tiếp (khớp `pushback-triggers.md` MEM-PBK-04).
- Map mỗi thay đổi về **lớp sở hữu hẹp nhất**, ưu tiên sửa theo **ID** thay vì rewrite cả file.

## Handoff Input
**Receives from:** ba-lead (nhánh CR) hoặc BA standalone khi có correction/CR trên feature đã có artifact.
**Cần có:** folder feature `artifacts/[ID]--[slug]/` đã tồn tại (ít nhất 04-spec hoặc 03-planning--prd) + phát biểu thay đổi.
**Nếu thiếu phát biểu thay đổi:** hỏi đúng 1 câu — *"Thay đổi là gì? Dán yêu cầu/correction/rule mới, mình phân tích impact."*

## Lớp sở hữu (owning layers — flat SSOT Kit A)
| Loại thay đổi | Artifact sở hữu | Rerun bởi |
|---|---|---|
| Đổi scope / phân loại NF-CR | `01-qualified-ticket.md` | intake-qualification |
| Đổi goal / feature list / navigation epic | `03-planning--prd.md` (ID: BG/F/SCR/R) | write-prd (spec-agent PRD) |
| Đổi business rule / field / flow / edge case | `04-spec--[feature].md` (ID: BR-*/FR-*) | write-spec (spec-agent SPEC) |
| Đổi màn hình / state / navigation active | `05-design--[feature].md` + `05-design-state.md` | design-instruction (design-agent) |
| Đổi acceptance criteria / story | `06-stories/[feature].md` | write-story (delivery-agent) |
| Đổi rule/message dùng chung | `_shared/common-rules.md` / `message-list.md` (CR-*/MSG-*) | Lead BA (reviewer approve) |

## Workflow

### Step 1 — Resolve target
Xác định đúng feature folder. Nếu nhiều folder khớp mơ hồ → dừng, hỏi BA chọn (KHÔNG đoán theo mtime — MEM-PBK, khớp Kit B `never_use_mtime`).

### Step 2 — Đọc source of truth
Đọc `04-spec--[feature].md` (SSOT) + `03-planning--prd.md` (Section 4 + ID grammar). Nếu cần business rule hiện tại → đọc `02-NDS-Knowledge-context.md` (check `stale_status`; quá hạn/unknown → search_wiki lại). KHÔNG đọc lan man ngoài phạm vi thay đổi.

### Step 3 — Classify change → owning layer
Dùng bảng "Lớp sở hữu". Xác định artifact **hẹp nhất** sở hữu node bị đổi (theo ID nếu PRD/spec đã ID-first).

### Step 4 — Trace downstream (ai bị ảnh hưởng)
Từ node đổi, quét xuôi theo chuỗi ID: **PRD (BG/F/SCR)** → **Spec (BR/FR)** → **Design (SCR)** → **Story (AC)**. Với mỗi artifact downstream tham chiếu node đổi → đánh **confirmed** (trace ID trùng) hoặc **possible** (liên quan module nhưng chưa xác nhận). Kế thừa "3 chiều" từ `root-cause-drill` Step 5 (cross-system / cross-customer / cross-vertical) nếu CR có scope rộng.

### Step 5 — Xuất Impact Report
Ghi `artifacts/[folder]/impacts/CR-00X-impact.md` theo format ở `references/impact-format.md`:
Change Summary → Affected Artifacts (bảng: artifact / impact type / confirmed|possible / rerun bởi agent nào) → Recommended Rerun Sequence (hẹp nhất trước) → Open Questions.

### Step 6 — Ghi Staleness Registry
Cập nhật `artifacts/[folder]/_staleness.md` (format ở `references/impact-format.md`): mỗi artifact stale + stale-since + lý do (CR ID) + lệnh/agent refresh + status. Reviewer check registry này trước khi flip Approved lại.

### Step 7 — Return (KHÔNG mutate)
Trả ba-lead: report path + rerun sequence + open questions. Việc sửa thật do agent tương ứng thực hiện SAU khi được duyệt rerun (Accepted-Scope: đã duyệt bước nào → khóa bước đó).

## Quality Control
| Check | Criteria |
|---|---|
| Analysis-only | KHÔNG có artifact nào bị sửa trong lần chạy này |
| Owning layer | Mỗi thay đổi map về artifact hẹp nhất, không rewrite cả project |
| Downstream trace | Có phân loại confirmed vs possible; dùng ID nếu có |
| Rerun sequence | Thứ tự hẹp-trước, ghi rõ agent/skill nào chạy |
| Staleness | `_staleness.md` cập nhật với lệnh refresh chính xác |
| No guess | Thiếu thông tin → open question, không đoán (fail-closed) |

## Raise ba-lead khi
folder không resolve được · thay đổi chạm nhiều SSOT layer không rõ ranh giới · conflict wiki/ticket (MEM-PBK-03).
