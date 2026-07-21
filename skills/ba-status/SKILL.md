---
name: ba-status
description: >
  Xem nhanh trạng thái pipeline BA của 1 hoặc tất cả feature/ticket — stat-only,
  KHÔNG mutate. Đọc _meta.json + _staleness.md + sự TỒN TẠI của các file prefix
  (00→06) → in bảng: ticket đang ở bước nào, artifact nào stale, bước tiếp theo.
  Trigger: "ba-status", "pipeline đang ở đâu", "ticket này tới đâu rồi",
  "cái gì đang stale", "status các ticket".
  Output tiếng Việt.
version: 1.0.0
---

# BA Status — stat-only pipeline view

> Nguồn cơ chế: ba-kit-solo `core/workflows/next.md` + `status` + `package-status-next` (stat-only state machine). Port sang artifact **flat SSOT** của Kit A. **KHÔNG** port memory tiering hot/warm/cold (giữ NDS-Knowledge làm chân lý ngoài).

## Nguyên tắc
- **Stat-only:** chỉ check **file tồn tại** + đọc 2 manifest (`_meta.json`, `_staleness.md`). **KHÔNG mở nội dung** 04-spec/PRD/design → nhanh, rẻ token, deterministic.
- **Không mutate.** Chỉ đọc và in.
- **Deterministic:** cùng trạng thái đĩa → cùng output block.

## Input
- 1 feature: `artifacts/[ID]--[slug]/`
- Hoặc không tham số → quét **mọi** folder trong `artifacts/` → in bảng tổng.

## Workflow
1. Resolve folder(s) (nhiều folder mơ hồ → liệt kê hết, không đoán mtime).
2. Đọc `_meta.json` (current_version, current_spec, last_deployed, open_cr) + `_staleness.md` (stale list) nếu có.
3. Check tồn tại prefix artifact → map Stage.
4. Suy **Next step** bằng forward rules (chỉ dựa file tồn tại + open_cr + stale, không đọc nội dung).
5. In status block cố định.

## Stage mapping (theo file tồn tại)
| File tồn tại | Stage đạt |
|---|---|
| 01-qualified-ticket.md | ① Intake |
| 02-research.md + 03-problem-statement.md | ② Discovery |
| 03-planning--prd.md | ③ PRD |
| 04-spec--*.md | ④ Spec |
| 05-design--*.md | ⑤ Design |
| 06-stories/*.md | ⑥ Delivery |

## Next-step rules (forward, stat-only — dừng ở rule đầu khớp)
1. `_staleness.md` có dòng `stale` → **refresh trước** (in artifact + lệnh/agent refresh).
2. `_meta.json.open_cr` ≠ null → **impact-analysis** cho CR đó (nếu chưa có `impacts/CR-xxx-impact.md`).
3. thiếu 01 → **intake-qualification**
4. có 01, thiếu 02/03 → **discovery-agent**
5. có 03-problem, thiếu 03-planning--prd → **write-prd** (full) · nếu là CR → **impact-analysis**
6. có prd, thiếu 04-spec → **write-spec**
7. có 04-spec, thiếu 05-design (feature có UI) → **design-instruction**
8. có 05-design, thiếu 06-stories → **write-story**
9. đủ 01→06 + không stale → **Hoàn tất** (chờ đẩy Jira)

## Output block (deterministic)
```
📊 BA STATUS — [ID]--[slug]   ([YYYY-MM-DD])
Stage:     ④ Spec  (▓▓▓▓░░ 4/6)
Version:   v1.1 · current_spec: 04-spec--display-rules.md · last_deployed: 2026-06-20
Artifacts: 00✓ 01✓ 02✓ 03-ps✓ 03-prd✓ 04-spec✓ 05-design— 06-stories—
Stale:     05-design--display-rules.md  (CR-002, since 2026-07-01)
Open CR:   CR-002 (chưa có impact report)
▶ Next:    impact-analysis CR-002  →  design refresh (design-agent)
```
Quét toàn bộ → in bảng: | Ticket | Stage | Stale | Open CR | Next |.

## Quality Control
| Check | Criteria |
|---|---|
| Stat-only | Không mở nội dung file (chỉ existence + 2 manifest) |
| Deterministic | Output là block cố định |
| Next đúng rule | Dừng ở forward rule đầu tiên khớp |
| Stale ưu tiên | Có stale → next = refresh trước mọi thứ |

## Raise/Note
- Thiếu `_meta.json` (feature cũ) → degrade: chỉ suy Stage từ file tồn tại, ghi "meta absent".
- Không tự sửa gì; đề xuất lệnh để BA/ba-lead chạy.
