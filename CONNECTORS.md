# BA Pipeline — Connectors

> File này là bản đồ tổng quan của toàn bộ pipeline BA.  
> Mỗi khi một skill output artifact, BA dùng file này để biết artifact đó đi đến đâu tiếp theo.

> **Pipeline mode vs Standalone:** Diagram dưới đây mô tả HITL gate ở mỗi bước — đây là flow khi dùng skill standalone.
> Khi chạy qua ba-lead (pipeline mode), HITL được xử lý tự động bởi @reviewer agent — BA không cần validate thủ công.

---

## Full Pipeline Flow

```
[Ticket/Request vào]
        │
        ▼
① intake-qualification
   → Output: Qualified Ticket
   → HITL: Classify đúng chưa? DoR pass? Approve?
        │
        ├── [New Feature] ──────────────────────────────────────────┐
        │                                                           │
        ▼                                                           ▼
② 5w2h-interview                                      ② root-cause-drill
   Khám phá vấn đề từ đầu                               Bóc ngược CR tìm root cause
   → Output: Context Brief                              → Output: Root Cause Brief
   → HITL: 7 chiều đầy đủ?                             → HITL: Root cause đúng? Recommendation hợp lý?
        │                                                    │
        │                                    ┌───────────────┼──────────────────────────────┐
        │                                    │               │                              │
        │                          [Needs more discovery]    │                              │
        │                                    │    [Build as requested]           [Build differently]
        │                                    │               │                              │
        │                                    └───→ 5w2h-interview                           │
        │                                               │                                  │
        └──────────────────────┬────────────────────────┘                                  │
                               ▼                                                           │
                   ③ problem-statement ◄──────────────────────────────────────────────────┘
                      Chuẩn hóa: Context + HMW + User Story
                      → Output: Problem Statement
                      → HITL: HMW đủ rộng? User Story đúng intent?
                               │
                               ▼
                   ④ write-prd
                      Epic overview: Goals, Features, Use Cases, Metrics
                      + System Overview diagram (New Feature: system flow; CR: As-Is / To-Be)
                      → Output: PRD + diagram PNG
                      → HITL: Feature List đủ? Non-goals chặn scope creep? Diagram đúng luồng/hệ thống?
                               │
                               ▼ (lặp lại cho từng feature trong Feature List)
                   ⑤ write-spec × N
                      Spec chi tiết: Mermaid diagram, field table, data model, edge cases
                      → Output: Spec (1 spec / 1 feature)
                      → HITL: Flow đúng? Fields đủ? Edge cases cover hết?
                               │
                               ▼
                   ⑥ design-instruction
                      Vẽ màn hình lên Figma qua figma-mcp-go
                      Screens grouped by user story → screenshot-ready
                      → Output: Figma screens (live) + log file 05-design--[feature].md
                      → HITL: Screens đúng flow? Brand tokens áp dụng đúng? States đủ?
                               │
                               ▼
                   ⑦ write-story (optional)
                      Viết Jira user story từ spec + Figma screenshots
                      → Input: 04-spec.md + screenshot từng story group
                      → Output: User stories sẵn sàng paste lên Jira
```

---

## Handoff Artifacts

| Artifact | Produced by | Consumed by | Status cần đạt trước handoff |
|---|---|---|---|
| Qualified Ticket | `intake-qualification` | `5w2h-interview` hoặc `root-cause-drill` | Approved |
| Context Brief | `5w2h-interview` (BRIEF mode) | `problem-statement` | Approved |
| Research Brief | `5w2h-interview` (RESEARCH mode) | Search Agent | Draft (để agent xử lý) |
| Root Cause Brief | `root-cause-drill` | `problem-statement` hoặc `5w2h-interview` | Approved |
| Problem Statement | `problem-statement` | `write-prd` | Approved |
| PRD + diagram PNG | `write-prd` | `write-spec` × N | Approved |
| Spec | `write-spec` | `design-instruction` | Approved |
| Figma Screens | `design-instruction` | `write-story` | Approved (screens live in Figma) |
| Design Log | `design-instruction` | `reviewer`, `delivery-agent` | Approved — `05-design--[feature].md`: Figma page link + story groups + screenshot paths |
| Screenshots | `design-instruction` | `delivery-agent` | `screenshots/verify-[story-slug].png` per story group |

