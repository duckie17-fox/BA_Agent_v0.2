# Problem Statement Skill

Skill giúp Claude tự động generate Problem Statement chuẩn (Context + HMW + User Story)
từ interview notes hoặc ticket mô tả. Tích hợp sẵn HITL checklist để BA validate trước khi đi tiếp.

## Cài đặt

### Trên Claude.ai
1. Zip toàn bộ folder này: `zip -r problem-statement-skill.zip problem-statement-skill/`
2. Vào **Settings > Capabilities > Skills**
3. Upload file zip
4. Bật toggle để activate

### Trên Claude Code (CLI)
```bash
cp -r problem-statement-skill ~/.claude/skills/
```
Skill tự động active, không cần restart.

## Cách dùng

### Trigger phrases
- "Viết problem statement cho yêu cầu này"
- "Frame lại vấn đề từ notes interview"
- "Định nghĩa HMW cho ticket này"
- "Reframe requirement này thành problem"

### Input tốt nhất
Paste trực tiếp một trong:
- Notes từ buổi 5W2H interview
- Root Cause Brief sau 5 Whys
- Mô tả ticket từ CS/Sales (dù còn thô)

### Ví dụ
```
Dùng problem-statement skill để xử lý notes này:

[paste notes của bạn vào đây]
```

## Output bạn sẽ nhận được
- **Context** — 2-3 câu mô tả vấn đề thuần, không có solution
- **HMW Question** — câu hỏi ideation với constraint rõ ràng
- **User Story** — đủ 3 phần As a / I want to / So that
- **Confidence level** — để biết cần hỏi thêm gì
- **HITL checklist** — 4 câu hỏi để bạn validate trước khi dùng

## Khi nào cần chỉnh tay

Skill sẽ báo Confidence = Thấp/Trung bình khi:
- Input quá ngắn hoặc thiếu context về user
- Input mô tả solution, không mô tả vấn đề
- Không rõ business impact

Trong các trường hợp này, bổ sung thêm context rồi chạy lại.
