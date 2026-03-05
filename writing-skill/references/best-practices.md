# Best Practices for Writing Skills

How to write effective skill instructions.

## Core Principles

### 1. Concise is Key

The context window is shared. Every token costs.

**Default assumption**: The model is already smart. Only add what it doesn't know.

```
❌ Bad: This skill will help you validate JSON files by checking syntax...

✅ Good: Validate JSON syntax with python3 json.tool.
```

### 2. Description Contains Trigger Info

The `description` field determines when the skill loads. Put all "when to use" info there, not in the body.

```yaml
# ❌ Bad - body has trigger info
description: PDF processor
---
## When to Use
Use this skill when working with PDF files like invoices, reports...

# ✅ Good - description has trigger info
description: Process PDF files - rotate, merge, split. Use when working with .pdf files for invoices, reports, or documents.
```

### 3. Match Freedom to Task Fragility

| Task Type | Freedom Level | Approach |
|-----------|---------------|----------|
| Multiple valid approaches | High | Text instructions |
| Preferred pattern exists | Medium | Pseudocode + params |
| Must be exact sequence | Low | Specific script |

### 4. Progressive Disclosure

Don't dump everything in SKILL.md. Structure by need:

```
skill/
├── SKILL.md          # Core workflow (loaded on trigger)
└── references/
    ├── advanced.md   # Advanced features (loaded as needed)
    └── api.md        # API reference (loaded as needed)
```

Reference from SKILL.md:
```markdown
## Advanced Features

For form filling, see [FORMS.md](references/FORMS.md).
For API details, see [API.md](references/API.md).
```

## Writing the Body

### Use Imperative Form

```
❌ Bad: The skill will validate the file...
✅ Good: Validate the file with...
```

### Prefer Examples Over Explanations

```
❌ Bad: The rotate function allows you to change the orientation...
✅ Good: Rotate a PDF 90 degrees: pdftk input.pdf cat 1-endright output.pdf
```

### Avoid Redundancy

Don't repeat what's in description:
```markdown
# ❌ Bad
## When to Use
This skill is for validating JSON files when you edit configs...

# ✅ Good
(Start directly with usage)
```

## Organizing Larger Skills

### Keep SKILL.md Under 500 Lines

When approaching this limit, split content:
- Core workflow stays in SKILL.md
- Variant-specific details go to references/

### Pattern: Multi-Domain Skill

```
analytics-skill/
├── SKILL.md          # Overview + navigation
└── references/
    ├── finance.md    # Revenue, billing
    ├── sales.md      # Pipeline, opportunities
    └── product.md    # Usage, features
```

SKILL.md:
```markdown
# Analytics

Query company metrics.

## Domains

- **Finance**: Revenue, billing → see [finance.md](references/finance.md)
- **Sales**: Pipeline, opportunities → see [sales.md](references/sales.md)
- **Product**: Usage, features → see [product.md](references/product.md)
```

### Pattern: Multi-Framework Skill

```
deploy-skill/
├── SKILL.md          # Workflow + provider selection
└── references/
    ├── vercel.md     # Vercel patterns
    ├── netlify.md    # Netlify patterns
    └── aws.md        # AWS patterns
```

## Common Mistakes

### Creating Unnecessary Files

```
❌ Don't create:
skill/
├── SKILL.md
├── README.md           # Not needed
├── CHANGELOG.md        # Not needed
└── QUICK_START.md      # Not needed
```

Skills are for AI agents. Agents only read SKILL.md and referenced files.

### Deep Nesting

```
❌ Bad:
references/
└── docs/
    └── api/
        └── v2/
            └── endpoints.md  # Too deep!

✅ Good:
references/
└── api-v2.md  # One level deep
```

### Long Descriptions

```yaml
❌ Bad (>200 chars):
description: This comprehensive skill provides extensive functionality for processing and manipulating PDF documents including rotation merging splitting text extraction and more...

✅ Good (<150 chars):
description: Process PDFs - rotate, merge, split, extract text. Use when working with .pdf files for documents, reports, or invoices.
```

## Testing Your Skill

1. Create the skill
2. Restart Gateway or say "refresh skills"
3. Test with: "use the [skill-name] skill to..."
4. Iterate based on behavior
