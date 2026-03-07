---
name: using-subagent
description: Spawn and manage subagents for parallel/distributed tasks. Use when needing to delegate work to other agents (technical-support, image-generator) or run isolated tasks. Covers sessions_spawn, model selection, thinking levels.
---

# Using Subagent

Delegate tasks to other agents via `sessions_spawn`.

## Quick Reference

```javascript
sessions_spawn({
  mode: "run",           // "run" (one-shot) or "session" (persistent)
  task: "Task content",  // Short instruction, let agent read files
  label: "agent-name",   // "technical-support", "image-generator"
  timeoutSeconds: 300,   // 5 minutes recommended
  model: "MiniMax",      // Optional: override model
  thinking: "low"        // Optional: "low" | "medium" | "high"
})
```

## Methods Overview

| Method | Purpose | Config Required |
|--------|---------|-----------------|
| `sessions_spawn` | Start subagent task | No |
| `sessions_send` | Send to existing session | `visibility=all` |
| `subagents` | List/steer/kill child agents | No |
| `sessions_list` | View active sessions | No |

**Recommended**: Use `sessions_spawn` mode: "run" - no config needed.

## Model Selection

| Task Type | Model Alias | Model ID (optional) |
|-----------|-------------|-------------------|
| Tool calling, config | `MiniMax` | `bailian/MiniMax-M2.5` |
| Complex analysis | `Qwen-Max` | `bailian/qwen3-max-2026-01-23` |
| Simple tasks | `GLM-Flash` | `zai/glm-4.7-flash` |
| Long context | `Qwen` | `bailian/qwen3.5-plus` |
| General purpose | `GLM` | `zai/glm-5` |
| Coding | `Kimi` | `kimi-coding/k2p5` |

**推荐用 alias**（更简洁），完整 model ID 也可用（更明确）。

## Thinking Levels

| Level | Use For |
|-------|---------|
| `"low"` | Simple tasks (read file, execute command) |
| `"medium"` | Normal tasks (default, inherits from caller) |
| `"high"` | Complex analysis, research, decisions |

**Default**: Inherits caller's thinking level.

## Task Delegation Flow

### When to Use Handbook/Task Files

| Situation | Action | Path |
|-----------|--------|------|
| Task requires standardization | Create handbook template | `handbook/模板名.md` |
| Task description > 100 chars | Create task brief | `memory/task/brief-topic.md` |
| One-time task | Use direct task message | - |

### Task Brief Template

```markdown
# Task: [Topic]

## Target
- Target workspace: ~/openclaw-workspaces/{agent}/
- Files to modify: [list]

## Requirements
1. [Requirement 1]
2. [Requirement 2]

## Reference
- Example: ~/openclaw-workspaces/main/FILE.md
- Docs: [URL or path]

## Expected Output
- [What agent should produce]
```

### Handbook Template

```markdown
# [Topic] Handbook

## Purpose
[Why this handbook exists]

## Rules
1. [Rule 1]
2. [Rule 2]

## Examples
[Examples]

## Checklist
- [ ] Item 1
- [ ] Item 2
```

### How to Call

```javascript
// ✅ GOOD - Task brief
sessions_spawn({
  task: "Read memory/task/brief-flatten.md and execute",
  timeoutSeconds: 300
})

// ✅ GOOD - Handbook reference
sessions_spawn({
  task: "Follow handbook/memory-rules.md to update your workspace",
  timeoutSeconds: 300
})
```

## Best Practices

### ✅ DO

```
✅ task: "Read memory/task/brief-xxx.md and execute"
✅ Create task brief for complex tasks
✅ Create handbook for reusable rules
✅ Specify target workspace path explicitly
```

### ❌ DON'T

```
❌ task: "完整说明（500字）..." + timeout: 120
❌ Only give reference path without specifying target
❌ Repeat long instructions in task message
```

## Common Mistakes

| Mistake Type | Line | Severity |
|--------------|------|----------|
| Wrong File Paths (Target vs Reference) | 3 | ❌ Critical |
| Task Too Long (>100 chars) | 35 | ❌ Error |
| Timeout Too Short | 59 | ❌ Error |
| Wrong Thinking Level | 93 | ❌ Error |
| Wrong Model for Task | 121 | ❌ Error |

详细内容：[common-mistakes.md](references/common-mistakes.md)

## Task Templates

详细内容：[task-templates.md](references/task-templates.md)

## Examples

### Basic Task

```javascript
sessions_spawn({
  mode: "run",
  label: "technical-support",
  task: "Run npm install in ~/my-project",
  timeoutSeconds: 120
})
```

### Complex Task (with model override)

```javascript
sessions_spawn({
  mode: "run",
  label: "technical-support",
  task: "Read ~/docs/api.md and implement the integration",
  model: "MiniMax",
  thinking: "high",
  timeoutSeconds: 600
})
```

### Simple Task (fast model)

```javascript
sessions_spawn({
  mode: "run",
  label: "image-generator",
  task: "Generate a logo with Flux",
  model: "GLM-Flash",
  thinking: "low",
  timeoutSeconds: 120
})
```

---

_Best practices for subagent delegation_