---

## Routing Rules

### Sau `intake-qualification`
| Kết quả phân loại | Handoff đến |
|---|---|
| New Feature | `5w2h-interview` |
| Change Request | `root-cause-drill` |
| Ambiguous | Hỏi lại BA — không route cho đến khi có quyết định |
| DoR: Not Ready | Trả về requester để bổ sung thông tin |

### Sau `root-cause-drill`
| Recommendation | Handoff đến |
|---|---|
| Build as requested | `problem-statement` (kèm Root Cause Brief) |
| Build differently | `problem-statement` (kèm Root Cause Brief + alternative được đề xuất) |
| Needs more discovery | `5w2h-interview` (kèm Root Cause Brief để biết gap cần tìm) |
| Reject | Kết thúc — thông báo requester với lý do |

### Sau `write-prd`
- Gọi `write-spec` **N lần** — mỗi lần cho 1 feature trong Feature List
- Ưu tiên P0 trước, sau đó P1, P2
- Không merge nhiều feature vào 1 spec call

---

## Standard Metadata Header

Mỗi output artifact phải có block này ở đầu, trước nội dung chính:

```
---
artifact: [Qualified Ticket | Context Brief | Research Brief | Root Cause Brief | Problem Statement | PRD | Spec]
ticket-id: [Jira ID hoặc temp ID — ví dụ: VPR-001]
source-skill: [intake-qualification | 5w2h-interview | root-cause-drill | problem-statement | write-prd | write-spec]
prepared-by: [Tên BA]
date: [YYYY-MM-DD]
status: [Draft | HITL Review | Approved]
handoff-to: [tên skill tiếp theo]
---
```

**Status flow:** `Draft` → `HITL Review` → `Approved`  
Chỉ khi `status: Approved` mới được paste vào skill tiếp theo.

---

## Output File Convention

Mỗi skill sau khi generate xong phải lưu output ra file `.md` bằng Write tool.

### Cấu trúc thư mục — Flat with Single Source of Truth

**Base folder:** `artifacts/` nếu tồn tại, `docs/` nếu không, hoặc hỏi user.

**New Feature:**
```
artifacts/[FEATURE-ID]--[feature-slug]/
├── 00-decision-log.md               ← append-only, tạo ở Bước 1, cập nhật mỗi bước
├── 00-external-context.md           ← Jira source ticket + duplicate search results (tạo bởi ba-lead Bước 1)
├── _versions.md                     ← version index (tạo khi Intake Approved)
├── _meta.json                       ← pointer đến spec hiện tại
├── 01-qualified-ticket.md
├── 02-research.md                   ← NF: 5W2H Brief · CR: Root Cause Brief
├── 02-NDS-Knowledge-context.md              ← cache NDS-Knowledge context cho spec-agents
├── 03-problem-statement.md
├── 03-brainstorm--notes.md
├── 03-planning--prd.md              ← update khi CR thay đổi scope/epic
├── 04-spec--[feature].md            ← single source of truth, luôn là bản production mới nhất
├── 05-design--[feature].md          ← log: Figma page link + story groups + screens list
├── 05-design-state.md               ← design state: navigation pattern, wireframes, tokens per screen
├── 06-stories/[feature].md
└── cr/                              ← context docs cho từng CR
    ├── CR-001--[slug].md
    └── CR-002--[slug].md

Ví dụ: artifacts/VPR-001--display-rules/04-spec--display-rules.md
```

**Multiple sub-features trong 1 PRD:**
```
artifacts/[FEATURE-ID]--[feature-slug]/
├── 00-decision-log.md               ← append-only, tạo ở Bước 1, cập nhật mỗi bước
├── 03-planning--prd.md
├── 04-spec--[feature-1].md
├── 04-spec--[feature-2].md
├── 05-design--[feature-1].md        ← log Figma + story groups
├── 05-design--[feature-2].md
└── cr/
    └── CR-001--[slug].md
```

