# Write-Spec Skill — v2.0 Updates

> Summary of changes to support descriptive file naming and versioned folder structure for feature specs and change requests.

---

## What Changed

### 1. Descriptive File Naming Convention

**Old:** `05-spec.md`, `06-design.md`

**New:** Descriptive, self-documenting names

```
01-intake--qualified.md          # Qualification assessment
02-discovery--problem-statement.md
03-planning--prd.md
04-spec--feature-name.md         # ← write-spec outputs this
05-design--instruction.md
```

**Benefits:**
- File names are self-explanatory
- Pipeline stage clear at a glance
- Easier to navigate and organize
- Consistent with discovery → planning → specification → design flow

---

### 2. Versioned Folder Structure

**Old:** All versions in single file (e.g., `05-spec.md` v1.0, v1.1, v1.2)

**New:** Each version in separate folder with root metadata

```
artifacts/VPR-001--display-rules/
├── _versions.md           ← Version history table
├── _meta.json             ← Current version pointers
├── v1.0/                  ← Base release
│   ├── 04-spec--display-rules.md
│   ├── diagrams/
│   └── figma-code/
├── v1.1/                  ← CR-VPR-001-001
│   ├── 04-spec--display-rules.md
│   ├── _cr-info.json
│   └── diagrams/
└── v1.2/                  ← CR-VPR-001-002
    ├── 04-spec--display-rules.md
    ├── _cr-info.json
    └── diagrams/
```

**Benefits:**
- v1.0 is frozen baseline (never modified)
- Each CR tracked separately with full context
- Root `_meta.json` always points to latest
- Complete version history in `_versions.md`
- Easy to compare versions or rollback

---

### 3. CR Metadata Files

**New templates for change request tracking:**

#### `_versions.md`
Version history table at feature level:

| Version | Type | Release Date | CR ID | Title | Status | Links |
|---------|------|--------------|-------|-------|--------|-------|
| v1.0 | Initial | Date | — | Title | Approved | [Link] |
| v1.1 | Patch | Date | CR-ID | Title | Approved | [Link] |

#### `_meta.json`
Current version pointers (root level):

```json
{
  "feature-id": "VPR-001",
  "latest-version": "v1.2",
  "current-version-links": {
    "spec": "v1.2/04-spec--display-rules.md",
    "prd": "v1.2/03-planning--prd.md"
  }
}
```

#### `_cr-info.json`
CR details (one per version folder):

```json
{
  "cr-id": "CR-VPR-001-001",
  "version": "v1.1",
  "type": "patch",
  "title": "Add planogram mode",
  "breaking-changes": false
}
```

---

### 4. CR File Naming

When writing specs for CRs, use `cr-` prefix for discovery/planning:

```
v1.1/
├── 02-discovery--cr-problem-statement.md
├── 03-planning--cr-prd.md
├── 04-spec--display-rules.md  (NO prefix — always merged)
└── _cr-info.json
```

**Why?** Makes CR-specific sections clear while showing 04-spec is the complete, up-to-date spec.

---

### 5. Version Numbering Rules

| Type | Pattern | When |
|------|---------|------|
| New Feature | v1.0 | First release |
| Patch CR | v1.1, v1.2, … | Bug fixes, minor enhancements, backward compatible |
| Major CR | v2.0, v3.0, … | Breaking changes, major redesign, incompatible |

---

## Files Updated

### Skill Definition
- **`SKILL.md`**
  - Updated Output File section with new versioned folder guidance
  - Added "Versioning & CR Handling" section with decision rules
  - Added reference to `versioning-guide.md`

### Templates & Guides
- **`assets/spec-template.md`**
  - Added `version: v1.0` and `cr-id: null` metadata fields

- **`references/format-guide.md`**
  - Added "Artifact File Naming Convention" section
  - Added "Versioned Folder Structure" explanation
  - Added "Version Numbering Rules" table
  - Added "CR File Naming" section
  - Added templates for `_versions.md`, `_meta.json`, `_cr-info.json`

