---
name: intake-qualification
description: >
  Qualifies incoming feature requests or change requests at the intake stage.
  Checks Definition of Ready (DoR), detects duplicates with existing backlog,
  classifies as New Feature or Change Request, and suggests priority level.
  Use as the FIRST step when a new ticket or request arrives, before any research
  or brainstorm. Trigger when someone says "có yêu cầu mới", "ticket mới vào",
  "qualify yêu cầu này", "check DoR", or pastes a raw ticket/request description.
  Output is in Vietnamese.
---

# Intake Qualification Skill

First gate in the BA pipeline. Qualifies raw requests before any analysis begins.
Prevents wasted effort on incomplete, duplicate, or mis-classified requests.

Output is in **Vietnamese**.

## What this skill does

1. **Extracts** structured information from raw ticket text
2. **Checks DoR** — is the request ready to be worked on?
3. **Classifies** — New Feature or Change Request?
4. **Flags duplicates** — asks BA to check against known backlog
5. **Suggests priority** — P0/P1/P2 based on signals in the request
6. **Outputs** a qualified ticket summary ready for next step

## Input

Accept any of:
- Raw ticket text pasted by BA
- Jira ticket link + description
- Verbal description from BA
- Email or chat message from Sales/CS/customer

## Workflow

### Step 1 — Clarify input (TRƯỚC KHI làm bất cứ gì khác)

Hoạt động như một **senior BA** — không fill form, mà khai thác yêu cầu theo 2 lớp song song:

#### Lớp 1 — Dependency check
Trước khi hỏi bất cứ gì, tự hỏi: *"Request này cần gì trước đó mới work được?"*
Xác định chain phụ thuộc của request (data → logic → UI → output).
Nếu có dependency chưa rõ → đây là câu hỏi ưu tiên hỏi đầu tiên.

Ví dụ: "dashboard SOS" → chain: `data chụp ảnh → AI nhận diện → tính SOS → hiển thị`
→ Câu hỏi đầu: *"Brand này đang dùng AI evaluation chưa?"* — nếu chưa thì request là premature.

#### Lớp 2 — Hypothesis-first questioning
Thay vì hỏi open-ended ("vấn đề là gì?"), đưa ra **2-3 hypothesis cụ thể** dựa trên pattern của loại request, để BA/requester confirm hoặc bác bỏ.

Pattern phổ biến theo loại request:
- **Dashboard/Report request** → thường ẩn 1 trong 3: (A) thiếu data, (B) có data nhưng không xem được, (C) xem được nhưng không hiểu được
- **Thêm tính năng mới** → thường ẩn 1 trong 3: (A) workflow hiện tại bị broken, (B) thiếu automation, (C) feature parity với competitor
- **Thay đổi behavior hiện tại** → thường ẩn 1 trong 3: (A) edge case không được handle, (B) business rule thay đổi, (C) user mental model khác với system

#### Branching logic
Câu trả lời của mỗi hypothesis quyết định câu hỏi tiếp theo — tối đa **3 câu, có thứ tự**:
1. **Confirm dependency** — cái này có chạy được không với hệ thống hiện tại?
2. **Identify root type** — đây là thiếu data, thiếu UI, hay thiếu logic?
3. **Xác định scope** — detail quan trọng để phân biệt NF vs CR, hoặc small vs large

**Quy tắc:**
- Tối đa 4 câu, gom vào 1 lần duy nhất — không hỏi rải rác
- Không hỏi "vấn đề thực sự là gì?" — đây là câu hỏi lazy, defensive, và không extract được signal
- Urgency và "1 khách vs nhiều khách" → tự infer từ ngôn ngữ trong ticket, không hỏi
- Chỉ tiếp tục Step 2 sau khi BA đã trả lời

Format:
> Trước khi qualify, mình cần làm rõ thêm:
> 1. [dependency check hoặc hypothesis A vs B]
> 2. [branching từ câu 1 nếu cần]
> ...

### Step 2 — Extract structured info
Sau khi đã clarify, parse toàn bộ thông tin và extract:
- **Requester**: who sent this? (Sales / CS / Customer / Internal)
- **Channel**: how did it arrive? (Jira / Email / Chat / Meeting)
- **Problem description**: what are they describing?
- **Proposed solution** (if any): are they asking for something specific?
- **Affected users**: who is impacted?
- **Urgency signals**: any deadline, customer threat, revenue mention?

### Step 3 — Classify: New Feature vs Change Request
See `references/classification-guide.md` for decision rules.

