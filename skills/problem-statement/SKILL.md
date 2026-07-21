---
name: problem-statement
description: >
  Generates a standardized Problem Statement including HMW (How Might We) question
  and User Story from a requirements brief or interview notes. Use this skill whenever
  asked to define a problem, write a problem statement, frame a requirement as HMW,
  convert interview notes into a structured problem definition, or when the user says
  "viết problem statement", "định nghĩa vấn đề", "frame vấn đề", or "HMW".
---

# Problem Statement Skill

## Handoff Input
**Receives from:** `5w2h-interview` (Context Brief) HOẶC `root-cause-drill` (Root Cause Brief)
**Expected artifacts:**
- Context Brief: `artifact: Context Brief`, `status: Approved` — có đủ WHO, WHAT, WHY, HOW sections với data thực tế
- Root Cause Brief: `artifact: Root Cause Brief`, `status: Approved` — Recommendation ≠ "Needs more discovery", có Root cause + JTBD Statement
**Routing:** Nhận diện loại artifact qua field `artifact:` trong metadata header để điều chỉnh cách frame vấn đề
**Hard gate:** Nếu Root Cause Brief có Recommendation = "Needs more discovery" → từ chối, chuyển về `5w2h-interview` trước

## Mục tiêu

Biến raw input (interview notes, ticket mô tả, root cause brief) thành một Problem Statement
chuẩn hóa gồm 3 phần: Context, HMW question, và User Story. Output phải tập trung vào
**vấn đề**, không phải solution.

## Khi nào dùng skill này

- Sau bước 5W2H Interview → cần chuẩn hóa Context Brief thành Problem Statement
- Sau bước Root Cause Drill → cần chuyển root cause thành HMW actionable
- Khi ticket mô tả solution thay vì vấn đề → cần reframe
- Khi cần align stakeholder về đúng vấn đề trước khi phân tích Impact/Effort

## Input cần có

Skill hoạt động tốt nhất khi có ít nhất một trong các input sau:

1. **Context Brief** từ 5W2H Interview (Who, What, Where, When, Why, How, How much)
2. **Root Cause Brief** từ 5 Whys / JTBD drill
3. **Ticket mô tả** từ Sales/CS (dù còn thô)
4. **User persona** biết trước (nếu có)

Nếu thiếu thông tin quan trọng, hỏi tối đa 3 câu làm rõ trước khi generate.

## Quy trình thực hiện

### Bước 1 — Đọc và phân tích input

Xác định:
- **Ai** đang gặp vấn đề? (user segment, role, context)
- **Vấn đề thực sự là gì?** (tách khỏi solution nếu input đang mô tả solution)
- **Impact** nếu vấn đề không được giải quyết là gì?
- **Trigger** — khi nào vấn đề xảy ra?

Nếu input đang mô tả solution (ví dụ: "cần thêm nút X", "cần tích hợp Y"):
→ Ghi chú "[Detected: solution-first request]" và reframe về vấn đề gốc.

### Bước 2 — Viết Context (2-3 câu)

Format:
> [User segment] đang gặp khó khăn khi [hành động cụ thể] vì [nguyên nhân/rào cản].
> Điều này dẫn đến [hệ quả tiêu cực] và ảnh hưởng đến [business/user outcome].

Quy tắc:
- Không được nhắc đến solution trong phần này
- Phải có subject rõ ràng (ai), verb (đang làm gì), barrier (vì sao không được)
- Không dùng từ mơ hồ: "nhanh hơn", "tốt hơn", "dễ dùng hơn" — phải cụ thể

### Bước 3 — Viết HMW Question

Format:
> **How Might We** [động từ hành động] [đối tượng] [mà không/trong khi] [constraint hoặc trade-off]?

Quy tắc:
- "How" → đủ rộng để có nhiều solution
- "Might" → không cam kết solution cụ thể, khuyến khích ideation
- "We" → vấn đề của cả team, không đổ lỗi
- Phải có **1 constraint** hoặc trade-off để tránh HMW quá chung chung
- Không viết HMW dạng: "How Might We build [feature X]?" — đây là solution, không phải problem

Ví dụ tốt:
> HMW giúp CS lead theo dõi trạng thái xử lý yêu cầu khách hàng mà không cần hỏi thủ công qua chat mỗi ngày?

Ví dụ cần tránh:
> HMW xây dựng dashboard realtime? ← đây là solution
> HMW cải thiện trải nghiệm người dùng? ← quá chung chung, không có constraint

### Bước 4 — Viết User Story

