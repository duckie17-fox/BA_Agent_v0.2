# Format Guide — write-spec Section Detail

## Artifact File Naming Convention

### New Feature (v1.0)

Descriptive naming pattern:

```
01-intake--qualified.md          # Qualified ticket assessment
02-discovery--problem-statement.md
03-planning--prd.md
04-spec--feature-name.md         # Feature spec (this skill output)
05-design--instruction.md
```

Each number corresponds to the artifact pipeline stage:
- **01** = Intake & Qualification
- **02** = Discovery (Problem Statement)
- **03** = Planning (PRD)
- **04** = Specification (Feature Detail) ← write-spec outputs this
- **05** = Design (Detailed instructions)

### For Multiple Sub-Features

When a feature has multiple independent specs:

```
04-spec--feature-1.md
04-spec--feature-2.md
04-spec--feature-3.md
```

### Folder Structure

Flat structure — không có version subfolder:

```
artifacts/[ID]--[slug]/
├── 01-intake--qualified.md
├── 02-discovery--problem-statement.md
├── 03-planning--prd.md
├── 04-spec--[feature].md          ← single source of truth, luôn là bản production mới nhất
└── cr/
    ├── CR-001--[slug].md          ← context doc, tạo khi CR vào, không đổi sau deployed
    └── CR-002--[slug].md
```

**Quy tắc:**
- `04-spec` và `03-planning--prd` là file duy nhất — rewrite trực tiếp khi CR deployed
- Changelog nằm trong `04-spec` (bảng Lịch sử thay đổi)
- `cr/` là context docs — lưu lý do CR, root cause, ai approve
- Push `04-spec` lên wiki **sau khi deployed production**, không phải sau approved

---



## Section C — Workflow (Mermaid)

Gồm 2 mục chính:

- Mô tả luồng hoạt động bằng text:   
  1. Luồng hoạt động chính (Normal Course) - Các bước từ khi kích hoạt đến khi đạt được mục tiêu
      - **Format:** 
         - Numbered list (1, 2, 3…)
         - Each step: one single action
         - Start with a clear subject (Actor / System)
         - Active voice + present tense
         - Short steps, 1-2 sentences each
      - **DO NOT embed:**
         - If/else → move to Alternative Course
         - Loops → use "Steps X-Y repeat until Z"
         - Exceptions → move to Exceptions
         - Internal system logic → that's design, not a UC
  2. Luồng xử lý ngoại lệ (alternative course) - Một luồng quy trình thay thế vẫn dẫn đến mục tiêu (vẫn thành công), chỉ là đi theo một lộ trình khác
      - **Rules:**
        - ID format: UC-XX.AC.N (AC = Alternative Course)
        - Each AC starts with: "At step Y of the Normal Course, if [condition], execute the alternative: …"
        - After the AC, state explicitly which step of the Normal Course to continue from
  3. Luồng xử lý thất bại (failure course) - Một luồng quy trình không đạt được mục tiêu, dẫn đến kết quả không mong muốn hoặc dừng hoạt động.
    - **Rules:**
        - ID format: UC-XX.EX.N (EX = Exception)
        - Each exception needs 3 parts:
            - Trigger condition: When the exception occurs
            - System response: What the system does
            - Final state: The end state (rollback? partial? log?)

- Vẽ Mermaid diagram dựa trên mô tả luồng hoạt động ở trên:
   1. **Mermaid diagram** — xem `mermaid-guide.md` để chọn loại diagram phù hợp
   2. **Mô tả text** — giải thích ngắn các nhánh quan trọng (optional nếu diagram đã rõ)

### Cấu trúc đầy đủ:

