# Impact Format — Report + Staleness Registry

> Nguồn: ba-kit-solo `impact-report-template.md` + `shared-staleness-template.md` (verbatim 2026-07-03), map sang artifact flat SSOT của Kit A.

## Impact Report — `artifacts/[folder]/impacts/CR-00X-impact.md`

```
---
artifact: Impact Report
change-id: CR-00X
ticket-id: [Jira ID]
source-skill: impact-analysis
prepared-by: [Tên BA / agent]
date: [YYYY-MM-DD]
status: Draft
---

# Impact Report — CR-00X: [tên thay đổi]

## Change Summary
[1 đoạn: thay đổi là gì + business driver. Nếu là correction trần → ghi nguyên văn câu correction.]

## Affected Artifacts
| Artifact | Path | Impact Type | Confirmed/Possible | Rerun bởi |
|---|---|---|---|---|
| Spec | 04-spec--[feature].md | BR/FR/flow change | confirmed | spec-agent (write-spec) |
| Design | 05-design--[feature].md | screen/state change | possible | design-agent (design-instruction) |
| Story | 06-stories/[feature].md | AC change | possible | delivery-agent (write-story) |
| PRD | 03-planning--prd.md | scope/feature list | confirmed/possible | spec-agent (write-prd) |
| Registry | _shared/common-rules.md | CR-* thay đổi | confirmed | Lead BA |

## Cross-Impact (nếu scope rộng — kế thừa root-cause-drill Step 5)
- Cross-system: [module/feature khác bị chạm]
- Cross-customer: [khách khác gặp vấn đề tương tự?]
- Cross-vertical: [ngành khác?]

## Recommended Rerun Sequence (hẹp nhất trước)
1. [Bước 1 — artifact sở hữu hẹp nhất]
2. [Bước 2]
3. [Bước 3]

## Open Questions
- [ ] OQ-1: [câu hỏi — cần hỏi ai]
```

## Staleness Registry — `artifacts/[folder]/_staleness.md`

```
---
artifact: Staleness Registry
feature: [ID]--[slug]
updated: [YYYY-MM-DD]
---

# Staleness — [ID]--[slug]

> Ghi artifact đang stale + lệnh refresh. Reviewer check trước khi flip Approved.

## Stale Artifacts
| Artifact | Path | Stale Since | Lý do (CR) | Refresh bởi | Status |
|---|---|---|---|---|---|
| 05-design--[feature].md | ... | [date] | CR-00X | design-agent | stale |

## Resolved
| Artifact | Resolved On | Resolved By |
|---|---|---|
| ... | [date] | [agent/command] |
```

## Quy tắc
- Impact Report `status: Draft` cho tới khi rerun được duyệt; không tự Approved.
- `_staleness.md` là "bộ nhớ cái gì đang cũ" — 1 file/feature folder, append + move sang Resolved khi refresh xong.
- Dùng ID (BG/F/SCR/BR/FR/AC) để trace confirmed; module-only match → possible.