Format chuẩn (3 phần bắt buộc):
> **As a** [persona cụ thể],
> **I want to** [hành động/outcome cụ thể],
> **So that** [business value hoặc user value đo được].

Quy tắc:
- "As a" → persona đủ cụ thể (không dùng "user" chung chung)
- "I want to" → mô tả outcome, không phải feature ("xem được trạng thái" thay vì "có dashboard")
- "So that" → phải có value rõ ràng, tránh circular ("so that I can do X" khi X = "I want to" đã viết)
- Nếu có nhiều persona → viết thành nhiều User Story riêng

### Bước 5 — Self-check trước khi output

Trước khi trả kết quả, kiểm tra:

| Tiêu chí | Check |
|---|---|
| Context không đề cập solution | ✓/✗ |
| HMW có constraint hoặc trade-off | ✓/✗ |
| HMW không phải dạng "HMW build X" | ✓/✗ |
| User Story có đủ 3 phần | ✓/✗ |
| "So that" có value đo được | ✓/✗ |
| Không có từ mơ hồ (tốt hơn, nhanh hơn) | ✓/✗ |

Nếu bất kỳ check nào là ✗ → tự sửa trước khi output.

## Output File

After BA validates and approves:

1. Set `status: Approved` in the metadata header
2. Save to: `artifacts/[folder]/03-problem-statement.md`
3. Notify BA: `✓ Saved to artifacts/[folder]/03-problem-statement.md`
4. Suggest next step:
   > **Bước tiếp theo:** HMW đã được frame — đây là lúc lý tưởng để explore giải pháp trước khi commit vào PRD.
   > - **`/brainstorm`** — nếu muốn explore nhiều hướng giải quyết, stress-test assumptions, hoặc chưa chắc về approach
   > - **`write-prd`** — nếu hướng giải quyết đã đủ rõ và sẵn sàng define scope

## Format output

Trả kết quả theo cấu trúc sau, không thêm bớt section:

---

### Problem Statement

**Context**
[2-3 câu mô tả vấn đề, không có solution]

**HMW Question**
> How Might We [...]?

**User Story**
> As a [persona],
> I want to [outcome],
> So that [value].

---

**Confidence level:** [Cao / Trung bình / Thấp]
**Lý do nếu Trung bình hoặc Thấp:** [Thiếu thông tin gì, assumption nào đang được dùng]

**HITL — Bạn cần validate:**
- [ ] Context có đúng với vấn đề thực tế không?
- [ ] HMW có đủ rộng để ideation không? Có quá rộng không?
- [ ] Persona trong User Story có đúng không?
- [ ] "So that" có đúng với business goal hiện tại không?

---

## Ví dụ minh họa

### Input
> CS team phản ánh: khách hàng hay hỏi "yêu cầu của tôi đang ở đâu rồi?" nhưng CS không có cách nào
> tra cứu nhanh ngoài việc ping sang team product qua Slack. Mất khoảng 15-30 phút mỗi lần.
> Xảy ra khoảng 20-30 lần/tuần.

### Output

---

**Context**
CS agent đang tốn 15-30 phút mỗi lần để tra cứu trạng thái xử lý yêu cầu khách hàng vì
không có công cụ nào cho phép họ tự tra cứu mà không cần hỏi team Product qua Slack.
Điều này dẫn đến phản hồi chậm cho khách hàng (~20-30 lần/tuần) và tạo interrupt cost
cho Product team.

**HMW Question**
> How Might We giúp CS agent tự tra cứu trạng thái yêu cầu khách hàng mà không cần
> tạo interrupt cho Product team?

**User Story**
> As a CS agent,
> I want to see the current status and owner of any customer request in real time,
> So that I can respond to customers within 5 minutes without waiting for internal pings.

---

**Confidence level:** Cao
**HITL — Bạn cần validate:**
- [ ] Context có đúng với vấn đề thực tế không?
- [ ] HMW có đủ rộng để ideation không? Có quá rộng không?
- [ ] Persona trong User Story có đúng không?
- [ ] "So that" có đúng với business goal hiện tại không?

---

## Lưu ý quan trọng

- **Không generate solution** trong bất kỳ phần nào của output
- **Không assume** thêm thông tin không có trong input — nếu thiếu, ghi rõ trong Confidence level
- **Không bỏ qua HITL checklist** — đây là điểm bắt buộc để BA review trước khi dùng output
- Nếu input là tiếng Việt → output Context và HITL bằng tiếng Việt, HMW và User Story có thể giữ tiếng Anh hoặc tiếng Việt tùy user chỉ định