```
**C. Workflow:**

### Normal Course (luồng hoạt động chính)
**Ví dụ:**
1. Learner navigates to the "My Mentors" section and selects a mentor profile.
2. System displays the mentor's profile: bio, expertise, average rating, and available time slots for the next 14 days.
3. Learner selects a preferred date and time slot.
4. Learner enters a topic or question for the session (max 500 characters) and clicks "Send Request".
5. System validates that the learner has at least 1 unused session quota in their current subscription.
6. System creates a session request with status='Pending_Mentor_Review' and sends a notification to the mentor.
7. System displays a confirmation screen: "Request sent! Your mentor will respond within 24 hours."
8. System invokes UC-NOTI-03 to send a confirmation email to the learner.

### Alternative Course (luồng xử lý ngoại lệ)
Ví dụ:
UC-LEARN-01.AC.1: Enroll using an enterprise voucher
At step 5 of the Normal Course, if the learner selects "Enterprise Voucher" as the payment method:
5a. System displays a voucher code input field.
5b. Learner enters the code and clicks "Apply".
5c. System validates the voucher (expiry, applicability, remaining uses).
5d. System updates the total amount to 0 VND → continue from step 7 of the Normal Course (no payment gateway call).

### Failure Course (luồng xử lý thất bại)
Ví dụ:
UC-LEARN-01.EX.2: Course reaches full capacity between page load and enrollment
Trigger: At step 7, LMS returns CAPACITY_EXCEEDED because another learner filled the last slot milliseconds earlier.
Response: System displays "Sorry, this course just reached full capacity. Join the waitlist to be notified when a slot opens."
Final state: Payment is refunded automatically within 1 business day. No enrollment record is created. Learner is offered the waitlist option.


### Diagram
[Mermaid diagram]

**Ghi chú:**
- [Nhánh X xảy ra khi...]
- [Nhánh Y xảy ra khi...]
```



## Section E — Business Rules

- Rules:
   - Viết ngắn gọn, rõ ràng - một rule chỉ mô tả một ràng buộc
   - Dùng từ ngữ đơn giản
   - Sắp xếp rules theo nhóm tính năng
   - Kiểm tra kĩ tránh conflics giữa các rules và edge cases
   - Tách biệt khỏi yêu cầu cầu chức năng (Functional Requirements) để tránh trùng lặp thông tin

- Ví dụ cách viết đúng:
   - BAD: Khách hàng đăng ký thẻ phải đủ 18 tuổi và có thu nhập ổn định từ 10 triệu trở lên mỗi tháng
   - GOOD:
      - BR1: Khách hàng phải từ 18 tuổi trở lên
      - BR2: Thu nhập tối thiểu 10tr/tháng 

- Ví dụ conflicts: Rule “Khách VIP được ưu tiên xử lý trong 2h” có conflict với rule “Đơn khiếu nại được xử lý theo thứ tự”.

## Section F - Functional Requirements
 
### Định nghĩa
FR mô tả **hệ thống làm được gì** ở cấp độ feature/capability — tức là hành vi và chức năng mà hệ thống phải cung cấp cho người dùng.
 
### Ranh giới — KHÔNG đưa vào FR
| Loại thông tin | Đặt ở đâu |
|---|---|
| Ràng buộc nghiệp vụ (tuổi tối thiểu, hạn mức, điều kiện tính điểm...) | Section E — Business Rules |
| Validation từng trường (required, max length, format...) | Section I — Bảng mô tả trường |
| Luồng thao tác từng bước | Section C — Workflow |
| Trường hợp lỗi / ngoại lệ | Section H — Edge Cases |
 
### Những gì nên đưa vào FR
FR phù hợp khi mô tả:
- **Capability** — "Hệ thống cho phép [actor] thực hiện [hành động]"
- **Screen/Module behavior** — "Màn hình X hiển thị / tính toán / xuất..."
- **System action** — "Hệ thống tự động [gửi notification / tạo bản ghi / cập nhật trạng thái] khi..."
- **Integration behavior** — "Hệ thống đồng bộ dữ liệu với [service Y] khi..."
- **Permission/role behavior** — "Chỉ [role] mới có thể thực hiện [hành động]"
> **Rule of thumb:** Nếu câu bắt đầu bằng *"Trường [X] phải..."* → đưa vào Section I.
> Nếu bắt đầu bằng *"Hệ thống phải..."* hoặc *"Người dùng có thể..."* → đây là FR.
 
### Format
ID theo pattern: `FR-[NN]` (VD: FR-01, FR-02)
 
```markdown
| # | ID | Description |
|---|----------|-----------------|
| 1 | FR-01 | Hệ thống cho phép người dùng tạo mới chương trình Off-invoice với các loại: Chiết khấu theo sản lượng, Tặng hàng, Hỗ trợ chi phí. |
| 2 | FR-02 | Hệ thống tự động gửi notification cho Manager khi chương trình được submit duyệt. |
| 3 | FR-03 | Chỉ người dùng có role Trade Marketer mới có thể tạo và chỉnh sửa chương trình. |
| 4 | FR-04 | Hệ thống cho phép export danh sách chương trình ra file Excel với toàn bộ dữ liệu hiển thị trên màn hình. |
| 5 | FR-05 | Hệ thống tự động cập nhật trạng thái chương trình sang "Đang chạy" khi đến ngày bắt đầu. |
```
 
