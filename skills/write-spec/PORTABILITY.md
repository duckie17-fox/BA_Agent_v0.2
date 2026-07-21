# Portability Guide — Using write-spec in Your Project

> This skill is designed to be portable across different projects and organizations.

---

## How the Skill Adapts to Your Project

### Base Folder Auto-Detection

The write-spec skill **automatically detects** where to save output:

1. **Check for common base folders** (in order):
   - `artifacts/` — NDS project standard
   - `docs/` — Generic documentation folder
   - `specs/` — Alternative specs folder
   - `requirements/` — Alternative requirements folder

2. **If none exist**, the skill will ask you:
   ```
   📁 Which folder should I save specs to?
   - artifacts/
   - docs/
   - specs/
   - Other (specify)
   ```

3. **Once detected**, all output uses that folder as the base:
   ```
   [YOUR-BASE-FOLDER]/VPR-001--display-rules/v1.0/04-spec--display-rules.md
   ```

### No Hardcoded Paths

✅ **Portable** — Works in any project structure
- Spec files: `[BASE]/[FEATURE-ID]--[slug]/v1.0/04-spec--*.md`
- Diagrams: `[BASE]/[FEATURE-ID]--[slug]/v1.0/diagrams/`
- Figma code: `[BASE]/[FEATURE-ID]--[slug]/v1.0/figma-code/`

❌ **Not portable** — Would fail in different projects
- Spec files: `/path/to/my-project/artifacts/VPR-001--display-rules/...` ← absolute path
- Hard-coded folder names for specific companies

---

## Recommended Folder Setup

Before using the skill, create one of these folder structures:

### Option A: Artifacts Folder (Recommended)
```
project/
├── artifacts/          ← Where all specs live
│   └── VPR-001--display-rules/
│       └── v1.0/
└── README.md
```

### Option B: Docs Folder
```
project/
├── docs/               ← Where all specs live
│   └── VPR-001--display-rules/
│       └── v1.0/
└── README.md
```

### Option C: Specs Folder
```
project/
├── specs/              ← Where all specs live
│   └── VPR-001--display-rules/
│       └── v1.0/
└── README.md
```

**The skill will detect whichever folder exists.**

---

## Using in a New Project

### Step 1: Create Base Folder

Choose one of the options above and create the folder. Example:

```bash
mkdir -p artifacts
```

### Step 2: Run write-spec

The skill will auto-detect `artifacts/` and use it for all output.

### Step 3: Output Structure

Specs will be saved as:
```
artifacts/VPR-001--feature-name/
├── _versions.md
├── _meta.json
└── v1.0/
    ├── 01-intake--qualified.md
    ├── 02-discovery--problem-statement.md
    ├── 03-planning--prd.md
    ├── 04-spec--feature-name.md
    ├── 05-design--instruction.md
    ├── diagrams/
    └── figma-code/
```

---

## For Distributed Teams

If your team uses this skill across multiple projects:

1. **Standardize base folder choice** in your organization:
   - Document in your project README or CONTRIBUTING.md
   - Example: "Specs go in `artifacts/`"

2. **Each project is independent:**
   - Project A: `artifacts/` → stores all specs for Project A
   - Project B: `docs/` → stores all specs for Project B
   - Project C: `specs/` → stores all specs for Project C
   - ✅ No conflicts, no shared state

3. **Share the skill** without modification:
   - No need to customize paths for each team member
   - No need to maintain per-project versions
   - Everyone can use the same skill in their project

---

## Customizing the Base Folder (Advanced)

If your project uses a different folder naming convention:

### Option 1: Create a Symlink
```bash
# If your specs are in "design/specs/" but you prefer to call it "artifacts/"
ln -s design/specs artifacts
```

### Option 2: Rename Your Folder
```bash
# If you currently have "design-docs/" but want standardized name
mv design-docs/ artifacts/
```

### Option 3: Answer the Prompt
When the skill asks "Which folder?", you can specify any existing folder:
```
Which folder should I save specs to?
- artifacts/
- docs/
- specs/
- Other (specify): design/documentation/features
```

---

## What Gets Saved Where

### Always Portable
```
[BASE]/VPR-001--display-rules/          ← Feature folder (portable)
├── _versions.md                         ← Version history (portable)
├── _meta.json                           ← Metadata (portable)
└── v1.0/
    ├── 04-spec--display-rules.md       ← Spec (portable)
    ├── diagrams/                        ← Diagrams (portable)
    └── figma-code/                      ← Figma exports (portable)
```

### Never Hardcoded
❌ Don't do this:
```
/path/to/my-project/artifacts/VPR-001--display-rules/...
```

✅ Do this instead:
```
artifacts/VPR-001--display-rules/...     ← Relative path only
```

---

## Troubleshooting Portability

### Issue: "Folder not found"
**Solution:** Create one of the standard base folders:
```bash
mkdir -p artifacts/
# OR
mkdir -p docs/
# OR
mkdir -p specs/
```

### Issue: "Output saved to wrong location"
**Solution:** Check that you have ONLY ONE base folder. If you have both `artifacts/` and `docs/`, the skill uses this priority:
1. artifacts/
2. docs/
3. specs/
4. requirements/

Remove or rename extra folders to clarify.

### Issue: "Different teams using different folders"
**Solution:** This is fine! Each project can use its preferred folder. The skill adapts automatically. Just document your choice in your project:

```markdown
# Project Setup

Specs are stored in:
📁 artifacts/

All feature specs follow the format:
artifacts/[FEATURE-ID]--[slug]/v1.0/04-spec--*.md
```

---

## Summary

| Aspect | How it Works |
|--------|---|
| **Paths** | Relative only — no absolute/hardcoded paths |
| **Base folder** | Auto-detected from standard names; or ask user |
| **Per-project config** | Zero configuration needed |
| **Shared across teams** | ✅ Same skill works everywhere |
| **Migration** | ✅ Change base folder → all specs follow |
| **Version control** | ✅ Works in any git repo |

