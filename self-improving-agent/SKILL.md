---
name: self-improving-agent
description: Captures learnings, errors, and corrections to enable continuous improvement. Use when: (1) A command or operation fails unexpectedly, (2) User corrects Agent, (3) User requests a capability that doesn't exist, (4) An external API or tool fails, (5) Agent realizes its knowledge is outdated or incorrect, (6) A better approach is discovered for a recurring task.
---

# Self-Improving Agent

> 让 Agent 越用越聪明

---

## 🔔 核心机制

你要必须自动：
1. **记录学习内容** → `.learnings/` 目录
2. **晋升到永久文件** → AGENTS.md、SOUL.md、TOOLS.md

---

## 📂 目录结构

每个 workspace 都有：
```
~/openclaw-workspaces/[workspace]/
└── .learnings/
    ├── LEARNINGS.md        # 学习记录（纠正/知识缺口/最佳实践）
    ├── ERRORS.md           # 错误记录（命令失败/异常）
    └── FEATURE_REQUESTS.md # 功能请求 （期望实现的功能/需求）
```

---

## 🎯 触发场景

| 场景 | 记录到 | 例子 |
|------|--------|------|
| 命令/操作失败 | ERRORS.md | 执行 bash 命令报错 |
| 用户纠正 Agent | LEARNINGS.md | Daniel说"不对，应该这样" |
| 功能缺失/无法实现 | FEATURE_REQUESTS.md | Agent 发现自己"做不到" |
| API 调用失败 | ERRORS.md | 第三方服务返回异常 |
| 知识过时 | LEARNINGS.md | 某个 API 已废弃 |
| 发现更好的做法 | LEARNINGS.md | 找到更优解 |
| 发现新的工具配置 | TOOL.md | 发现了新的 Skill/MCP 配置 |

---

## 📤 晋升规则

**临时记录 → 永久文件**

| 学习类型 | 晋升目标 | 例子 |
|----------|----------|------|
| 行为模式 | SOUL.md | "用比喻的方式说明技术方案" |
| 工作流改进/规则 | AGENTS.md | "Every Session 先读 SOUL.md" |
| 工具知识 | TOOLS.md | "context7 用于搜索文档" |
| 架构理解 | MEMORY.md | "OpenClaw 架构图" |

---

## 📝 记录格式

### LEARNINGS.md

```markdown
## [LRN-YYYYMMDD-XXX] 类别

**Logged**: 时间戳
**Priority**: low/medium/high
**Status**: pending
**Area**: config/troubleshooting/best-practices

### Summary
一句话描述学到了什么

### Details
详细说明：发生了什么，哪里错了，正确的做法

### Suggested Action
具体的改进建议

---
```

### ERRORS.md

```markdown
## [ERR-YYYYMMDD-XXX] 命令名

**Logged**: 时间戳
**Priority**: high
**Status**: pending

### Summary
简要描述失败原因

### Error
```
实际错误信息
```

### Context
- 尝试的操作
- 输入参数
- 环境细节

### Suggested Fix
如何解决

---
```

### FEATURE_REQUESTS.md

```markdown
## [FEAT-YYYYMMDD-XXX] 功能名

**Logged**: 时间戳
**Priority**: medium
**Status**: pending

### Requested Capability
用户想要什么功能

### User Context
为什么需要，解决什么问题

---
```

---

## ✅ 状态值

| 状态 | 说明 |
|------|------|
| pending | 待处理 |
| in_progress | 处理中 |
| resolved | 已解决 |
| promoted | 已晋升到永久文件 |
| wont_fix | 不修复（说明原因） |

---

## 🔄 优先级

| 优先级 | 使用场景 |
|--------|----------|
| critical | 阻塞核心功能、数据丢失风险、安全问题 |
| high | 影响大、影响常见工作流、反复出现 |
| medium | 影响中等、有变通方案 |
| low | 小问题、边缘情况、锦上添花 |

---

## 📊 最佳实践

1. **立即记录** - 发生后马上记录，上下文最清晰
2. **简洁明了** - 未来 Agent 需要快速理解
3. **包含复现步骤** - 特别是错误
4. **建议具体修复** - 不只是"调查"
5. **积极晋升** - 如果不确定是否晋升，就晋升
6. **定期回顾** - 在开始大任务前检查 `.learnings/`

---

## 🏷️ 区域标签

| Area | 范围 |
|------|------|
| config | 配置文件、环境设置 |
| troubleshooting | 故障排查 |
| best-practices | 最佳实践 |
| learning | 学习笔记 |
| frontend | UI、组件、客户端代码 |
| backend | API、服务、服务器代码 |
| infra | CI/CD、部署、Docker、云服务 |
| tests | 测试文件、测试工具 |

---

## 📋 ID 格式

`TYPE-YYYYMMDD-XXX`

- TYPE: `LRN` (learning)、`ERR` (error)、`FEAT` (feature)
- YYYYMMDD: 日期
- XXX: 序号或随机字符

例子：`LRN-20260306-001`、`ERR-20260306-A3F`

---

## 🔗 相关文件

- **OpenClaw workspace 结构**: `~/openclaw-workspaces/[workspace]/`
- **全局 skills**: `~/.openclaw/skills/`

---

_Self-Improving Agent - 让每个 Agent 都能自我进化_
