# YAML Frontmatter Reference

Complete reference for SKILL.md frontmatter.

## Required Fields

### name

The skill identifier. Must match the folder name.

```yaml
name: json-validator
```

**Rules:**
- Lowercase letters, digits, hyphens only
- Under 64 characters
- Must match directory name

### description

The **primary trigger mechanism**. Models use this to decide when to load your skill.

```yaml
description: Create or improve OpenClaw skills. Use when the user wants to create a new skill, fix an existing SKILL.md, or learn skill writing.
```

**Guidelines:**
- Include **what** the skill does
- Include **when** to use it (specific triggers)
- Keep it under 200 characters if possible
- No quotes needed unless string contains special characters

**Good examples:**
```yaml
description: Validate JSON syntax and schema. Use when editing openclaw.json, mcporter.json, or any .json config files.
description: Deploy web apps to Vercel. Use when the user wants to deploy a frontend project or update deployment settings.
```

**Bad examples:**
```yaml
description: JSON validator  # Too vague - when should it trigger?
description: This skill helps you validate your JSON files by running syntax checks and schema validation.  # Missing trigger context
```

## Optional Fields

### metadata (OpenClaw-specific)

Single-line JSON object with OpenClaw features.

```yaml
metadata: { "openclaw": { "emoji": "🔧", "requires": { "bins": ["python3"] } } }
```

**Important**: Must be on a single line. Multi-line JSON will cause parsing errors.

### metadata.openclaw fields

| Field | Type | Description |
|-------|------|-------------|
| `emoji` | string | Display emoji in macOS Skills UI |
| `homepage` | string | URL shown in Skills UI |
| `os` | array | Platform filter: `["darwin", "linux", "win32"]` |
| `always` | boolean | Skip all gating checks |
| `primaryEnv` | string | Env var for API key injection |
| `requires.bins` | array | Required binaries in PATH |
| `requires.env` | array | Required environment variables |
| `requires.config` | array | Required config paths (e.g., `browser.enabled`) |
| `requires.anyBins` | array | At least one must exist |
| `install` | array | Installer specs for macOS UI |

## Non-Standard Fields (Avoid)

These are NOT part of the AgentSkills spec:

```yaml
# ❌ Don't use these
version: 1.0.0
author: Your Name
created: 2024-01-01
```

If you need metadata, put it in `metadata`:

```yaml
# ✅ Use this instead
metadata: { "openclaw": { "version": "1.0.0" } }
```

## Examples

### Minimal Skill

```yaml
---
name: hello-world
description: Say hello in different languages. Use when the user asks for a greeting.
---
```

### Skill with Dependencies

```yaml
---
name: pdf-processor
description: Process PDF files - rotate, merge, split, extract text. Use when working with .pdf files.
metadata: { "openclaw": { "emoji": "📄", "requires": { "bins": ["pdftk", "python3"] } } }
---
```

### Platform-Specific Skill

```yaml
---
name: macos-notifier
description: Send macOS notifications. Use when the user wants desktop alerts on Mac.
metadata: { "openclaw": { "os": ["darwin"], "requires": { "bins": ["terminal-notifier"] } } }
---
```

### Skill with Config Dependency

```yaml
---
name: browser-automation
description: Automate browser tasks. Use when the user wants web scraping, form filling, or UI testing.
metadata: { "openclaw": { "requires": { "config": ["browser.enabled"] } } }
---
```
