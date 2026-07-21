---
name: write-story
description: Viết Jira user story theo format chuẩn của team NDS. Dùng khi user cung cấp mô tả tính năng hoặc task cần tạo ticket, hoặc khi nói "viết story", "tạo task", "tạo Jira ticket", "viết user story cho", "tạo story cho". Output là story hoàn chỉnh sẵn sàng paste lên Jira.
---

Skill này tạo Jira user story theo đúng format chuẩn của team NDS Vietnam. Output bằng tiếng Việt, sẵn sàng paste lên Jira.

## Format chuẩn

```
## [PROJECT-ID] Tiêu đề ngắn gọn, mô tả đúng phạm vi

**Là** [persona], **tôi muốn** [mục tiêu],
**để** [lý do / giá trị mang lại].

**Mô tả:**
[1–3 câu. Nêu bối cảnh và vấn đề cần giải quyết — KHÔNG mô tả cách làm.]

**Luồng xử lý:** (nếu có)
[Dạng danh sách đánh số thứ tự. Mô tả WHAT xảy ra, không mô tả HOW implement.]

**Tiêu chí nghiệm thu:**
[Dùng format: "Nếu [điều kiện], khi [trigger], thì [kết quả mong đợi]"]

**Checklist:**
- [ ] [Việc cần làm — actionable, kiểm tra được]

**Phụ thuộc:** [Story/task liên quan, nếu có]

**Nhãn:** [Label1] [Label2]
```

---

## Nguyên tắc viết

### User Story (dòng Là/Tôi muốn/Để)
- **Là**: persona cụ thể — không dùng "user" chung chung. Nêu rõ role và context (ví dụ: "developer trong team Sabeco SOT", "người dùng đang thực địa tại quán")
- **Tôi muốn**: mục tiêu cụ thể, đo lường được
- **Để**: lý do kinh doanh hoặc giá trị mang lại — không phải lý do kỹ thuật, không lặp lại "tôi muốn"

### Mô tả
- Tập trung vào **What** (vấn đề cần giải quyết) và **Why** (tại sao cần)
- **Không** đề cập implementation, thư viện, pattern kỹ thuật, hay cách tổ chức code
- **Không** dùng từ kỹ thuật như: implement, API, database, schema, queue, worker, retry, cache...
- Nếu có context thực tế (lỗi đã gặp khi test, số liệu cụ thể), đưa vào để làm rõ bối cảnh

### Luồng xử lý
- Dùng dạng danh sách đánh số thứ tự (1. 2. 3...) — không dùng sơ đồ mũi tên `→`. Số thứ tự giúp đối chiếu trực tiếp với step trong tool QC.
- Mỗi bước là một **hành động hoặc quyết định** — không phải một dòng code
- Nếu 1 bước rẽ nhánh theo điều kiện, giữ chung 1 số thứ tự và liệt kê từng nhánh bằng gạch đầu dòng con, bắt đầu bằng "Nếu..." (xem ví dụ)
- Không dùng tên hàm, class, biến, hay thuật ngữ implementation
- Người đọc hiểu được flow mà không cần biết tech stack

**Ví dụ đúng:**
```
1. Ảnh đầu vào
2. Phân loại khu vực trong ảnh
3. Nhận diện sản phẩm theo khu vực vừa xác định
4. Kiểm tra kết quả
   - Nếu phát hiện sai → điều chỉnh lại kết quả
   - Nếu không sai → giữ nguyên
5. Lưu kết quả
```

**Ví dụ sai:**
```
1. Excel input
2. Build queue (skip nếu đã có .json)
3. Khởi N workers với round-robin API key
4. Gọi LLM API, parse JSON response, lưu file
```

### Tiêu chí nghiệm thu
- Format: **"Nếu [điều kiện], khi [trigger], thì [kết quả đo được]"**
- Mỗi tiêu chí chỉ test **1 scenario duy nhất**
- Phải cover đủ **3 loại**: luồng chính (happy path) + điều kiện biên/validation + lỗi/negative path
- Kết quả phải **đo lường được** — tester có thể confirm pass/fail
- Không dùng từ mơ hồ: "nhanh", "phù hợp", "rõ ràng", "thân thiện"
- Không viết "implement X", "code phải Y" — viết kết quả người dùng/hệ thống quan sát được
- Không bao gồm chi tiết kỹ thuật như error code, tên biến, tên hàm

### Checklist
- Mỗi item là một **việc cần làm**, không phải kết quả
- Actionable: người đọc biết mình phải làm gì
- Có thể tick được khi hoàn thành
- Có thể bao gồm chi tiết kỹ thuật hơn Tiêu chí nghiệm thu, nhưng vẫn tránh over-specify implementation