### Gợi ý nhóm FR thường gặp
- **CRUD:** Tạo mới, xem chi tiết, chỉnh sửa, xóa/hủy
- **Search & Filter:** Tìm kiếm, lọc, sắp xếp danh sách
- **State transition:** Duyệt, từ chối, kích hoạt, kết thúc
- **Notification & Integration:** Gửi thông báo, đồng bộ với hệ thống khác
- **Export / Import:** Xuất file, tải lên dữ liệu
- **Permission:** Phân quyền theo role
---

## Section G - Non-Functional Requirements

### Định nghĩa
NFR mô tả **hệ thống hoạt động tốt thế nào** — các thuộc tính chất lượng không liên quan đến *chức năng cụ thể* mà liên quan đến *cách hệ thống vận hành*.
 
### Các nhóm NFR cần xem xét
 
| Nhóm | Mô tả | Ví dụ |
|---|---|---|
| **Performance** | Thời gian phản hồi, throughput | Tải trang < 2s với 1000 records |
| **Security** | Xác thực, phân quyền, mã hóa | Chỉ user có session hợp lệ mới truy cập được |
| **Availability** | Uptime, recovery time | Tính năng hoạt động 99.5% uptime |
| **Scalability** | Khả năng chịu tải khi scale | Hỗ trợ đồng thời 500 user |
| **Usability** | Trải nghiệm người dùng | Thao tác tạo mới hoàn thành trong < 5 phút |
| **Auditability** | Log, tracking thay đổi | Mọi thay đổi trạng thái đều được ghi log |
| **Data retention** | Lưu trữ, sao lưu dữ liệu | Dữ liệu lịch sử giữ tối thiểu 2 năm |
| **Compatibility** | Thiết bị, browser, OS | Hỗ trợ Chrome, Edge phiên bản 2 năm gần nhất |
 
### Hướng dẫn viết NFR
- **Phải đo được** — tránh viết chung chung như "hệ thống phải nhanh"; phải có con số cụ thể hoặc điều kiện rõ ràng.
- **Gắn với feature này** — không copy NFR generic của toàn hệ thống nếu không có yêu cầu đặc thù.
- **Flag `[giả định]`** nếu con số chưa được xác nhận với Engineering/Product.
> **Khi nào bỏ qua NFR:** Nếu feature không có yêu cầu đặc thù về performance, security hay khả năng mở rộng — có thể ghi `Áp dụng NFR chung của hệ thống, không có yêu cầu đặc thù.` thay vì để trống.
 
### Format
ID theo pattern: `NFR-[NN]` (VD: NFR-01, NFR-02)
 
```markdown
| # | ID | Description |
|---|----------|-----------------|
| 1 | NFR-01 | Màn hình danh sách chương trình phải tải xong trong vòng 2 giây với tối đa 500 records [giả định]. |
| 2 | NFR-02 | Chỉ user đã đăng nhập và có quyền "Trade Marketer" mới truy cập được tính năng tạo chương trình. |
| 3 | NFR-03 | Mọi thao tác tạo mới, chỉnh sửa, duyệt, từ chối phải được ghi audit log: user, timestamp, giá trị trước/sau. |
| 4 | NFR-04 | Tính năng phải hoạt động đúng trên Chrome và Edge (2 phiên bản mới nhất), màn hình tối thiểu 1280×720px. |
```

## Section H - Edge Cases:

**Gợi ý các loại edge cases cần cover:**
- Empty/null input cho trường bắt buộc
- Duplicate data (unique constraint violations)
- Boundary dates (start = end, dates in the past)
- Permission edge cases (user không có quyền)
- Concurrent access (2 users cùng thao tác)
- Network/timeout scenarios
- State transition violations (duyệt khi chưa đủ điều kiện)
- Bulk operation limits nếu có

**Format**

```markdown
| # | Scenario | Input/Điều kiện | Expected behavior |
|---|----------|-----------------|-------------------|
| EC-01 | Mã chương trình trùng | program_code đã tồn tại trong DB | Hiển thị lỗi inline "Mã chương trình đã tồn tại", không tạo mới |
| EC-02 | Lưu khi chưa điền trường bắt buộc | Bất kỳ trường P0 để trống | Highlight tất cả trường lỗi cùng lúc, không submit |
| EC-03 | End date < Start date | end_date < start_date | Disable ngày không hợp lệ trong date picker |
| EC-04 | Concurrent edit | 2 users cùng chỉnh sửa 1 chương trình | [Xác nhận behavior với Engineering] |
| EC-05 | Session timeout khi đang điền form | User idle > [N] phút | [Xác nhận behavior] |
| EC-06 | Duyệt khi chưa cài đặt đủ tab | Tab Điều kiện trả thưởng trống | Hiển thị thông báo lỗi, không chuyển trạng thái |
```

