# Write-Spec Skill v2.0

> Writes detailed functional specs with descriptive naming and versioned folder structure. Fully portable — works in any project.

---

## What This Skill Does

Generates **feature specifications** with:
- ✅ Descriptive file naming (01-intake, 02-discovery, 03-planning, 04-spec, 05-design)
- ✅ Versioned folder structure (v1.0, v1.1, v2.0 for change requests)
- ✅ PlantUML/Mermaid workflow diagrams
- ✅ Field-level specifications and validation rules
- ✅ Business rules, edge cases, and NFRs
- ✅ HITL checklist for quality validation

---

## Quick Start

### 1. Create Base Folder

```bash
mkdir -p artifacts/
# or: mkdir -p docs/
# or: mkdir -p specs/
```

### 2. Run the Skill

```
/write-spec
```

The skill will:
- Auto-detect your base folder
- Ask for feature details
- Generate versioned spec structure

### 3. Output

Specs saved to:
```
artifacts/VPR-001--display-rules/
├── _versions.md        # Version history
├── _meta.json          # Current pointers
└── v1.0/
    ├── 01-intake--qualified.md
    ├── 02-discovery--problem-statement.md
    ├── 03-planning--prd.md
    ├── 04-spec--display-rules.md     ← Main output
    ├── 05-design--instruction.md
    ├── diagrams/
    └── figma-code/
```

---

## File Structure

| File | Purpose | Read |
|------|---------|------|
| `SKILL.md` | Skill definition & workflow | First |
| `UPDATES.md` | What changed in v2.0 | Next |
| `PORTABILITY.md` | How to use in your project | Important |
| `README.md` | This file | You are here |
| `references/format-guide.md` | Section-by-section guidance | When writing |
| `references/format-guide.md` | CR workflow & version rules | For CRs |
| `references/format-guide.md` | Real-world example | For reference |
| `assets/spec-template.md` | Blank template to fill in | As needed |

---

## Key Concepts

### Descriptive Naming

Files are self-documenting:

```
01-intake--qualified.md           # Qualification assessment
02-discovery--problem-statement.md
03-planning--prd.md
04-spec--feature-name.md         # Feature specification
05-design--instruction.md        # Design & components
```

**Benefits:**
- Clear which stage each file represents
- Easy to navigate and understand
- Consistent naming across all projects

### Versioned Structure

Each feature gets a folder with version subfolders:

```
VPR-001--display-rules/          # Feature folder
├── _versions.md                 # Version history
├── _meta.json                   # Current version pointers
├── v1.0/                        # Initial release
├── v1.1/                        # Patch CR
└── v2.0/                        # Major CR
```

**Benefits:**
- v1.0 is frozen baseline (never modified)
- Each CR tracked separately
- Complete version history
- Easy to compare or rollback

### Version Numbering

| Type | Pattern | When |
|------|---------|------|
| New Feature | v1.0 | First release |
| Patch CR | v1.1, v1.2, … | Bug fixes, minor changes |
| Major CR | v2.0, v3.0, … | Breaking changes |

---

## Using the Skill

### New Feature

```
/write-spec

Input: Feature from approved PRD
Output: artifacts/VPR-001--display-rules/v1.0/04-spec--display-rules.md
```

### Change Request

```
/write-spec

Input: Feature + CR details
Output: artifacts/VPR-001--display-rules/v1.1/04-spec--display-rules.md
        (also creates _cr-info.json with CR metadata)
```

---

## Spec Contents

Each spec includes:

| Section | What |
|---------|------|
| A. Jira Link | Ticket reference |
| B. User Story | Role + action + goal |
| C. Workflow | Mermaid/PlantUML diagram + flow description |
| D. Design | Link to design instruction or Figma |
| E. Business Rules | Ràng buộc, constraints, conditions |
| F. Functional Requirements | What system must do |
| G. Non-Functional Requirements | Performance, security, scalability |
| H. Edge Cases | Error scenarios, boundary conditions |
| I. Field Tables | Field definitions, validation rules |
| J. Use Cases | Related features (optional) |
| K. Open Questions | Blocking questions (optional) |
| HITL Checklist | Quality validation |
| L. Figma Code | Component code from Figma (optional) |

---

## Portability

✅ **Fully portable** — Share with any team

- No absolute paths (`/d/MyCompany/...`)
- No hardcoded folder names
- Auto-detects base folder (artifacts/, docs/, specs/)
- Same skill works everywhere

See `PORTABILITY.md` for details.

---

## Guidance Documents

### For Daily Use
- **`SKILL.md`** — How the skill works, input/output
- **`assets/spec-template.md`** — Blank template to fill in