---

## Lọc nội dung từ Spec (bắt buộc trước khi viết)

Spec artifact thường chứa nhiều nội dung không cần đưa vào story — vì story chỉ phản ánh trạng thái Approved cuối cùng, không phải quá trình soạn thảo. Trước khi viết Mô tả / Luồng xử lý / Tiêu chí nghiệm thu, loại bỏ:

- **Lịch sử revision / decision-log**: các đoạn kiểu "v1.14 → v1.17, round N: sửa X", "Entry NN (decision-log)", ghi chú "[cần verify]" đã chốt xong. Story chỉ lấy kết quả cuối cùng, không lấy quá trình sửa.
- **Nội dung phụ lục / không active**: các phần spec đánh dấu kiểu "[PHỤ LỤC - v2 candidate, KHÔNG active v1]", "deprecated" — không đưa vào story của phiên bản đang active.

Nếu không chắc 1 đoạn là nội dung active hay chỉ là lịch sử/phụ lục → RAISE ba-lead thay vì tự đoán.

---

## Xử lý input

### Mode 1: Từ Spec + PRD (Optimized)
**Khi user paste spec + PRD hoặc cả 2:**

1. **Auto-extract 4 thông tin từ spec:**
   - **Persona**: From Section B "User Story: Là một [role]..."
   - **Goal**: From Section B "tôi muốn [action]"
   - **Business value**: From Section B "để [lý do]" + PRD Why
   - **Context**: From PRD feature description + Spec Mô tả

2. **Confirm 1-2 lần** (thay vì hỏi 4 lần):
   ```
   ✅ Persona: PG thực địa tại quán?
   ✅ Goal: Xem và xóa ảnh chụp chưa đạt?
   ✅ Business value: Không mất thời gian chụp lại?
   ✅ Context: Feature trong VisibilityPRO?
   
   Có đúng không? [Yes/No]
   ```

3. **Output ngay** (không hỏi thêm)

---

### Mode 2: Từ Feature Description Thô
**Khi user paste mô tả tính năng thô (không có spec/PRD):**

- Tự suy luận mode từ context
- Hỏi tối đa 2 câu:
  - Persona là ai?
  - Vấn đề/nhu cầu cụ thể là gì?
- Tổng hợp và viết luôn

---

### Mode 3: Refine/Review
**Khi user paste story có sẵn + nói "sửa" / "review":**
- Review và refine theo INVEST standards
- Suggest improvements

---

### Mode 4: Thêm Tiêu Chí
**Khi user paste story + nói "thêm tiêu chí":**
- Bổ sung tiêu chí nghiệm thu
- Confirm coverage (happy path + edge case + error)

---

## Workflow Optimized cho Spec + PRD

### Step 1: User Input
User cung cấp:
- Spec (04-spec--*.md) hoặc
- PRD (03-planning--prd.md) hoặc
- Cả 2

Hoặc paste content từ spec + PRD trực tiếp

### Step 2: Auto-Extract & Confirm
Mình tự extract từ spec:
- Section B: Persona, Goal, Business value
- PRD: Context, Feature description

Confirm lại 1 lần:
```
Extracted từ spec:
✓ Persona: [role]
✓ Goal: [action]
✓ Business value: [lý do]
✓ Context: [module/feature]

Đúng không?
```

### Step 3: Generate Story
Không hỏi thêm → viết ngay

### Step 4: Output
Ready-to-paste Jira story

---

## Khi nào cần tách story

### Mặc định: 1 màn hình = 1 story
Ưu tiên tách story theo ranh giới màn hình trong spec — mỗi story ứng với 1 màn hình chính, để QC test độc lập từng màn. Chỉ gộp nhiều màn vào 1 story khi:
- Một trong số đó chỉ là popup/dialog phụ thuộc chặt vào màn chính, không đứng độc lập được (VD: popup xác nhận, popup lỗi).
- Tách riêng sẽ làm vỡ 1 luồng nghiệp vụ không thể test được nếu thiếu màn kia.

### Tách thêm vì lý do khác (Spike / quá lớn)
Dù đã tách theo màn, vẫn cân nhắc tách tiếp khi:
- Story chứa "AND" trong tiêu đề → cân nhắc split
- Tiêu chí nghiệm thu vượt quá 8 scenarios → story quá lớn
- Chưa đủ thông tin để ước lượng effort (cần research trước) → tách spike story riêng, ước lượng story chính sau khi spike xong

Pattern split khác (khi tách theo màn không đủ hoặc không áp dụng):
- Theo CRUD: Create / Read / Update / Delete
- Theo persona: từng role khác nhau
- Theo luồng: từng bước trong workflow
- Theo platform: Web / Mobile / API

