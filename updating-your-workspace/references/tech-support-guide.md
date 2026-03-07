# Technical-Support Agent 指南

technical-support workspace 的内容定位规则。

## 文件定位

| 文件 | 定位 | 内容类型 | 变化频率 |
|------|------|----------|----------|
| **MEMORY.md** | 长期记忆 | 阿技业务的思维模式、工作原则、架构理解、核心知识 | 极少变 |
| **AGENTS.md** | 工作规则 + 流程 | Every Session、工单流程、Safety | 低 |
| **skills/technical-supporting/** | 技术细节 + 知识库 | 具体命令、配置参数、排错步骤、学习笔记 | 高 |

## 内容分流决策树

```
新知识出现
    ↓
1️⃣ 是原则性/身份相关的？
    ├─ 是 → MEMORY.md
    │      例："Gateway 是总开关"、"先备份再改配置"
    └─ 否 ↓

2️⃣ 是通用工具/流程？
    ├─ 是 → AGENTS.md
    │      例："Every Session 流程"、"工单结束流程"
    └─ 否 ↓

3️⃣ 是具体命令/配置/排错？
    ├─ 是 → skills/technical-supporting/
    │      ├─ 故障问题 → troubleshooting/
    │      ├─ 配置经验 → configuration/
    │      ├─ 最佳实践 → best-practices/
    │      └─ 学习记录 → learning/
    └─ 否 → 留在 memory/，不升级
```

## 具体例子

### 写入 MEMORY.md

```markdown
## 🏗️ OpenClaw 架构理解

用户 → Channel (Telegram 等) → Binding (路由) → Agent → Model (AI)
                                    ↓
                              Gateway (核心服务)
                                    ↓
                              Workspace (文件/记忆)
```

**特征**：原则性、长期有效、理解系统必需

### 写入 AGENTS.md

```markdown
## 📋 Every Session

Before doing anything else:
1. Read `SOUL.md` — this is who you are
2. Read `USER.md` — this is who you're helping
3. Read `memory/YYYY-MM-DD.md` (today + yesterday)
```

**特征**：每次会话都要执行、工作流程、规则

### 写入 skills/technical-supporting/

```markdown
# Cron 任务排查

## 症状
Cron 任务不执行

## 排查步骤
1. 检查 Gateway 状态：openclaw gateway status
2. 检查任务列表：openclaw cron list
3. 查看日志：cat /tmp/openclaw/cron.log
```

**特征**：具体命令、可复用、技术细节

## 知识库子目录

```
skills/technical-supporting/
├── SKILL.md              # 索引文件
├── configuration/        # 配置经验
│   ├── heartbeat-cron.md
│   └── model-override.md
├── troubleshooting/      # 故障排查
│   └── gateway-wont-start.md
├── best-practices/       # 最佳实践
│   └── backup-strategy.md
└── learning/             # 学习记录
    └── isolated-vs-main.md
```

### 选择子目录

| 内容 | 子目录 |
|------|--------|
| 如何配置某个功能 | configuration/ |
| 某个问题的排查步骤 | troubleshooting/ |
| 避免坑的经验总结 | best-practices/ |
| 新学到的概念理解 | learning/ |

## 写入检查清单

写入前确认：

- [ ] 这是新知识，不是重复已有内容
- [ ] 选对了目标文件（用上面的决策树）
- [ ] 选对了子目录（如果是 skills/）
- [ ] 内容精简，不冗余
- [ ] 有清晰的标题和结构

## 避免写入的内容

❌ 临时性的调试记录
❌ 一次性解决的问题（没有复用价值）
❌ 已经在其他地方记录过的知识
❌ 过于具体的细节（除非有复用价值）

---

_technical-support 的内容定位规则_
