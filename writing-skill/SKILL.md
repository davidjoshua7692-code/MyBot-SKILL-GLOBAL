---
name: writing-skill
description: Create or improve OpenClaw/AgentSkills skills. Use when the user wants to create a new skill, fix or optimize an existing SKILL.md, or learn skill writing best practices. Covers YAML frontmatter, description writing, progressive disclosure, and directory structure.
---

# Skill Writing Guide

A concise guide for creating effective OpenClaw skills.

## Quick Start

### Minimal Skill Structure

```
skill-name/
└── SKILL.md
```

That's it. Only `SKILL.md` is required.

### YAML Frontmatter (Required)

```yaml
---
name: skill-name
description: What the skill does and when to use it.
---
```

**Two fields only**: `name` and `description`. No `version`, `author`, etc.

### Description = Trigger

The `description` is the **primary trigger mechanism**. Models use it to decide whether to load your skill.

**Write descriptions that include:**
- **What**: What the skill does
- **When**: Specific triggers/contexts for when to use it

**Example:**
```yaml
description: Validate JSON syntax and OpenClaw config schema. Use when editing or writing .json files (openclaw.json, mcporter.json, jobs.json) to catch errors before Gateway restart.
```

**Bad example (too vague):**
```yaml
description: Validate JSON files.
```

## Skill Loading Levels

Skills use progressive disclosure - only load what's needed:

1. **Metadata** (name + description) - Always in context (~100 words)
2. **SKILL.md body** - Loaded when skill triggers (<5k words)
3. **Bundled resources** - Loaded as needed (unlimited)

**Principle**: Keep SKILL.md lean. Move detailed content to reference files.

## Directory Structure (Optional)

```
skill-name/
├── SKILL.md           # Required - Core instructions
├── scripts/           # Executable code (Python/Bash/etc.)
├── references/        # Documentation loaded as needed
└── assets/            # Files used in output (templates, images)
```

### When to Use Each Folder

| Folder | Purpose | Example |
|--------|---------|---------|
| `scripts/` | Deterministic, reusable code | `scripts/rotate_pdf.py` |
| `references/` | Domain docs, schemas, patterns | `references/api-docs.md` |
| `assets/` | Output templates, images | `assets/template.html` |

## Naming Conventions

- Lowercase letters, digits, hyphens only
- Verb-led phrases preferred: `pdf-editor`, `json-validator`
- Keep under 64 characters
- Folder name = skill name

## OpenClaw-Specific Metadata (Optional)

Add to frontmatter for OpenClaw features:

```yaml
---
name: my-skill
description: Skill description here.
metadata: { "openclaw": { "emoji": "🔧", "requires": { "bins": ["python3"] } } }
---
```

**Note**: `metadata` must be a **single-line JSON object**.

### Common Metadata Fields

- `emoji` - Display emoji in macOS Skills UI
- `homepage` - URL shown in Skills UI
- `requires.bins` - List of required binaries in PATH
- `requires.env` - List of required environment variables
- `requires.config` - List of required config paths in openclaw.json
- `os` - Platform list: `["darwin", "linux", "win32"]`

## What NOT to Include

Avoid creating extra documentation files:
- README.md
- INSTALLATION.md
- CHANGELOG.md
- QUICK_REFERENCE.md

Skills are for AI agents, not humans. Only include what the agent needs.

## References

For detailed information, see:

- **[frontmatter.md](references/frontmatter.md)** - Complete YAML frontmatter reference
- **[best-practices.md](references/best-practices.md)** - Writing effective skill instructions
- **[openclaw-features.md](references/openclaw-features.md)** - OpenClaw-specific features (gating, config, etc.)

---

_Based on OpenClaw official documentation and skill-creator skill_