**New Feature** (mơ hồ): Request describes a problem or need without specifying solution.
**Change Request** (áp đặt): Request specifies a concrete change to existing functionality.

If unclear: flag as ambiguous, explain why, ask BA to confirm.

### Step 4 — Related features analysis
Cross-reference với danh sách tính năng hiện có trong CLAUDE.md (section "Các tính năng chính").
Xác định:
- **Tính năng liên đới trực tiếp**: tính năng nào có thể bị ảnh hưởng nếu request này được thực hiện?
- **Tính năng liên đới gián tiếp**: tính năng nào dùng chung data/flow với request này?
- **Dependency**: request này có phụ thuộc vào tính năng nào đang có không?

Nếu không có liên đới rõ ràng: ghi "Không xác định được liên đới — cần thêm context."

### Step 5 — DoR Check
Run against Definition of Ready checklist in `references/dor-checklist.md`.
Mark each criterion as ✅ Pass / ⚠️ Partial / ❌ Fail.
Count: if < 4 Pass → Not Ready, return to requester.

### Step 6 — Duplicate detection
**Pipeline mode (ba-lead):** ba-lead đã tự chạy `jira_search` trước khi invoke skill này và lưu kết quả vào `00-external-context.md`. Đọc kết quả từ file đó thay vì hỏi BA.
**Standalone mode:** Skill không truy cập Jira trực tiếp — list 2-3 search queries để BA tự chạy trong Jira/backlog.

### Step 7 — Priority suggestion
Score P0/P1/P2 based on signals. See `references/priority-guide.md`.

### Step 8 — Output qualified ticket
Follow format in `references/output-format.md`.

**QUAN TRỌNG:** Ở bước này KHÔNG generate research brief và KHÔNG suggest next step cụ thể. Chỉ output qualified ticket và HITL checklist. Research brief chỉ được generate SAU KHI BA approve (xem Output File section).

## Output File

After completing HITL and BA approves:

1. Set `status: Approved` in the metadata header
2. Derive folder name: `[ticket-id]--[feature-slug]` (e.g., `VPR-042--display-rules`). If ticket-id not yet assigned, use `DRAFT-YYYYMMDD--[feature-slug]`
3. **Append research brief** vào cuối file (xem format trong `references/output-format.md` — section "Post-Approval Append")
4. Save to: `artifacts/[folder]/01-qualified-ticket.md`
5. Notify BA: `✓ Saved to artifacts/[folder]/01-qualified-ticket.md`
6. Suggest next step:
   - **Nếu New Feature:** → `5w2h-interview`
   - **Nếu Change Request:** → `root-cause-drill`

## Quality Control

| Check | Criteria |
|---|---|
| Clarification done | Dependency check đã chạy; hypothesis được confirm hoặc bác bỏ trước khi qualify |
| Classification | Clearly stated as New Feature or Change Request with reason |
| Related features | Ít nhất 1 tính năng liên đới được xác định (hoặc giải thích tại sao không có) |
| DoR result | Each criterion explicitly marked Pass/Partial/Fail |
| Action clear | If Not Ready: clear list of what's missing |
| Duplicate queries | At least 2 specific Jira search queries provided |
| Priority | P0/P1/P2 with reasoning, not just a label |
| No premature next step | Research brief và routing KHÔNG xuất hiện trong Draft output |

## HITL Checklist

BA validates before proceeding:
- [ ] Thông tin đầu vào đã đủ và chính xác chưa?
- [ ] Classification đúng không? (New Feature vs Change Request)
- [ ] Các tính năng liên đới có được xác định đúng không?
- [ ] DoR assessment có chính xác không?
- [ ] Đã check duplicate trong backlog chưa?
- [ ] Priority suggestion hợp lý với business context hiện tại không?
- [ ] Approve để tiếp tục pipeline?

## v2 — Pipeline Depth (lite / full)

Sau khi classify, đề xuất **độ sâu pipeline** để CR nhỏ không chạy full 7 bước:
| Depth | Khi nào | Route |
|---|---|---|
| **lite** | CR nhỏ, scope rõ, đụng 1 feature đã có artifact | `impact-analysis` → spec update (skip PRD + discovery đầy đủ + design nếu không đổi UI) |
| **full** | New Feature, hoặc CR đổi scope/epic/nhiều màn | Pipeline đầy đủ intake→...→design→story |
Ghi `pipeline-depth: lite|full` vào qualified ticket. ba-lead route theo field này.
