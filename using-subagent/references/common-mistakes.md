# Common Mistakes with Subagent

## ❌ Critical Error: Wrong File Paths

### Problem

Giving reference paths instead of target paths:

```javascript
// ❌ WRONG - Agent will check wrong file!
task: "Update your workspace. Reference: ~/openclaw-workspaces/main/MEMORY.md lines 70-85"
```

Agent checks `~/openclaw-workspaces/main/MEMORY.md`, sees content already there, thinks "done" but didn't touch its own file.

### Solution

Always specify **target file paths explicitly**:

```javascript
// ✅ CORRECT - Clear target + reference
task: `Update YOUR workspace:
- Target: ~/openclaw-workspaces/technical-support/MEMORY.md
- Reference: ~/openclaw-workspaces/main/MEMORY.md lines 70-85`
```

### Rule

| Type | Path | Purpose |
|------|------|---------|
| **Target** | Agent's own workspace | Files to modify |
| **Reference** | Your workspace or docs | Examples to follow |

**Always distinguish "target" vs "reference" in task message.**

---

## ❌ Error: Task Too Long

### Problem

Sending 500+ character task descriptions:

```javascript
// ❌ BAD - Too long, agent spends time parsing
task: "Update workspace:\n1. Do X with these details...\n2. Do Y with these rules...\n3. Do Z with these exceptions..." // 600+ chars
```

**Result**: Agent takes longer to process, may timeout.

### Solution

Keep task short, let agent read files:

```javascript
// ✅ GOOD - Short + file reference
task: "Read ~/handbook/task-rules.md and execute"

// Or even shorter
task: "Execute memory扁平化，见 handbook/message.md"
```

### Rule

- Task message < 100 characters
- Detailed instructions in files
- Agent reads files, not long messages

---

## ❌ Error: Timeout Too Short

### Problem

```javascript
// ❌ BAD - Complex task with 2min timeout
sessions_spawn({
  task: "Analyze entire codebase and write report",
  timeoutSeconds: 120  // Too short!
})
```

**Result**: Task times out, agent marked as "failed" but may have done the work.

### Solution

Match timeout to task complexity:

| Task Type | Timeout | Example |
|-----------|---------|---------|
| Simple (read, execute) | 120s (2min) | Read file, run command |
| Medium (analyze, update) | 300s (5min) | Update multiple files |
| Complex (research, create) | 600s (10min) | Create report, implement feature |
| No limit | 0 | Long-running tasks |

```javascript
// ✅ GOOD - Complex task with 10min timeout
sessions_spawn({
  task: "Analyze codebase",
  thinking: "high",
  timeoutSeconds: 600  // 10 minutes
})
```

---

## ❌ Error: Wrong Thinking Level

### Problem

Using high thinking for simple tasks:

```javascript
// ❌ WASTEFUL - Simple task with high thinking
sessions_spawn({
  task: "List files in directory",
  thinking: "high"  // Overkill!
})
```

**Result**: Slower execution, wasted compute.

### Solution

Match thinking to task complexity:

```javascript
// ✅ GOOD - Simple task, low thinking
sessions_spawn({
  task: "List files",
  thinking: "low",  // Fast
  timeoutSeconds: 60
})

// ✅ GOOD - Complex task, high thinking
sessions_spawn({
  task: "Research and recommend architecture",
  thinking: "high",  // Thorough analysis
  timeoutSeconds: 600
})
```

---

## ❌ Error: Wrong Model for Task

### Problem

Using general model for specialized task:

```javascript
// ❌ SUBOPTIMAL - Tool-heavy task with general model
sessions_spawn({
  task: "Configure MCP servers",
  model: "GLM"  // Not optimized for tool calling
})
```

### Solution

Match model to task type:

| Task Type | Recommended Model | Reason |
|-----------|------------------|--------|
| General | `GLM` (default) | Balanced |
| Tool calling | `MiniMax` | Optimized for tools |
| Long context | `Qwen` | 1M context window |
| Fast/simple | `GLM-Flash` | Quick responses |
| Complex analysis | `Qwen-Max` | Advanced reasoning |
| Coding | `Kimi` | Code-focused |

```javascript
// ✅ GOOD - Tool task with MiniMax
sessions_spawn({
  task: "Configure MCP",
  model: "MiniMax"  // Best for tool calling
})
```

---

## Checklist Before Spawning

Before calling `sessions_spawn`, verify:

- [ ] **Target path** is specified (agent's own workspace)
- [ ] **Task message** is < 100 characters
- [ ] **Timeout** matches task complexity
- [ ] **Thinking level** matches task complexity
- [ ] **Model** matches task type (if overriding)

---

_Common mistakes when delegating to subagents_
