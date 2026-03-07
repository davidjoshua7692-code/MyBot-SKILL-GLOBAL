# Task Templates

Templates for creating task briefs and handbook files.

## Task Brief

**Location**: `memory/task/brief-{topic}.md`

**When to use**:
- Task description > 100 characters
- Complex multi-step task
- Need to preserve task for future reference

### Template

```markdown
# Task: [Brief Topic]

## Target
- Agent: [technical-support | image-generator]
- Workspace: ~/openclaw-workspaces/{agent}/
- Files to modify:
  - FILE_1
  - FILE_2

## Requirements
1. [Requirement 1]
2. [Requirement 2]
3. [Requirement 3]

## Reference
- Example: ~/openclaw-workspaces/main/FILE.md
- Docs: [URL or path]

## Expected Output
- [What agent should produce]
- [Where to save it]

## Notes
- [Special considerations]
- [Potential issues]
```

### Example

```markdown
# Task: Memory Flatten

## Target
- Agent: technical-support
- Workspace: ~/openclaw-workspaces/technical-support/
- Files to modify:
  - memory/ (entire directory)

## Requirements
1. Move all files from subdirectories to memory/ root
2. Rename files to YYYY-MM-DD-type-topic.md format
3. Delete empty subdirectories
4. Update MEMORY.md with new structure

## Reference
- Example: ~/openclaw-workspaces/main/memory/
- Naming rules: ~/openclaw-workspaces/main/MEMORY.md#索引

## Expected Output
- Flat memory/ directory
- Updated MEMORY.md
- No subdirectories (except handbook/)

## Notes
- Keep handbook/ subdirectory
- Don't modify skill-audit/ if exists
```

---

## Handbook

**Location**: `handbook/{topic}.md`

**When to use**:
- Reusable rules/patterns
- Standardization requirements
- Onboarding instructions for agents

### Template

```markdown
# [Topic] Handbook

## Purpose
[Why this handbook exists and when to use it]

## Rules
1. [Rule 1]
2. [Rule 2]
3. [Rule 3]

## File Naming
- Pattern: `YYYY-MM-DD-type-topic.md`
- Allowed types: [list]

## Examples

### Example 1: [Scenario]
```markdown
[Example content]
```

### Example 2: [Scenario]
```markdown
[Example content]
```

## Checklist
- [ ] Item 1
- [ ] Item 2
- [ ] Item 3

## Notes
- [Special considerations]
```

### Example

```markdown
# Memory Rules Handbook

## Purpose
Standardize memory file naming and structure across all agents.

## Rules
1. All files use YYYY-MM-DD-type-topic.md format
2. Allowed types: report, research, insight, info, system, tip-*
3. No random naming
4. System files renamed to system-* by Heartbeat

## File Naming
- Pattern: `YYYY-MM-DD-type-topic.md`
- Allowed types:
  - report (cron reports)
  - research (research notes)
  - insight (deep insights)
  - info (config/flow info)
  - system (auto-generated)
  - tip-openclaw (OpenClaw tips)
  - tip-obsidian (Obsidian tips)
  - tip-mcp (MCP tips)

## Examples

### Example 1: Cron Report
```markdown
# 2026-03-07 Noon Report

## Progress
- Completed X
- Started Y

## Issues
- Issue with Z

## Next Steps
- Continue Y
```

### Example 2: Research Note
```markdown
# 2026-03-07 Research - ClawHub Skills

## Summary
[Research summary]

## Key Findings
1. Finding 1
2. Finding 2

## References
- [Link 1]
- [Link 2]
```

## Checklist
- [ ] File name follows YYYY-MM-DD-type-topic.md
- [ ] Type is from allowed list
- [ ] Content has clear structure
- [ ] Timestamp included

## Notes
- Heartbeat auto-renames system files
- Daily files use YYYY-MM-DD-daily.md
```

---

## Quick Reference

| File Type | Location | Purpose |
|-----------|----------|---------|
| Task brief | `memory/task/brief-{topic}.md` | One-time complex task |
| Handbook | `handbook/{topic}.md` | Reusable rules/patterns |

## How to Use

### 1. Create Task Brief

```bash
# Create task brief
cat > memory/task/brief-flatten.md << 'EOF'
[Task brief content]
EOF
```

### 2. Call Agent

```javascript
sessions_spawn({
  mode: "run",
  label: "technical-support",
  task: "Read memory/task/brief-flatten.md and execute",
  timeoutSeconds: 300
})
```

### 3. Cleanup (Optional)

```bash
# Remove task brief after completion
rm memory/task/brief-flatten.md
```

---

_Templates for task delegation_
