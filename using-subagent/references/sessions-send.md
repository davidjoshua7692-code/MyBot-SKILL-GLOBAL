# sessions_send Reference

Send messages to other agent sessions. Requires `tools.agentToAgent.enabled: true`.

## Quick Start

```javascript
sessions_send({
  sessionKey: "agent:technical-support:telegram:direct:7772073074",
  message: "Task description",
  timeoutSeconds: 60
})
```

## Parameters

| Parameter | Required | Default | Description |
|-----------|----------|---------|-------------|
| `sessionKey` | Yes | - | Target session key |
| `message` | Yes | - | Message content |
| `timeoutSeconds` | No | 0 | 0 = fire-and-forget, >0 = wait for reply |

## Return Values

| Status | Meaning |
|--------|---------|
| `ok` | Success, got reply |
| `timeout` | Timeout, task continues in background |
| `forbidden` | Agent-to-agent disabled |
| `accepted` | Fire-and-forget accepted |

## Announce Mode (Critical)

**Rule**: Target agent's reply is announced to requester's channel based on reply content.

| Target Reply | Result |
|--------------|--------|
| `ANNOUNCE_SKIP` | Silent (no message to Telegram) |
| Other content | **Send to Telegram** ✅ |

### Control via Message

```javascript
// ❌ Silent mode (default)
sessions_send({
  message: "Check system status"
})

// ✅ Announce to Telegram
sessions_send({
  message: "Check system status. Reply to Telegram (do NOT use ANNOUNCE_SKIP)"
})
```

## Session Key Format

```
agent:<agentId>:<channel>:<type>:<id>

Examples:
- agent:main:telegram:direct:7772073074
- agent:technical-support:telegram:direct:7772073074
- agent:image-generator:telegram:direct:7772073074
```

## Best Practices

### ✅ DO

```javascript
// ✅ Use sessions_list to find sessionKey
const sessions = sessions_list({ activeMinutes: 60 })

// ✅ Set timeout for expected response
sessions_send({
  message: "Check status",
  timeoutSeconds: 60  // Wait for reply
})

// ✅ Request Telegram announce when needed
sessions_send({
  message: "Check status. Reply to Telegram (do NOT use ANNOUNCE_SKIP)"
})
```

### ❌ DON'T

```javascript
// ❌ Guess sessionKey
sessions_send({ sessionKey: "agent:tech:telegram:direct:123" })  // Wrong!

// ❌ Ask questions in dashboard (user can't see)
// If you need user input, send to Telegram via message tool

// ❌ Forget timeout for long tasks
sessions_send({
  message: "Analyze entire codebase",
  timeoutSeconds: 30  // Too short!
})
```

## Common Patterns

### Pattern 1: Quick Task with Result

```javascript
const result = sessions_send({
  sessionKey: "agent:technical-support:telegram:direct:7772073074",
  message: "Check gateway status. Reply result to Telegram.",
  timeoutSeconds: 60
})

// Result will be announced to user's Telegram automatically
```

### Pattern 2: Fire-and-Forget

```javascript
sessions_send({
  sessionKey: "agent:image-generator:telegram:direct:7772073074",
  message: "Generate image: cute cat",
  timeoutSeconds: 0  // Don't wait
})

// Use sessions_history later to check result
```

### Pattern 3: With Follow-up

```javascript
// 1. Send task
sessions_send({
  sessionKey: targetSession,
  message: "Analyze logs",
  timeoutSeconds: 120
})

// 2. Check result later via sessions_history
sessions_history({
  sessionKey: targetSession,
  limit: 5
})

// 3. Send summary to user via message tool
message({
  action: "send",
  target: "7772073074",
  message: "✅ Task complete: [summary]"
})
```

## Debugging

### Check if agent is online

```javascript
sessions_list({ activeMinutes: 5 })
// If session not in list, agent may be offline
```

### Check delivery status

```javascript
// sessions_send returns:
{
  "status": "ok",
  "delivery": { "status": "pending" | "ok" }
}
```

### View conversation history

```javascript
sessions_history({
  sessionKey: "agent:technical-support:telegram:direct:7772073074",
  limit: 10,
  includeTools: true
})
```

## Configuration Required

```json5
{
  "tools": {
    "sessions": {
      "visibility": "all"  // or "tree" | "agent"
    },
    "agentToAgent": {
      "enabled": true  // Required for cross-agent messaging
    }
  }
}
```

## Related

- [common-mistakes.md](common-mistakes.md) - Common errors with sessions_send
- [task-templates.md](task-templates.md) - Task brief templates
