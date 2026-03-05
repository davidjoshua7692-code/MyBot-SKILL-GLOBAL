# 进度文件格式

`memory/.update-log` 的格式说明。

## 文件位置

```
<workspace>/memory/.update-log
```

例如：
- technical-support: `~/openclaw-workspaces/technical-support/memory/.update-log`
- main: `~/openclaw-workspaces/main/memory/.update-log`

## 格式

JSON 格式，单行（便于解析）：

```json
{
  "lastRun": "2026-03-05T14:30:00+08:00",
  "processedFiles": ["2026-03-04.md", "2026-03-05.md"],
  "updates": [
    {
      "date": "2026-03-05T14:30:00+08:00",
      "source": "2026-03-05.md",
      "changes": [
        { "target": "MEMORY.md", "action": "added", "section": "新架构理解" },
        { "target": "skills/technical-supporting/configuration/", "action": "created", "file": "cron-advanced.md" }
      ]
    }
  ]
}
```

## 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| `lastRun` | string (ISO 8601) | 上次运行时间 |
| `processedFiles` | array | 已处理的 memory 文件名列表 |
| `updates` | array | 每次运行的更新记录 |

### updates[].changes[] 字段

| 字段 | 值 | 说明 |
|------|-----|------|
| `target` | 文件路径 | 更新的目标文件 |
| `action` | `added` / `updated` / `created` / `removed` | 操作类型 |
| `section` | string | 更新的章节（可选） |
| `file` | string | 创建的文件名（可选） |

## 操作流程

### 读取进度

1. 检查 `memory/.update-log` 是否存在
   - 不存在 → 初始化新对象
   - 存在 → 解析 JSON

2. 获取 `processedFiles` 列表

### 处理文件

3. 扫描 `memory/*.md`
4. 排除 `processedFiles` 中的文件
5. 处理剩余文件

### 更新进度

6. 将新处理的文件加入 `processedFiles`
7. 将本次更新记录加入 `updates`
8. 更新 `lastRun`
9. 写入文件（JSON 单行）

## 初始值

首次运行时创建：

```json
{"lastRun": null, "processedFiles": [], "updates": []}
```

## 注意事项

- **单行 JSON**：避免多行格式，保持紧凑
- **追加不覆盖**：`processedFiles` 只追加，不删除
- **时区**：使用 local time + timezone（如 `+08:00`）
- **排序**：`processedFiles` 按处理顺序，不必按日期排序

## 示例：完整运行周期

### 初始状态

文件不存在。

### 第一次运行

处理 `memory/2026-03-05.md`，发现需要更新 `MEMORY.md`。

写入：
```json
{"lastRun":"2026-03-05T10:00:00+08:00","processedFiles":["2026-03-05.md"],"updates":[{"date":"2026-03-05T10:00:00+08:00","source":"2026-03-05.md","changes":[{"target":"MEMORY.md","action":"added","section":"新原则"}]}]}
```

### 第二次运行（同一天）

检查 `processedFiles`，发现 `2026-03-05.md` 已处理。
扫描发现 `memory/2026-03-06.md`（新文件）。

处理 `2026-03-06.md`，发现无需更新。

写入：
```json
{"lastRun":"2026-03-06T14:00:00+08:00","processedFiles":["2026-03-05.md","2026-03-06.md"],"updates":[{"date":"2026-03-05T10:00:00+08:00","source":"2026-03-05.md","changes":[{"target":"MEMORY.md","action":"added","section":"新原则"}]},{"date":"2026-03-06T14:00:00+08:00","source":"2026-03-06.md","changes":[]}]}
```

---

_进度追踪文件格式规范_
