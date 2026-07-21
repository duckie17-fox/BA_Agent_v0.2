---
name: context-kit
description: "Set up or sync a lightweight external-memory system (PLAN.md, STATE.md, MEMORY.md, plus a checkpoint-discipline section appended to CLAUDE.md) for a project, so Claude Code and Claude Cowork stay oriented across sessions without re-explaining context, losing state to compaction, or re-reading long history each time. Use this skill whenever the user asks to set up, install, or sync a 'context kit', says things like 'khởi tạo context cho project này', 'setup PLAN/STATE/MEMORY cho project', 'đồng bộ lại state cho project đang làm dở', 'project này cần context kit', wants a brand-new project bootstrapped with this file system, or wants an in-progress project's context backfilled into this structure from its existing files/git history. Always use this skill rather than improvising ad-hoc PLAN/STATE files by hand — it enforces the correct structure and checkpoint discipline."
---

# Context Kit

Thiết lập hoặc đồng bộ 4 thành phần cho 1 project: `PLAN.md` (tĩnh), `STATE.md` (động, ghi đè), `MEMORY.md` (tích lũy, append-only, ghi nhớ đầy đủ), và 1 section được append vào `CLAUDE.md` để ép checkpoint disipline.

Nguyên tắc nền: STATE.md trả lời "đang ở đâu", MEMORY.md trả lời "tại sao lại thế", PLAN.md trả lời "mục tiêu là gì". Không được gộp 3 cái này lại — đó là lỗi gốc mà skill này tồn tại để tránh.

## Bước 0 — Xác định mode

Nhìn vào folder project:
- **NEW** — folder trống hoặc gần như trống (chưa có code/docs đáng kể, chưa có PLAN/STATE/MEMORY).
- **EXISTING** — đã có code/docs/lịch sử commit, đang làm dở, hoặc đã có sẵn 1 phần của bộ file này.

Nếu không rõ, hỏi thẳng người dùng 1 câu thay vì đoán.

## Bước 1 — Tạo/ghi 3 file

Copy nội dung từ `assets/PLAN.template.md`, `assets/STATE.template.md`, `assets/MEMORY.template.md` vào gốc project (`PLAN.md`, `STATE.md`, `MEMORY.md`). Nếu file nào đã tồn tại rồi, **không ghi đè** — báo cho người dùng biết file đã có, hỏi có muốn merge hay giữ nguyên.

### Nếu mode = NEW
Hỏi người dùng tối đa 3-4 câu ngắn (không dài dòng):
1. Mục tiêu project này là gì (1-2 câu)?
2. Trong phạm vi / ngoài phạm vi có gì rõ ràng ngay từ đầu không?
3. Task đầu tiên định làm là gì?

Điền câu trả lời trực tiếp vào `PLAN.md` (mục tiêu, scope, milestone đầu) và `STATE.md` (đang làm = task đầu tiên, next step = bước cụ thể để bắt tay vào ngay). `MEMORY.md` để trống — không bịa quyết định/gotcha chưa xảy ra.

### Nếu mode = EXISTING
Tự làm trước, đừng bắt người dùng kể lại từ đầu:
1. Đọc README, docs, comment TODO/FIXME trong code nếu có.
2. Xem git log gần đây (`git log --oneline -20`) để nắm hướng đi và tiến độ gần nhất.
3. Nếu đã có STATE/PLAN/MEMORY cũ (kể cả tên khác, format khác) — đọc và migrate nội dung sang format chuẩn, không bỏ sót.

Từ đó soạn **bản nháp** PLAN.md / STATE.md / MEMORY.md — nhưng đánh dấu rõ phần nào là suy luận của mình (ví dụ: `[suy đoán từ code — anh xác nhận lại]`) và phần nào là fact chắc chắn (đọc thẳng từ file/commit message). Không được viết suy đoán như thể là sự thật đã xác nhận.

Sau khi soạn xong, đưa bản nháp cho người dùng xem, hỏi xác nhận/sửa trước khi coi đó là nguồn sự thật chính thức. Đây là bước bắt buộc — không tự ý finalize.

## Bước 2 — Append vào CLAUDE.md

- Nếu project **chưa có** `CLAUDE.md`: tạo mới với nội dung từ `assets/CLAUDE.section.md`.
- Nếu project **đã có** `CLAUDE.md`: đọc file hiện tại, append nội dung `assets/CLAUDE.section.md` vào cuối, **giữ nguyên toàn bộ nội dung cũ**. Không xoá, không sửa rule đang có sẵn. Nếu phát hiện rule cũ xung đột với checkpoint discipline mới (ví dụ đã có quy ước ghi log kiểu khác), báo cho người dùng biết chỗ xung đột thay vì tự ý quyết định giữ cái nào.

## Bước 3 — Xác nhận lần cuối

Tóm tắt ngắn gọn cho người dùng: đã tạo/sửa file nào, mode nào, có phần nào cần họ tự điền/xác nhận thêm không. Không cần dài dòng giải thích lại toàn bộ triết lý hệ thống — họ đã biết, chỉ cần biết đã xong và cần check gì.

## Ghi nhớ khi vận hành (áp dụng cho mọi lần Claude Code/Cowork làm việc trong project đã cài skill này)

- STATE.md luôn ngắn, đọc trong 5 giây, ghi đè chứ không append.
- MEMORY.md append đầy đủ, ghi nhớ toàn bộ quyết định và bài học — không tự xoá/archive.
- PLAN.md gần như bất biến — chỉ sửa khi scope thật sự đổi, không phải mỗi lần có tiến độ mới.
- Checkpoint STATE.md ngay khi xong subtask/chuyển việc/context dài ra — không đợi tới cuối phiên.