---

## Anti-patterns — KHÔNG làm

❌ Persona chung chung: "Là người dùng" → ✅ "Là PG đang thực địa tại quán"
❌ Mục tiêu mơ hồ: "tôi muốn quản lý hình ảnh" → ✅ "tôi muốn xem lại và xóa ảnh chụp chưa đạt"
❌ Để trống hoặc lặp lại ở "Để": "để tôi có thể quản lý hình ảnh" → ✅ "để tôi không mất thời gian chụp lại toàn bộ khi chỉ 1 ảnh bị lỗi"
❌ Tiêu chí nghiệm thu mô tả UI: "thì nút chuyển màu xanh" → ✅ "thì hệ thống hiển thị thông báo xác nhận"
❌ Tiêu chí nghiệm thu có implementation detail: "thì gọi API /v1/images/delete" → ✅ "thì ảnh bị xóa khỏi danh sách"
❌ Chỉ có happy path → ✅ phải có đủ 3 loại: luồng chính + điều kiện biên + lỗi
❌ Mô tả hướng về How: "Script đọc file Excel, fetch ảnh, gọi LLM" → ✅ "Pipeline xử lý hàng loạt hình ảnh từ danh sách đầu vào"

---

## Labels thường dùng

| Nhãn | Dùng khi |
|---|---|
| `UI` | Story liên quan đến giao diện người dùng |
| `Mobile` | Feature dành cho mobile app |
| `Backend` | Logic phía server, API, data processing |
| `AI` | Tích hợp hoặc sử dụng AI/LLM |
| `Prompt Engineering` | Viết hoặc cải thiện prompt cho AI |
| `Data Quality` | Xử lý, validate, hoặc correction dữ liệu |
| `Performance` | Tối ưu tốc độ, throughput, scalability |
| `Pipeline` | Luồng xử lý dữ liệu batch hoặc automation |
| `Integration` | Tích hợp hệ thống bên ngoài |
| `Delivery` | Task giao kết quả cho client |
| `QA` | Kiểm tra, test, quality assurance |
| `Spike` | Research hoặc đánh giá kỹ thuật |
| `Business Logic` | Quy tắc nghiệp vụ cốt lõi |

---

## Ví dụ tham khảo

Story mẫu viết đúng format (backend/pipeline, không technical trong Mô tả và Tiêu chí nghiệm thu):

```
## VPR-001 Hệ thống tự động gắn cờ cửa hàng không tuân thủ trưng bày liên tiếp

**Là** Brand Manager quản lý chuỗi cửa hàng, **tôi muốn** được thông báo khi
một cửa hàng không đạt chuẩn trưng bày 3 lần liên tiếp trong kỳ đánh giá,
**để** tôi ưu tiên xử lý kịp thời thay vì phải kiểm tra từng cửa hàng thủ công.

**Mô tả:**
Trong kỳ đánh giá hiện tại, một số cửa hàng liên tục không đạt chuẩn trưng bày
nhưng Brand Manager chỉ phát hiện cuối kỳ khi đã trễ để can thiệp.
Hệ thống cần tự động nhận diện và đánh dấu các cửa hàng này ngay khi đạt ngưỡng.

**Tiêu chí nghiệm thu:**
- Nếu một cửa hàng có 3 lượt đánh giá Không đạt liên tiếp trong cùng kỳ,
  khi kết quả lượt thứ 3 được lưu, thì cửa hàng đó được gắn cờ tự động trong danh sách.
- Nếu cửa hàng đạt 1 lượt bất kỳ xen giữa, khi lượt đó được ghi nhận,
  thì chuỗi đếm liên tiếp reset về 0.
- Nếu Brand Manager xem báo cáo kỳ, khi lọc theo "Cờ: Có",
  thì chỉ hiển thị các cửa hàng đang bị gắn cờ trong kỳ đó.
```

---

## INVEST Self-check (BA internal review)

> Dùng để tự kiểm tra trước khi xuất. Không đưa vào output Jira.

| Tiêu chí | Câu hỏi |
|---|---|
| **Independent** | Story này chạy được độc lập, không bị block bởi story khác? |
| **Negotiable** | Không over-specify, không chỉ định technology cụ thể? |
| **Valuable** | Mang lại giá trị rõ ràng cho user hoặc business? |
| **Estimable** | Dev team có thể ước lượng effort không? Nếu không → tách spike. |
| **Small** | Hoàn thành trong 1 sprint? Nếu không → cân nhắc split. |
| **Testable** | Tiêu chí nghiệm thu đủ để QA viết test case không? |