### For Detailed Guidance
- **`references/format-guide.md`** — Section-by-section help
  - What to put in each section (E, F, G, H, I, etc.)
  - Examples and patterns
  - Templates for _versions.md, _meta.json, _cr-info.json

### For Versioning & CRs
- **`references/format-guide.md`** — Complete CR workflow
  - Decision tree: patch vs major CR
  - Step-by-step workflow
  - File naming conventions
  - FAQ section

### For Real-World Example
- **`references/format-guide.md`** — Display Rules case study
  - Complete folder listings
  - Sample file contents
  - Multiple CR examples
  - How files flow through versions

### For Your Project
- **`PORTABILITY.md`** — How to adapt to your setup
  - Base folder auto-detection
  - Project-specific customization
  - Troubleshooting

---

## Common Tasks

### Create a New Spec

1. Get approved PRD from write-prd
2. Run `/write-spec`
3. Skill outputs to `[BASE]/VPR-001--feature/v1.0/04-spec--feature.md`

### Handle a Change Request

1. Classify: patch (v1.1) or major (v2.0)?
2. Copy v1.0 → v1.1 folder
3. Update: 02-discovery--cr-*, 03-planning--cr-*, 04-spec--*
4. Create: _cr-info.json
5. Update root: _versions.md, _meta.json

### Create Multiple Specs (One PRD, Multiple Features)

```
VPR-050--evaluation-plan/
└── v1.0/
    ├── 04-spec--ai-evaluation.md      # Feature 1
    ├── 04-spec--manual-evaluation.md  # Feature 2
    ├── 05-design--ai-evaluation.md
    ├── 05-design--manual-evaluation.md
    └── diagrams/
```

---

## Support

| Question | Read |
|----------|------|
| "How do I get started?" | This README + PORTABILITY.md |
| "What goes in each section?" | format-guide.md |
| "How do CRs work?" | format-guide.md (§ CR workflow) |
| "Show me an example" | README.md (§ Flat Example v2) |
| "How do I use this in my project?" | PORTABILITY.md |
| "What changed in v2.0?" | UPDATES.md |

---

## What's New in v2.0

✅ **Descriptive file naming** — 01-intake, 02-discovery, 03-planning, 04-spec, 05-design

✅ **Versioned folder structure** — v1.0, v1.1, v2.0 for managing CRs

✅ **CR metadata files** — _versions.md, _meta.json, _cr-info.json

✅ **Portable** — No hardcoded paths, works in any project

See `UPDATES.md` for detailed changes.

---

## Distribution

**To share this skill with your team:**

1. Copy the entire folder:
   ```bash
   cp -r .claude/skills/write-spec /path/to/team/repo/.claude/skills/
   ```

2. Document in your project README:
   ```markdown
   ## Using write-spec

   Specs are stored in: `artifacts/`
   See `.claude/skills/write-spec/README.md` for guidance.
   ```

3. Team members create their base folder and run:
   ```bash
   /write-spec
   ```

No customization needed — the skill adapts to each project.

---

## Notes

- **All output in Vietnamese** (can be adapted)
- **Integrates with write-prd** (use write-prd first)
- **Integrates with design-instruction** (after spec approved)
- **Zero dependencies** — pure markdown + JSON files
- **Version control friendly** — all text files, git-trackable

---

## Quick Links

- 📖 [SKILL.md](SKILL.md) — How to use the skill
- 🔄 [UPDATES.md](UPDATES.md) — What's new in v2.0
- 🌍 [PORTABILITY.md](PORTABILITY.md) — Use in your project
- 📋 [format-guide.md](references/format-guide.md) — Section guidance
- 📚 [versioning-guide.md](references/format-guide.md) — CR workflow
- 📄 [example-folder-structure.md](references/format-guide.md) — Real example
- ✏️ [spec-template.md](assets/spec-template.md) — Blank template


---

## v2 — Flat Example (thay `example-folder-structure.md` đã xóa)

```
artifacts/VPR-001--display-rules/
├── 00-decision-log.md        _versions.md   _meta.json
├── 01-qualified-ticket.md    02-research.md  02-NDS-Knowledge-context.md
├── 03-problem-statement.md   03-planning--prd.md
├── 04-spec--display-rules.md      ← SSOT (changelog nội bộ)
├── 05-design--display-rules.md    05-design-state.md
├── 06-stories/display-rules.md
├── cr/CR-001--unlock-inprogress.md
└── impacts/CR-001-impact.md       _staleness.md
```
Flat, không `v1.1/` subfolder, không `_cr-info.json`. CR = update `04-spec` in-place + changelog + `_versions.md` row.

> **Guidance docs (v2):** `versioning-guide.md` và `example-folder-structure.md` đã bỏ (mâu thuẫn flat SSOT). Folder/CR/versions/meta format nay ở `references/format-guide.md`.
