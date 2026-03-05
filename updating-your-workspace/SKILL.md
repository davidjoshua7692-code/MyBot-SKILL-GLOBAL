---
name: updating-your-workspace
description: Update and organize workspace memory files. Use when user says "update workspace", "organize memory", "整理记忆", "更新工作区", or on scheduled cron. Extracts valuable info from memory/ logs into MEMORY.md, AGENTS.md, or knowledge skills.
---

# Updating Your Workspace

从 memory/ 日志中提炼有价值信息，更新到正确的文件。

## 核心原则

1. **增量处理** - 只处理未读过的 memory 文件，避免重复更新
2. **分类准确** - 根据内容类型写入正确的目标文件
3. **进度追踪** - 记录已处理的文件，下次从断点继续
4. **结果汇报** - 完成后清晰汇报做了什么

## 工作流程

```
触发更新
    ↓
1. 读取 memory/.update-log
   - 存在 → 获取 processedFiles 列表
   - 不存在 → 初始化空列表
    ↓
2. 扫描 memory/*.md 文件
   - 排除已处理的
   - 剩下的 = 待处理
    ↓
3. 对每个待处理文件：
   a. 读取内容
   b. 分析有价值信息
   c. 分类判断（见下方）
   d. 写入目标文件
    ↓
4. 更新 memory/.update-log
   - 记录本次处理的文件
   - 记录更新内容
   - 更新 lastRun 时间
    ↓
5. 回复结果
```

## 内容分类规则

| 内容类型 | 目标文件 | 判断依据 |
|----------|----------|----------|
| 身份定义、战略思维、核心原则、架构理解、memory/索引入口 | MEMORY.md | 长期不变的原则性内容 |
| 工作流程、Session 规则、Safety | AGENTS.md | 每次会话都要遵循的规则 |
| 具体命令、配置参数、排错步骤 | skills/xxx/ | 技术细节、可复用的知识 |
| 临时记录、日常日志 | 留在 memory/ | 不需要升级的信息 |

## 你的 Agent 指南

根据你的 workspace 类型，阅读对应的指南：

阿技 - **technical-support** → [tech-support-guide.md](references/tech-support-guide.md)
小丹 - **main** → [main-guide.md](references/main-guide.md)
图图 - **image-generator** → [image-generator-guide.md](references/image-generator-guide.md)
- **其他 agent** → 联系丹哥创建你的指南文件

> ⚠️ 必须先阅读你的指南，了解具体的内容定位规则！

## 进度文件格式

进度存储在 `memory/.update-log`，格式见 [progress-format.md](references/progress-format.md)

## 回复格式

### 有更新时

```
✅ Workspace 更新完成

📝 处理文件：
- memory/2026-03-05.md

📂 更新内容：
- MEMORY.md
  - 添加：xxx 原则
- skills/technical-supporting/configuration/
  - 新增：cron-setup.md
```

### 无更新时

```
✅ Workspace 检查完成，无需更新

📝 检查文件：
- memory/2026-03-05.md

💡 未发现需要升级的信息
```

### 首次运行

```
✅ Workspace 初始化完成

📝 创建进度文件：memory/.update-log
📝 已处理文件：memory/2026-03-05.md
```

## 与 Cron 搭配

此 skill 可与 OpenClaw cron 配合定时运行：

```json
{
  "name": "workspace-update",
  "schedule": { "kind": "cron", "expr": "0 */4 * * *" },
  "payload": { "kind": "agentTurn", "message": "更新 workspace" },
  "sessionTarget": "isolated"
}
```

建议间隔：4-6 小时，避免频繁触发。

## 注意事项

- **不要删除** memory/ 下的原始日志
- **不要重复处理** 已在 .update-log 中的文件
- **写入前确认** 目标文件是否需要备份
- **保持精简** 只添加真正有价值的信息

---

## Daniel's Note

MEMORY.md 记忆的北极星 ——  这里存放着你们对工作的深层思考。是对未来的指引，不是速记碎片，而是关于"为什么做"和"往哪去"的指南针。

AGENT.md 协作契约的石碑 ——  这是我们共事的约定。你的工作原则、与我互动的方式，写在这里。

USER.md 不变的人设锚点 —— 这是我告诉你的关于我是谁。只有当我主动说"我想更新一下自我定义"时，这里才会改变。

SKILLS 书房里的参考书 —— 它们是你们的知识库，像是书架上那些不常翻看、但需要时知道在哪里的专业书籍。特定领域的知识，待命而非常用。

memory/ 记忆笔记 —— 每次对话后留下的痕迹，这些笔记是更新整个工作空间的养料。

---

_让 workspace 记忆井井有条_
