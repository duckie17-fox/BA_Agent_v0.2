# BA Pipeline — Setup Guide

## Yêu cầu

- Claude Code (claude.ai/code hoặc VS Code extension)
- VPN NDS (để dùng NDS-Knowledge + Jira)

## Cài đặt

### 1. Clone project

Mở project này trong Claude Code. Không cần config path gì — artifacts lưu tại `artifacts/` trong project root, skills đọc từ `.claude/skills/`.

### 2. Cấu hình MCP

Copy file template rồi điền thông tin:

```
cp .claude/settings.template.json .claude/settings.local.json
```

Sau đó mở `settings.local.json` và điền:

| Field | Lấy ở đâu |
|---|---|
| `FIGMA_ACCESS_TOKEN` | figma.com → Settings → Personal access tokens |
| `FIGMA_FILE_KEY` | URL của Figma file: `figma.com/file/**FILE_KEY**/...` |
| `MCP_BEARER_TOKEN` (NDS-Knowledge) | Liên hệ admin NDS để lấy token NDS-Knowledge |
| `JIRA_EMAIL` + `JIRA_API_TOKEN` | Jira → Profile → Manage API tokens |

> **Không commit `settings.local.json`** — file này chứa token cá nhân.

### 3. MCP nào bắt buộc?

| MCP | Bắt buộc? | Nếu không có |
|---|---|---|
| `figma-mcp-go` | Bắt buộc cho Design step (Bước 4) | Pipeline dừng tại Bước 4. Intake → Spec vẫn chạy bình thường. |
| `NDS-Knowledge` | Bắt buộc cho Discovery + Review | Agents fallback sang external research, một số context về hệ thống NDS có thể thiếu. |
| `ndsvn-jira` | Tùy chọn | User stories lưu local, push Jira thủ công. |

### 4. Chạy thử

Paste một ticket/yêu cầu vào Claude Code và gọi `@ba-lead`. Pipeline sẽ tự chạy từ Intake đến Stories.