## Section I — Bảng mô tả trường thông tin

Đặt theo format: `**I. Bảng mô tả trường — [Tên section]**`

Nếu màn hình có nhiều nhóm (VD: KPI Cards, Filters, Charts), tạo bảng riêng cho mỗi nhóm, tiếp tục dùng cùng ký tự I với ghi chú tên section:
`**I. Bảng mô tả trường — [Tên section 2]**`

Với các trường có logic phức tạp, thêm Given/When/Then:

```
• **Trường bắt buộc nhập**
• [Validation rules]

**Acceptance criteria:**
- Given: [Điều kiện]
- When: [Hành động]
- Then: [Kết quả mong đợi]
```

---


## Section J — Open Questions (optional)

Chỉ thêm khi có câu hỏi thực sự chưa có đáp án.

```markdown
**J. Open Questions**

| # | Câu hỏi | Owner | Blocking? | Deadline |
|---|---------|-------|-----------|---------|
| 1 | [Câu hỏi cụ thể] | Engineering/BA/Product | Có/Không | [Ngày] |
```

---

## HITL Checklist (luôn output)

Sau khi BA nhận draft, dùng checklist này để validate spec trước khi approve.

```markdown
## HITL Checklist

- [ ] User Story đúng intent không?
- [ ] Workflow (C) đúng flow không? Có error path covering không?
- [ ] Mermaid diagram chính xác không?
- [ ] Business Rules (E) đủ không? Có rule nào bị thiếu?
- [ ] Functional Requirements (F) đã cover đủ capability?
- [ ] Non-Functional Requirements (G) có con số phù hợp không?
- [ ] Edge Cases (H) đã cover đủ?
- [ ] Field table (I) đủ tất cả trường + validation rules?
- [ ] Use cases / User stories liên quan (J) có đúng + đủ không?
- [ ] Figma code (section L) có capture đủ components không?
```

---

## Changelog Format (trong 04-spec)

Bảng Lịch sử thay đổi trong `04-spec` là nguồn trace duy nhất — thêm row mỗi lần CR deployed:

```markdown
## Lịch sử thay đổi

| Version | Approved | Deployed | Author | CR ID | Nội dung thay đổi |
|---|---|---|---|---|---|
| v1.2 | 2026-07-15 | 2026-07-20 | An Phạm | CR-002 | Fix store scope — BR-03 rewrite, Section I trường Phạm vi áp dụng |
| v1.1 | 2026-06-10 | 2026-06-15 | An Phạm | CR-001 | Thêm planogram mode — Section C workflow, BR-01 rewrite, Section I thêm 2 trường |
| v1.0 | 2026-05-01 | 2026-05-05 | An Phạm | — | Khởi tạo tài liệu |
```

**Quy tắc:**
- Deployed = ngày lên production (không phải ngày approve)
- Cột "Nội dung thay đổi" ghi rõ section nào đổi — đủ để BA biết cần review chỗ nào
- Mới nhất ở trên cùng

## CR Context Doc Format (cr/CR-00X--slug.md)

```markdown
---
cr-id: CR-001
title: Thêm chế độ Planogram vào Display Rules
status: Deployed        ← In Progress / Approved / Deployed
approved: 2026-06-10
deployed: 2026-06-15
approved-by: An Phạm
---

## Vấn đề
[Mô tả vấn đề dẫn đến CR này]

## Root cause
[Nguyên nhân gốc rễ]

## Scope thay đổi
[Section nào trong spec bị ảnh hưởng, thay đổi gì]

## Ghi chú
[Breaking changes, rollback plan, dependencies nếu có]
```

### _cr-info.json Template — CŨ, không còn dùng

Metadata for each CR (one file per versioned folder):

```json
{
  "cr-id": "CR-VPR-001-001",
  "version": "v1.1",
  "base-version": "v1.0",
  "type": "patch",
  "status": "Approved",
  "created-date": "2026-06-01",
  "approved-date": "2026-06-15",
  "title": "Add planogram mode support",
  "problem-statement": "Display Rules need to support planogram-based configuration in addition to shelf-level rules.",
  "affected-modules": [
    "Display Rule",
    "AI Evaluation Plan",
    "Post-Audit Plan"
  ],
  "breaking-changes": false,
  "changes-summary": [
    "Added new config mode: Planogram (alongside existing Shelf-Level)",
    "Added planogram image upload and reference capability",
    "Extended Display Rule validation to handle planogram constraints"
  ],
  "related-crs": ["CR-VPR-001-002"],
  "approved-by": "Product Manager Name",
  "notes": "Deployed 2026-06-20"
}
```

