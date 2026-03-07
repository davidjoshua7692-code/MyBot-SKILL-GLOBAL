# Entry Examples

Concrete examples of well-formatted entries.

> **注意**：文件名已有日期（如 `2026-03-07-LEARNING.md`），所以 ID 不需要带日期。

## Learning: 用户纠正

追加到 `memory/2026-03-07-LEARNING.md`：

```markdown
## [LRN-001] correction

**Priority**: high
**Area**: config

### 发生了什么
丹哥说"安装到全局"，我错误地安装到了 `main/skills/`

### 学到了什么
全局 = `~/.openclaw/skills/`
正确做法：
\`\`\`bash
cd ~/.openclaw/skills
clawhub install <skill-name>
\`\`\`

---
```

## Learning: 知识缺口

追加到 `memory/2026-03-07-LEARNING.md`：

```markdown
## [LRN-002] knowledge_gap

**Priority**: medium
**Area**: best-practices

### 发生了什么
尝试用 `npm install` 但项目用的是 pnpm

### 学到了什么
检查 `pnpm-lock.yaml` 或 `pnpm-workspace.yaml` 确认包管理器
这个项目用 `pnpm install`

---
```

## Error: 命令失败

追加到 `memory/2026-03-07-ERROR.md`：

```markdown
## [ERR-sed-syntax] sed 命令

**Priority**: high

### 错误信息
\`\`\`
sed: -e expression #1, char 5: extra characters after command
\`\`\`

### 原因 & 解决
macOS 的 sed 和 GNU sed 语法不同
用 `gsed` 或改用 `awk`

---
```

## Error: API 调用失败

追加到 `memory/2026-03-07-ERROR.md`：

```markdown
## [ERR-api-timeout] 支付 API 超时

**Priority**: critical

### 错误信息
\`\`\`
TimeoutError: Request to payments.example.com timed out after 30000ms
\`\`\`

### 原因 & 解决
高峰期超时，需要实现重试 + 指数退避
考虑熔断器模式

---
```

## Feature Request

追加到 `memory/2026-03-07-FEATURE_REQUESTS.md`：

```markdown
## [FEAT-csv-export] 导出 CSV

**Priority**: medium

### 期望的功能
分析结果导出为 CSV 格式

### 为什么需要
用户每周要出报告，需要分享给非技术人员用 Excel 打开

---
```

## Feature Request: 已实现

追加到 `memory/2026-03-07-FEATURE_REQUESTS.md`：

```markdown
## [FEAT-dark-mode] 深色模式

**Priority**: low
**Status**: resolved

### 期望的功能
Dashboard 支持深色模式

### 为什么需要
用户晚上工作，亮色界面刺眼

### Resolution
- **Resolved**: 2026-03-07
- **Commit**: #142
- **Notes**: 已实现系统偏好检测 + 手动切换

---
```

## 晋升示例

### 晋升到 TOOLS.md

**从 LEARNING.md**：
> Git push 失败因为没有配置 auth，会触发桌面弹窗

**到 TOOLS.md**：
```markdown
## Git
- 推送前确认 auth 已配置
- 用 `gh auth status` 检查 GitHub CLI 认证状态
```

### 晋升到 AGENTS.md

**从 LEARNING.md**：
> API 变更后忘记重新生成客户端，导致类型不匹配

**到 AGENTS.md**：
```markdown
## 工作流规则
- 修改 API 后，必须运行 `pnpm run generate:api`
```

### 晋升到 SOUL.md

**从 LEARNING.md**：
> 用户喜欢用比喻理解技术概念

**到 SOUL.md**：
```markdown
## 沟通风格
- 用比喻说明技术概念
```