- **`references/format-guide.md`** (NEW)
  - Complete guide to versioning and CR workflow
  - Quick decision tree
  - Example workflows
  - Common questions & answers

- **`references/format-guide.md`** (NEW)
  - Real-world example: Display Rules with v1.0, v1.1, v1.2
  - Complete file listings
  - CR-specific content examples
  - How files flow through pipeline

### Pipeline Configuration
- **`CONNECTORS.md`** (root `/.claude/`)
  - Updated "Output File Convention" section with new naming
  - Updated folder structure diagrams to show versioned layout
  - Added "Version Management" subsection
  - Added CR workflow details (determine version type, create folder, update files, notify)
  - Updated "Tên file theo thứ tự pipeline" with descriptive names

---

## Quick Reference — For Users

### Creating a New Feature Spec

1. **Output file path:**
   ```
   artifacts/VPR-001--display-rules/v1.0/04-spec--display-rules.md
   ```

2. **Create root metadata:**
   - `_versions.md` with v1.0 row
   - `_meta.json` pointing to v1.0

### Creating a CR Spec

1. **Determine version type:**
   - Patch (v1.1) or Major (v2.0)?

2. **Copy and update:**
   ```bash
   cp -r v1.0/ v1.1/
   ```

3. **Update CR files:**
   - `02-discovery--cr-problem-statement.md` (new)
   - `03-planning--cr-prd.md` (new)
   - `04-spec--display-rules.md` (updated)
   - `_cr-info.json` (new)

4. **Update root metadata:**
   - Add row to `_versions.md`
   - Update `latest-version` in `_meta.json`

---

## Backward Compatibility

**Existing specs can migrate at their own pace:**
- Continue using `05-spec.md` naming if preferred
- Can rename to descriptive names (`04-spec--feature.md`) when next updated
- New specs should use descriptive naming from start

---

## Examples Provided

1. **Display Rules (v1.0 → v1.1 → v1.2)**
   - Complete folder structure example
   - Sample `_versions.md`, `_meta.json`, `_cr-info.json` content
   - File flow through each version

2. **Multiple sub-features**
   - How to organize when PRD has multiple features
   - Example: `04-spec--ai-evaluation.md` + `04-spec--manual-evaluation.md`

---

## Where to Find Guidance

| Need | Read |
|------|------|
| General overview | This file (UPDATES.md) |
| Detailed sections (E, F, G, etc.) | `references/format-guide.md` |
| Versioning workflow & CR handling | `references/format-guide.md` |
| Real-world example | `references/format-guide.md` |
| Field types & patterns | `references/field-types.md` |
| PlantUML diagrams | `references/plantuml-guide.md` |
| Blank template | `assets/spec-template.md` |
| Skill input/output | `SKILL.md` |

---

## No Breaking Changes

- All updates are **additive**
- Old naming still works (just not recommended for new specs)
- Version metadata is optional (only needed for CR tracking)
- Existing specs don't need immediate updates

---

## Portability

✅ **Fully portable** — No hardcoded paths
- Base folder auto-detected (`artifacts/`, `docs/`, `specs/`, etc.)
- Works in any project structure
- Same skill for all teams and organizations

See `PORTABILITY.md` for details on how the skill adapts to your project.

---

## Support

Questions? Check:
1. `references/format-guide.md` → Quick decision tree
2. `references/format-guide.md` → Real example
3. `references/format-guide.md` → Detailed section guidance
4. `PORTABILITY.md` → How to use in your project


---
> **Note lịch sử (v2):** File này ghi lại đợt nâng v2.0 (từng có mô hình *versioned* với `_cr-info.json`/`v1.1/`). Mô hình hiện hành là **flat SSOT** — xem SKILL.md + format-guide.md. Giữ file làm lịch sử, KHÔNG dùng làm hướng dẫn hiện tại.