### Đặt tên folder

| Trường hợp | Tên folder |
|---|---|
| Đã có Jira ticket | `[ticket-id]--[feature-slug]` — ví dụ: `VPR-042--display-rules` |
| Chưa có ticket-id | `DRAFT-YYYYMMDD--[feature-slug]` — ví dụ: `DRAFT-20260417--display-rules` |

`feature-slug`: kebab-case từ tên epic/feature, ưu tiên tiếng Anh.  
Ví dụ: `display-rules`, `evaluation-plan`, `reason-settings`.

### Tên file theo thứ tự pipeline — Descriptive Naming

Tên file dùng quy tắc **số thứ tự + tên rõ ràng** để dễ hiểu:

| Bước | Artifact | Tên file | Mô tả |
|---|---|---|---|
| — | Decision Log | `00-decision-log.md` | Append-only log toàn bộ quyết định quan trọng qua các bước |
| — | External Context | `00-external-context.md` | Jira source ticket + duplicate search results (tạo bởi ba-lead Bước 1) |
| — | Version Index | `_versions.md` | Index tất cả version + CR |
| — | Meta Pointer | `_meta.json` | Pointer đến spec và version hiện tại |
| 1 | Qualified Ticket | `01-qualified-ticket.md` | Qualification assessment |
| 2 | Research Brief | `02-research.md` | NF: 5W2H Brief · CR: Root Cause Brief |
| 2 | NDS-Knowledge Context Cache | `02-NDS-Knowledge-context.md` | Cache NDS-Knowledge context (tạo bởi discovery-agent, đọc bởi spec-agents) |
| 3 | Problem Statement | `03-problem-statement.md` | HMW + User Story + JTBD |
| 3 | Brainstorm Notes | `03-brainstorm--notes.md` | Approach options + hướng được chọn |
| 3 | PRD | `03-planning--prd.md` | Feature planning & roadmap |
| 4 | Spec | `04-spec--[feature].md` | Feature specification |
| 5 | Design Log | `05-design--[feature].md` | Log: screens drawn, story groups, Figma page link |
| 5 | Design State | `05-design-state.md` | Navigation pattern, wireframes, token contract per screen |
| 6 | User Stories | `06-stories/[feature].md` | Ready-to-push Jira stories |

**Pipeline stages:**
- `00-decision-log` = Decision audit trail (tạo lúc Bước 1, append suốt pipeline)
- `01-intake` = Qualification
- `02-discovery` = Problem & analysis
- `03-planning` = PRD & roadmap
- `04-spec` = Feature detail (Mermaid diagram, field table, business rules)
- `05-design` = Design instruction for Figma/components
- `cr/CR-00X--[slug].md` = CR context doc (vấn đề, root cause, scope thay đổi)

### Decision Log — Format & Quy tắc

**File:** `00-decision-log.md` — append-only, không xoá entry cũ.

**Khi nào append:** Mỗi khi ba-lead hoặc agent đưa ra quyết định có impact đến output sau này — không log những gì đã rõ ràng trong artifact.

**Quyết định cần log (không log mọi thứ):**
- Routing: phân loại NF vs CR, chọn approach trong brainstorm
- Scope: feature nào vào MVP, feature nào out-of-scope, tại sao
- Review verdict: PASS / FAIL + lý do chính (không log chi tiết nhỏ)
- Escalation: khi nào escalate, lý do
- Design: navigation pattern, story grouping, quyết định sidebar/layout khác với default
- Open Questions đã được chốt (OQ resolved)

**Format mỗi entry:**

```markdown
## [Bước] · [YYYY-MM-DD] · [Agent/Skill]

**Quyết định:** [1 câu tóm tắt ngắn]
**Lý do:** [Tại sao — context, constraint, trade-off]
**Bác bỏ:** [Lựa chọn khác đã xem xét và lý do không chọn] *(nếu có)*
**Ảnh hưởng:** [Downstream impact — ai/bước nào cần biết điều này]
```

