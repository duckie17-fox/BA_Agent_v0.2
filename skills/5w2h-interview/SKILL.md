---
name: 5w2h-interview
description: >
  Generates a structured 5W2H interview guide for New Feature discovery,
  then synthesizes interview notes into a Context Brief ready for problem-statement skill.
  Use after intake-qualification classifies a request as New Feature, or when someone says
  "5W2H", "interview yêu cầu này", "khai thác context", "tạo câu hỏi interview",
  "viết Context Brief", or "tổng hợp notes interview".
  Has two modes: GUIDE mode (generate questions) and BRIEF mode (synthesize notes into brief).
  Output is in Vietnamese.
---

# 5W2H Interview Skill

## Handoff Input
**Receives from:** `intake-qualification`
**Expected artifact:** Qualified Ticket (`artifact: Qualified Ticket`, `status: Approved`)
**Required fields:** Loại yêu cầu = "New Feature", Mô tả yêu cầu, Người dùng bị ảnh hưởng
**Mode selection:** GUIDE nếu có thể interview stakeholder — RESEARCH nếu không tiếp cận được hoặc gấp
**Cũng nhận từ:** `root-cause-drill` kèm Root Cause Brief (`handoff-to: 5w2h-interview`) → tự động dùng GUIDE mode, tập trung vào gaps được flag

Supports New Feature discovery in three modes:

- **GUIDE mode**: Generate a tailored 5W2H interview guide based on the ticket
- **BRIEF mode**: Synthesize raw interview notes into a structured Context Brief
- **RESEARCH mode**: When interview is not possible — generate a research brief for search agent to gather equivalent information from external sources

Output is in **Vietnamese**.

## Three modes

### GUIDE mode
Triggered when: BA has a qualified New Feature ticket and needs interview questions.
Input: Qualified ticket from intake-qualification.
Output: Tailored interview guide — questions grouped by 5W2H dimension.

### BRIEF mode
Triggered when: BA has completed interviews and needs to synthesize notes.
Input: Raw interview notes (any format — bullet points, transcript, freetext).
Output: Context Brief structured for problem-statement skill.

### RESEARCH mode
Triggered when: BA cannot conduct interviews (no access to stakeholders, time pressure,
early-stage exploration, or need to validate assumptions before interviewing).
Input: Qualified ticket from intake-qualification (same as GUIDE mode).
Output: Research brief with specific search queries per 5W2H dimension — designed as
direct input for search agent.

Ask BA which mode they need if not clear from context.
If BA says they cannot interview, proactively suggest RESEARCH mode.

## GUIDE mode workflow

### Step 1 — Understand the domain
Read the qualified ticket. Identify:
- What domain is this in? (Trade marketing / CS ops / finance / etc.)
- Who are the likely interviewees? (end users / CS team / managers)
- What's already known vs. unknown?

### Step 2 — Generate tailored questions
Do NOT use generic 5W2H questions. Tailor every question to the specific context.
See `references/question-patterns.md` for patterns per dimension.

Generate 3-5 questions per dimension. Mark priority: ⭐ Must ask / Optional.
Total: 15-25 questions, ordered by conversation flow (not by dimension).

### Step 3 — Add interview tips
Include practical tips for this specific interview:
- Who to interview first
- What artifacts to request (screenshots, data, existing workarounds)
- What to watch for (emotional signals, workaround complexity)

## RESEARCH mode workflow

When interview is not feasible, convert 5W2H questions into search queries
so the search agent can gather proxy information from external sources.

### Step 1 — Identify knowledge gaps per dimension
Read the qualified ticket. For each 5W2H dimension, determine:
- What's already known from the ticket?
- What would we normally learn from interviews?
- What can plausibly be found via web research?

### Step 2 — Generate research brief
For each 5W2H dimension, produce:
- **Known**: What we already have from the ticket
- **Gap**: What we'd need from interviews
- **Search queries**: 2-3 specific queries to find proxy data externally
- **Target sources**: Where to look (forums, case studies, industry reports, competitor docs)

See `references/research-brief-format.md` for output structure.

### Step 3 — Flag interview-only gaps
Some information can ONLY come from internal interviews (e.g., exact internal process steps,
internal KPIs, specific workaround details). Flag these clearly so BA knows what
the search agent CANNOT fill in — these become mandatory follow-up questions.

### Step 4 — Output
Produce a research brief ready to be handed to search agent, plus a short list of
"must-interview" questions that no external research can answer.

## BRIEF mode workflow

### Step 1 — Parse notes
Read raw interview notes. Extract facts, don't invent.
Flag anything inferred vs. directly stated.

### Step 2 — Structure into Context Brief
Map extracted info to 5W2H dimensions.
See `references/question-patterns.md` (§ Context Brief Format) for output structure.

### Step 3 — Identify gaps
Note what's still unknown. These become Open Questions in the Brief.

## Output File

After BA validates and approves:

**GUIDE mode:** Không lưu file — interview guide chỉ dùng trong conversation, không phải artifact pipeline.

**BRIEF mode và RESEARCH mode** (cả hai đều lưu vào `02-research.md`):
1. Set `status: Approved` in the metadata header
2. Check if `artifacts/[folder]/02-research.md` already exists:
   - **Không tồn tại:** Write file mới với nội dung đầy đủ
   - **Đã tồn tại** (ví dụ: root-cause-drill đã ghi trước): Read nội dung hiện tại, rồi **append** section mới xuống cuối:
     ```
     ---
     ## Context Brief — [date]
     [nội dung brief]
     ```
3. Notify BA: `✓ Saved to artifacts/[folder]/02-research.md`

## Quality Control — GUIDE mode

| Check | Criteria |
|---|---|
| Tailored | No generic questions — every question references the specific domain/context |
| Coverage | All 7 dimensions covered (Who, What, Where, When, Why, How, How much) |
| Priority marked | ⭐ on must-ask questions |
| Conversation flow | Questions ordered naturally, not by dimension headers |
| Artifacts listed | Interview asks for data/screenshots/workaround examples |

## Quality Control — BRIEF mode

| Check | Criteria |
|---|---|
| No invention | Every fact traces to interview notes |
| Gaps flagged | Unknown info listed as Open Questions |
| Problem vs solution | Brief describes problem, not solution |
| Confidence stated | Low/Medium/High based on evidence quality |
| Ready for next | Brief is self-contained input for problem-statement skill |

## Quality Control — RESEARCH mode

| Check | Criteria |
|---|---|
| All dimensions covered | 7 dimensions each have Known/Gap/Search queries |
| Queries specific | No generic queries — each references the domain and problem |
| Sources targeted | Each dimension suggests where to look |
| Interview-only gaps flagged | Items that require internal interviews clearly marked |
| Actionable for search agent | Brief can be handed directly to search agent without extra context |

## HITL Checklist

**After GUIDE:** BA validates before running interviews:
- [ ] Questions relevant to this specific context?
- [ ] Missing any important angle?
- [ ] Right people identified to interview?

**After BRIEF:** BA validates before using as input for problem-statement:
- [ ] Brief accurately reflects what was said in interviews?
- [ ] Any important nuance lost in synthesis?
- [ ] Open Questions list complete?
- [ ] Approve to feed into problem-statement skill?

**After RESEARCH:** BA validates before handing to search agent:
- [ ] Search queries có đúng hướng và đủ cụ thể không?
- [ ] Có dimension nào nên bỏ qua research vì chỉ có thể biết từ internal?
- [ ] "Must-interview" questions đã capture đủ chưa?
- [ ] Approve để chuyển research brief cho search agent?