---

## Section L — Figma Design Code (optional)

**Khi nào thêm:** Khi bạn đã lấy code từ Figma MCP (`get_node`, `get_selection`...)

**Nội dung:** JSON code từ Figma, chứa:
- Node ID
- Component names
- Styles (colors, fonts, sizes, padding, borders...)
- Children hierarchy
- Text content (placeholder, button text, labels...)

**Format:**

```markdown
## L. Figma Design Code

```json
{
  "id": "[Node ID từ Figma]",
  "name": "[Component Name]",
  "type": "[FRAME/COMPONENT/TEXT]",
  "bounds": {
    "height": [px],
    "width": [px],
    "x": [px],
    "y": [px]
  },
  "styles": {
    "fills": ["[color hex]"],
    "fontFamily": "[font name]",
    "fontSize": [size],
    "fontWeight": [weight],
    "padding": { "top": [px], "right": [px], "bottom": [px], "left": [px] }
  },
  "children": [
    {
      "id": "[Child Node ID]",
      "name": "[Child Name]",
      "type": "[TEXT/FRAME]",
      "characters": "[content nếu là TEXT]"
    }
  ]
}
```

**Ghi chú:**
- Xóa section này nếu không có Figma code
- Figma code được đẩy xuống cuối để dễ đọc spec mà không bị visual overload
- Dùng cho reference khi engineer/designer cần implement hoặc verify design specs

---

## Output Structure — Tóm tắt thứ tự

```
# Spec N. [Feature Name]

## Lịch sử thay đổi
## Mô tả tài liệu
  A. Link Jira
  B. User Story (mô tả + user story format)
  C. Workflow (Normal + Alternative + Failure + Diagram)
  D. Design (TBD / link / placeholder)
  E. Business Rules
  F. Functional Requirements
  G. Non-Functional Requirements
  H. Edge Cases
  I. Bảng mô tả trường (có thể nhiều bảng con)
  J. Use cases / User stories liên quan [optional]
  K. Open Questions [optional]

## HITL Checklist

## L. Figma Design Code [optional]
```
---

## v2 — Version/Meta Index Format (flat SSOT) — gộp từ versioning-guide (đã xóa)

> Mô hình **flat**, không version subfolder. `04-spec` là SSOT luôn phản ánh production. `_cr-info.json` đã **bỏ** (deprecated). Dưới đây là format 2 file index mà ba-lead tạo ở Intake.

### `_versions.md` — version index
```
# Version Index — [Feature Name]
| Version | Date (Approved) | Type | CR ID | Sections changed | Deployed |
|---|---|---|---|---|---|
| v1.0 | YYYY-MM-DD | New Feature | — | initial | YYYY-MM-DD |
| v1.1 | YYYY-MM-DD | CR | CR-001 | E, I | — |
```
### `_meta.json` — pointer (machine_facing)
```json
{ "current_version": "v1.1", "current_spec": "04-spec--[feature].md",
  "last_deployed": "YYYY-MM-DD", "open_cr": ["CR-002"] }
```
### CR workflow (flat) — nhắc lại
1. CR vào → `impact-analysis` → `impacts/CR-00X-impact.md` + `_staleness.md`.
2. Sửa `04-spec` in-place (section bị ảnh hưởng) + thêm row changelog nội bộ + row `_versions.md`.
3. `cr/CR-00X--[slug].md` giữ context (không đổi sau deploy).
> Ví dụ folder flat đầy đủ: xem `write-spec/README.md` §Flat Example (v2).

## v2 — Section J (Phụ thuộc chéo) + Trace frontmatter
- **Frontmatter trace/freshness** bắt buộc: `stale_status` (current|stale|unknown) · `linked_ids` · `depends_on` · `produces_for` · `changelog: date | command | note`. Traceability matrix sinh bằng grep, không viết tay.
- **Section J** nâng từ "Use cases liên quan" (list) → bảng **Depends on / Produces for** có ID + Data/State + Ticket. impact-analysis đọc bảng này để reverse inbound scan (ai consume) + outbound tracing (produce cho ai).