**Ví dụ entry:**

```markdown
## Bước 2 — Discovery · 2026-06-18 · discovery-agent

**Quyết định:** Chọn Approach B — Persistent Flagging thay vì Approach A (push notification)
**Lý do:** Notification infrastructure chưa confirm là có sẵn; Brand Manager đã có habit mở báo cáo. Approach B không tạo dependency kỹ thuật mới.
**Bác bỏ:** Approach A (push notification) — risk alert fatigue nếu N config sai + dependency infra chưa ready
**Ảnh hưởng:** PRD Non-Goals phải block push notification trong MVP; Engineering không cần build notification service trong Phase 1

## Bước 4 — Design · 2026-06-18 · design-agent

**Quyết định:** Story Group 2 (Plan Detail) dùng sidebar minimal thay vì full nav
**Lý do:** [ví dụ lý do không hợp lệ — entry này sẽ bị flag khi review]
**Bác bỏ:** —
**Ảnh hưởng:** Reviewer sẽ catch ở Bước 4c
```

**For multiple sub-features:**
```
04-spec--display-rules-1.md
04-spec--display-rules-2.md
05-design--display-rules-1.md
05-design--display-rules-2.md
```

### Quy tắc ghi file

#### New Feature

**Ghi sau khi HITL xong** — không ghi khi status còn là `Draft`.

**Write to:** `artifacts/[feature-id]--[slug]/04-spec--[name].md`

#### Change Request

Khi CR vào:
1. Tạo `cr/CR-00X--[slug].md` với context: vấn đề, root cause, scope thay đổi (status: In Progress)
2. Update `04-spec`: draft các section bị ảnh hưởng
3. Update `03-planning--prd.md` nếu CR thay đổi scope hoặc feature list ở epic level

Khi CR deployed lên production:
1. Rewrite các section bị ảnh hưởng trong `04-spec`
2. Thêm row vào changelog `04-spec` — ghi rõ Approved date, Deployed date, CR ID, section nào đổi
3. Update `cr/CR-00X.md`: status → Deployed, thêm deployed date
4. Update `03-planning--prd.md` nếu cần
5. Push `04-spec` lên wiki

**Notify BA:** `✓ Saved to artifacts/[feature-id]--[slug]/04-spec--[name].md`

**Sau khi ghi**, thông báo cho BA: `✓ Saved to artifacts/[folder]/[filename].md`

**Overwrite (mặc định):** Tất cả file trừ `02-research.md` — mỗi lần update sau HITL là ghi đè version cũ.

**Append — chỉ áp dụng cho `02-research.md`:**  
File này có thể được ghi bởi nhiều skill liên tiếp (ví dụ: `root-cause-drill` → "Needs more discovery" → `5w2h-interview`).  
Trước khi ghi, skill phải:
1. Đọc nội dung hiện tại của file (nếu đã tồn tại)
2. Append nội dung mới xuống dưới với header phân cách rõ ràng:
```
---
## [Tên artifact] — [date]
[nội dung mới]
```
Không xóa nội dung cũ. Mỗi lần append là thêm một section mới.

---

## Quick Reference — Trigger phrases

| Muốn làm gì | Gọi skill |
|---|---|
| Ticket mới vào, chưa biết là gì | `/intake-qualification` |
| New Feature, cần phỏng vấn stakeholder | `/5w2h-interview` |
| Change Request, cần tìm root cause | `/root-cause-drill` |
| Có Context Brief hoặc Root Cause Brief, cần frame vấn đề | `/problem-statement` |
| Có Problem Statement approved, cần PRD | `/write-prd` |
| Có PRD approved, cần spec từng feature | `/write-spec` |
| Có Spec approved, cần vẽ màn hình lên Figma | `/design-instruction` |
| Có Figma screens, cần viết Jira user stories | `/write-story` |
| Cần brainstorm solution space | `/brainstorm` |
| Cần phân tích đối thủ | `/competitive-brief` |
| Cần lên sprint | `/sprint-planning` |

---

## v2 — Change Management, Guardrail & Registries

