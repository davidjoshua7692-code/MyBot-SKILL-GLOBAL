# Image-Generator Agent 指南

image-generator workspace 的内容定位规则。

## 核心理念

- **服务至上**：用户要什么风格就做什么风格，不限定
- **美学无边界**：古风、赛博朋克、治愈系、暗黑、极简、写实、抽象... 都能驾驭
- **知识库定位**：提示词技巧 + 案例 + 各美学风格提示词

## 文件定位

| 文件 | 定位 | 内容类型 | 变化频率 |
|------|------|----------|----------|
| **MEMORY.md** | 身份 + 使用经验 + Dan 背景 | 身份定义、使用经验法则、Dan的背景 | 极少变 |
| **AGENTS.md** | 工作规则 + 流程 | Every Session、出图规则、Heartbeat/Cron 流程 | 低 |
| **skills/visual-creation-knowledge/** | 知识库 | 提示词技巧、案例、各美学风格提示词、Provider 测试、模型对比 | 中-高 |
| **skills/image-generation/** | ⚠️ 技能代码（不动） | 图像生成脚本、Provider 接口代码 | 极少变 |
| **memory/** | 每日日志 | 产出记录、临时笔记 | 每日 |

## 内容分流决策树

```
新知识出现
    ↓
1️⃣ 是身份/美学理念/Dan 相关？
    ├─ 是 → MEMORY.md
    │      例："Dan 是工业设计师"、"古风有留白的美"
    └─ 否 ↓

2️⃣ 是通用工作流程/规则？
    ├─ 是 → AGENTS.md
    │      例："每次出图自动发到 Telegram"、"Heartbeat 检查时间"
    └─ 否 ↓

3️⃣ 是 Provider 测试/试错经验/提示词技巧/美学风格？
    ├─ 是 → skills/visual-creation-knowledge/references/
    │      ├─ Provider 测试 → provider-tests.md
    │      ├─ 模型特性 → model-characteristics.md
    │      ├─ 提示词技巧 → prompt-techniques.md
    │      ├─ 美学风格提示词 → aesthetic-prompts.md （新增！）
    │      ├─ 试错经验 → troubleshooting.md
    │      └─ 模型对比 → comparisons.md
    └─ 否 ↓

4️⃣ 是今日产出/临时记录？
    ├─ 是 → memory/YYYY-MM-DD.md
    └─ 否 → 不记录
```

> ⚠️ **注意**：`skills/image-generation/` 是技能代码，不是知识库！知识写到 `skills/visual-creation-knowledge/`。

## 具体例子

### 写入 MEMORY.md

```markdown
## Dan 的背景
- 工业设计师/产品经理
- 2026年目标：打造个人IP + YouTube 讲故事频道

## 美学理念
- 古风有种留白的美
- 清冷与温柔并存
```

**特征**：身份性、长期有效、理解用户必需

### 写入 AGENTS.md

```markdown
## 工作规则
- 每次出图 → 自动发到 Telegram 对话框显示
- 发图时备注 Provider + Model

## Heartbeat 规则
- 时间：每天 12:00 / 23:00
- 任务：检查 memory/、记录工作日志
```

**特征**：每次会话都要执行、工作流程、规则

### 写入 skills/visual-creation-knowledge/

```markdown
# Provider 可用性测试 (provider-tests.md)

## ModelScope
- Z-image / flux-krea / flux-srpo ✓
- 速度：~15-36秒（免费，异步轮询）

## Gemini
- 3.1-flash / Imagen 4 / 3-pro ✓
- 注意：晚上美国工作时段，大概率 503 高负载
```

**特征**：可复用、有学习价值、技术细节

### 写入 memory/YYYY-MM-DD.md

```markdown
## 2026-03-05

### 12:40 - 随手测试
- ModelScope · Z-Image：深夜拉面店 ✓

### 12:43 - 冰棍女孩
- ModelScope · Z-Image：性感东方美女吃冰棍 ✓

### 12:54 - 杨幂 & 万茜 礼服
- ModelScope · Z-Image：红毯造型 ✓
```

**特征**：每日产出、临时记录、供 Cron 汇总使用

## 写入检查清单

写入前确认：

- [ ] 这是新知识，不是重复已有内容
- [ ] 选对了目标文件（用上面的决策树）
- [ ] 内容精简，不冗余
- [ ] 有清晰的标题和结构
- [ ] 如果是 memory/ 日志，标注时间点

## 避免写入的内容

❌ 单张图片的临时性描述（除非有学习价值）
❌ 没有复用价值的调试记录
❌ 已经在其他地方记录过的知识
❌ 过于碎片化的信息（整合后再写入）
❌ **往 `skills/image-generation/` 写任何东西（那是技能代码！）**
❌ **往 MEMORY.md 写技术细节（应该写到 knowledge skill！）**

## Cron 任务配合

**23:50 自我学习总结**：
1. 读取 `memory/YYYY-MM-DD.md`
2. 汇总今日所有产出
3. 提炼有价值信息，按决策树分流
4. 写入目标文件
5. 询问是否清理 output 文件夹

---

_image-generator 的内容定位规则_
