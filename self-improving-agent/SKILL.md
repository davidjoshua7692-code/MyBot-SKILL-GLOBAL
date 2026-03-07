---
name: self-improving-agent
description: Captures learnings, errors, and corrections to enable continuous improvement. Use when: (1) A command or operation fails unexpectedly, (2) User corrects Agent, (3) User requests a capability that doesn't exist, (4) An external API or tool fails, (5) Agent realizes its knowledge is outdated or incorrect, (6) A better approach is discovered for a recurring task.
---

# Self-Improving Agent

> 让 Agent 越用越聪明

---

## 🔔 核心机制

1. **自动记录学习内容** → `memory/YYYY-MM-DD-{TYPE}.md`
2. **判断是否需要晋升到永久文件** → MEMORY.md、AGENTS.md、SOUL.md、TOOLS.md
3. **向用户显性反馈使用记录** → "我刚学到了一个新东西，已经记录下来了！"

---

## 📂 目录结构

```
~/openclaw-workspaces/[workspace]/memory/
├── 2026-03-07-LEARNING.md        # 当天的学习记录
├── 2026-03-07-ERROR.md           # 当天的错误记录
├── 2026-03-07-FEATURE_REQUESTS.md # 当天的功能请求
├── 2026-03-06-LEARNING.md        # 昨天的学习记录
├── 2026-03-06-ERROR.md           # 昨天的错误记录
└── ...
```

**命名规则**：
- `YYYY-MM-DD-LEARNING.md` - 学习记录（纠正/知识缺口/最佳实践）
- `YYYY-MM-DD-ERROR.md` - 错误记录（命令失败/异常）
- `YYYY-MM-DD-FEATURE_REQUESTS.md` - 功能请求（期望实现的功能/需求）

**写入规则**：
- 当天追加到当天的文件
- 不同天创建新文件
- 文件名自带时间戳，无需在内容里重复

---

## 🎯 触发场景

| 场景 | 记录到 | 例子 |
|------|--------|------|
| 命令/操作失败 | `YYYY-MM-DD-ERROR.md` | 执行 bash 命令报错 |
| 用户纠正 Agent | `YYYY-MM-DD-LEARNING.md` | 丹哥说"不对，应该这样" |
| 功能缺失/无法实现 | `YYYY-MM-DD-FEATURE_REQUESTS.md` | Agent 发现自己"做不到" |
| API 调用失败 | `YYYY-MM-DD-ERROR.md` | 第三方服务返回异常 |
| 知识过时 | `YYYY-MM-DD-LEARNING.md` | 某个 API 已废弃 |
| 发现更好的做法 | `YYYY-MM-DD-LEARNING.md` | 找到更优解 |
| 发现新的工具配置 | `YYYY-MM-DD-LEARNING.md` | 发现了新的 Skill/MCP 配置 |

---

## 📤 晋升规则

**重要的、长期的** → 晋升到永久文件

| 学习类型 | 晋升目标 | 例子 |
|----------|----------|------|
| 行为模式 | SOUL.md | "以后要用比喻的方式说明技术方案" |
| 工作流改进/规则 | AGENTS.md | "每次 Every Session 先读 SOUL.md" |
| 工具知识 | TOOLS.md | "你可以使用 context7 搜索文档" |
| 架构理解 | MEMORY.md | "这是 OpenClaw 架构图" |
| 用户信息 | USER.md | "用户说长期目标是打造个人IP" |

**晋升信号**（关键词）：
- 重要性：**"这个很重要!"**、**"记下来！"**、**"这个常用到！"**
- 性质：**"原则性的"**、**"核心的"**、**"架构性的"**
- 频率：**"必须遵守"**、**"这是规则"**

---

## 📝 记录格式

### LEARNING 格式

追加到 `memory/YYYY-MM-DD-LEARNING.md`：

```markdown
## [LRN-XXX] 类别

**Priority**: low/medium/high
**Area**: config/troubleshooting/best-practices/etc

### 发生了什么
简短描述

### 学到了什么
正确的做法

---
```

### ERROR 格式

追加到 `memory/YYYY-MM-DD-ERROR.md`：

```markdown
## [ERR-XXX] 命令/操作名

**Priority**: high

### 错误信息
```
实际报错
```

### 原因 & 解决
为什么会错，怎么修

---
```

### FEATURE_REQUESTS 格式

追加到 `memory/YYYY-MM-DD-FEATURE_REQUESTS.md`：

```markdown
## [FEAT-XXX] 功能名

**Priority**: medium

### 期望的功能
用户想要什么

### 为什么需要
解决什么问题

---
```

---

## 📊 最佳实践

1. **立即记录** - 发生后马上记录，上下文最清晰
2. **简洁明了** - 文件名已有日期，内容只写核心
3. **当天追加** - 同一天的多条记录追加到同一个文件
4. **谨慎晋升** - 不确定是否晋升时，callback给用户确认或者先写到学习记录里，后续评估

---

## 📋 ID 格式

`TYPE-XXX`

- TYPE: `LRN` (learning)、`ERR` (error)、`FEAT` (feature)
- XXX: 序号或简短标识

例子：`LRN-001`、`ERR-sed-syntax`、`FEAT-obsidian-cli`

**注意**：文件名已有日期（如 `2026-03-07-ERROR.md`），ID 不需要重复日期。

---

_Self-Improving Agent - 让每个 Agent 都能自我进化_