> Bổ sung KitA_v2 (audit 2026-07-03). Port từ ba-kit-solo (verbatim). Giữ nguyên kiến trúc multi-agent + autopilot.

### Impact Routing (CR / correction) — cấm sửa mù
Khi có **Change Request** hoặc **câu correction trần** ("bỏ nhóm admin", "đổi default X") trên feature đã có artifact:
1. Chạy **`impact-analysis`** TRƯỚC (analysis-only) → sinh `impacts/CR-00X-impact.md` + cập nhật `_staleness.md`.
2. Chỉ SAU khi rerun được duyệt → agent sở hữu layer mới sửa (theo bảng "Lớp sở hữu" trong impact-analysis).
3. **Cấm** `04-spec` bị ghi đè trực tiếp từ correction trần (pushback `MEM-PBK-04`).

### Overwrite Guard
- **Artifact đã Approved:** trước khi ghi đè → check tồn tại → in path → chỉ ghi đè qua `impact-analysis` hoặc reviewer re-open. (Thay "Overwrite mặc định" cũ cho artifact Approved.)
- **Artifact Draft:** overwrite tự do (giữ tốc độ pipeline).
- `02-research.md`: vẫn append-only (như cũ).

### Cache Freshness — `02-NDS-Knowledge-context.md`
Thêm frontmatter: `stale_status: current|unknown` + `validated_at: YYYY-MM-DD`. spec-agent/reviewer check trước khi dùng; quá 5 ngày hoặc `unknown` → `search_wiki` lại rồi cập nhật.

### RAISE Codes (agent → ba-lead, máy đọc)
| Code | Nghĩa | Recovery |
|---|---|---|
| `SPEC_GATE_FAIL:{lý do}` | Spec thiếu Mermaid/field table | ba-lead trả spec-agent bổ sung |
| `NAV_GAP:section4_missing` | PRD thiếu Section 4 | ba-lead escalate bổ sung PRD |
| `FIGMA_MCP_DOWN` | figma-mcp-go không kết nối | ba-lead dừng Bước 4, báo user setup |
| `KB_OUT_OF_SCOPE:{dept}` | NDS-Knowledge out-of-scope | ba-lead báo BA xin quyền (MEM-PBK-01) |
| `KB_CONFLICT` | wiki vs ticket mâu thuẫn | escalate người phụ trách (MEM-PBK-03) |
| `IMPACT_REQUIRED` | sửa artifact Approved chưa qua impact | route `impact-analysis` |
| `BLOCKER_LOOP` | >3 vòng review | reviewer "Cần người quyết" |

### Shared Registries & Guards (product-level)
- `.claude/shared/common-rules.md` (CR-*) · `.claude/shared/message-list.md` (MSG-*) — spec/design tham chiếu mã, cấm redefine. Runtime seed vào `artifacts/_shared/`.
- `.claude/shared/pushback-triggers.md` (MEM-PBK-*) — agent đọc khi gặp tín hiệu rủi ro.
- `.claude/skills/design-instruction/references/control-type-library.md` — behaviour default UI (deviation model).

### Artifact Profiles (format theo người đọc)
- `user_facing` (prose đầy đủ): 01/02-research/03/04-spec/05-design/06-stories.
- `agent_facing` (bảng compact ID/path/freshness): `02-NDS-Knowledge-context.md`, `05-design-state.md`, `_staleness.md`.
- `machine_facing` (JSON): `_meta.json`.

### Decision Log v2 — Options bắt buộc log
Khi có Options Comparison (`03-brainstorm--notes.md §Options`): ba-lead **bắt buộc** append vào `00-decision-log.md` một entry với:
- **Quyết định:** OPT-x được chọn
- **Bác bỏ:** OPT-y, OPT-z + lý do từng cái
- **Lý do:** căn cứ chọn (impact/effort/risk)
- **Ảnh hưởng:** downstream nào phụ thuộc lựa chọn này
→ Options không chỉ nằm ở brainstorm notes mà phải truy vết được trong decision-log (provenance quyết định).
