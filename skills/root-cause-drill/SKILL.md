---
name: root-cause-drill
description: >
  Drills down to the root cause of a Change Request using 5 Whys and JTBD framework.
  Challenges the proposed solution to find the underlying problem, then produces a
  Root Cause Brief ready for the problem-statement skill.
  Use after intake-qualification classifies a request as Change Request, or when someone says
  "root cause drill", "tại sao cần change này", "drill down CR", "challenge yêu cầu này",
  "5 whys", or "tìm root cause cho change request".
  Output is in Vietnamese.
version: 1.0.0
---

# Root Cause Drill Skill

## Handoff Input
**Receives from:** `intake-qualification`
**Expected artifact:** Qualified Ticket (`artifact: Qualified Ticket`, `status: Approved`)
**Required fields:** Loại yêu cầu = "Change Request", Mô tả yêu cầu (CR as stated), Người dùng bị ảnh hưởng
**Hard gate:** Nếu Loại yêu cầu = "New Feature" → từ chối, chuyển sang `5w2h-interview`

Peels back a Change Request (áp đặt) to find the real problem underneath.
Uses 5 Whys + JTBD to test whether the proposed solution is the right one,
or whether a different approach might serve the user better.

Output is in **Vietnamese**.

## Why this skill exists

Change Requests arrive already framed as solutions ("add button X", "change logic Y").
The risk: building the wrong thing because the real problem was never examined.

This skill challenges the "what" to uncover the "why" — enabling better solution decisions.

## Key principle

A Change Request describes **one possible solution**. This skill finds the underlying
**job to be done** — which may have better solutions than what was requested.

The output is NOT a rejection of the CR. It's deeper understanding that enables
a smarter decision: build as requested / build differently / reject / defer.

## Input

- Qualified ticket from intake-qualification (Change Request classification)
- Any additional context: screenshots, error reports, user complaints
- Existing feature documentation if available

## Workflow

### Step 1 — Restate the proposed change
Extract and restate clearly:
- What change is being requested?
- To which existing feature/screen/behavior?
- Who is requesting it and why now?

### Step 2 — Run 5 Whys
Starting from the proposed change, ask "Why?" 5 times.
Goal: reach the underlying user need or business problem.

See `references/five-whys-guide.md` for technique and examples.

Each "why" should be:
- Grounded in what's known (don't invent)
- Pushed further if the answer is still a solution, not a problem
- Stopped when reaching a genuine user need or business outcome

### Step 3 — Apply JTBD lens
Reframe the finding as a Job to Be Done:
"When [situation], users want to [motivation], so they can [outcome]."

Test: Does the original CR fully solve this JTBD? Partially? Or is there a better solution?

### Step 4 — Evaluate the CR
Based on 5 Whys + JTBD, assess:
- **Build as requested**: CR directly solves the root cause, no better alternative obvious
- **Build differently**: Root cause identified, original CR is suboptimal
- **Needs more discovery**: Root cause unclear, requires user interview first (→ 5w2h-interview)
- **Reject**: Root cause is not a real problem, or CR would not solve it

### Step 5 — Broader Impact Analysis
Mở rộng góc nhìn ra ngoài phạm vi của CR gốc. Đây không phải bước kỹ thuật — mà là bước chiến lược để BA thấy được giá trị và rủi ro thực sự của change này.

Phân tích 3 chiều:

**5a. Cross-system impact — Ảnh hưởng tới hệ thống**
- Tính năng/module nào khác có thể bị ảnh hưởng nếu CR này được thực hiện?
- Có data model, API, hoặc business rule nào chia sẻ chung không?
- Change này có thể phá vỡ hành vi hiện tại của tính năng nào không?
- Có tính năng nào đang depend vào behavior sắp thay đổi không?

**5b. Cross-customer impact — Ảnh hưởng tới các khách hàng khác**
- CR này đến từ 1 khách — nhưng có bao nhiêu khách khác đang gặp vấn đề tương tự?
- Nếu áp dụng cho tất cả: ai được lợi, ai có thể bị ảnh hưởng tiêu cực?
- Có khách hàng nào đang dùng behavior hiện tại như một feature không?
- Nếu là config/toggle per customer: có đáng không, hay nên thành standard?

**5c. Cross-vertical impact — Ảnh hưởng tới các ngành hàng khác**
- Hệ thống đang phục vụ nhiều ngành (FMCG, pharma, phân phối...): change này có ý nghĩa gì với từng ngành?
- Có ngành nào được lợi đặc biệt, hoặc bị ảnh hưởng tiêu cực không?
- Root cause này có phải là vấn đề phổ biến của ngành (industry-wide problem) không?
- Nếu đây là vấn đề phổ biến → có thể nâng thành product feature thay vì custom change không?

**Output của Step 5:** Không phải danh sách exhaustive — chỉ highlight những điểm đáng chú ý nhất (tối đa 3-5 bullet mỗi chiều). Nếu không có tác động rõ ràng → ghi "Không xác định được tác động — cần thêm context."

### Step 6 — Write Root Cause Brief
Follow format in `references/five-whys-guide.md` (§ Root Cause Brief Format).

## Output File

After BA validates and approves (only when Recommendation ≠ "Needs more discovery"):

1. Set `status: Approved` in the metadata header
2. Check if `artifacts/[folder]/02-research.md` already exists:
   - **Không tồn tại:** Write file mới với nội dung đầy đủ
   - **Đã tồn tại:** Read nội dung hiện tại, rồi **append** section mới xuống cuối:
     ```
     ---
     ## Root Cause Brief — [date]
     [nội dung brief]
     ```
3. Notify BA: `✓ Saved to artifacts/[folder]/02-research.md`

If Recommendation = "Needs more discovery" → do NOT save as Approved. Route to `/5w2h-interview` first.

## Quality Control

| Check | Criteria |
|---|---|
| 5 Whys | At least 4 "why" levels, doesn't stop at a solution |
| JTBD | Clear situation / motivation / outcome structure |
| Recommendation | Explicit: build as-is / build differently / discover more / reject |
| Evidence | Every claim tied to what's known, not invented |
| No bias | Doesn't automatically validate the requested change |
| Broader Impact | 3 chiều được phân tích (system / customer / vertical), hoặc giải thích rõ tại sao không có tác động |
| Next step | Clear path: which skill or action follows |

## HITL Checklist

BA validates before proceeding:
- [ ] 5 Whys chain logic đúng không? Có bước nào bị nhảy cóc không?
- [ ] JTBD statement có phản ánh đúng nhu cầu thực sự không?
- [ ] Recommendation có hợp lý với business context không?
- [ ] Broader Impact — các tác động được identify có đúng không? Có điểm nào bị bỏ sót không?
- [ ] Nếu "Build differently" — BA có đồng ý không?
- [ ] Nếu "Needs more discovery" — approve để chuyển sang 5w2h-interview?
- [ ] Approve Root Cause Brief để feed vào problem-statement?
