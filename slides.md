---
theme: seriph
title: Claude Code 应用实践
titleTemplate: '%s - Runxi'
info: |
  ## Claude Code 应用实践
  从入门到进阶的 AI 辅助编程实践分享
author: Runxi Zhang
keywords: claude-code,ai,programming,presentation
colorSchema: dark
aspectRatio: 16/9
canvasWidth: 980
fonts:
  sans: Inter, Noto Sans SC
  serif: Inter, Noto Serif SC
  mono: Fira Code
favicon: https://cdn.jsdelivr.net/gh/slidevjs/slidev/assets/favicon.png
download: false
overviewSnapshots: false
exportFilename: claude-code-in-practice
transition: slide-left
---

# Claude Code 应用实践

从零开始的 AI 辅助编程之旅

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    按空格键继续 <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/anthropics/claude-code" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
欢迎来到 Claude Code 应用实践分享。
-->

---
transition: fade-out
layout: two-cols
layoutClass: gap-16
---

# 目录

<v-clicks>

- **什么是 Claude Code** - AI 编程助手简介
- **核心工作流** - 从需求到交付
- **实战演示** - 真实项目案例
- **高级技巧** - 提升效率的秘密
- **最佳实践** - 避坑指南

</v-clicks>

::right::

<div class="flex items-center justify-center h-full">

# Contents

<v-clicks>

- **What is Claude Code** - AI coding assistant
- **Core Workflow** - From requirements to delivery
- **Live Demo** - Real-world cases
- **Advanced Tips** - Productivity secrets
- **Best Practices** - Pitfall guide

</v-clicks>

</div>

---
transition: slide-up
layout: image-right
image: https://images.unsplash.com/photo-1555066931-4365d14bab8c?w=800
---

# 什么是 Claude Code
What is Claude Code

Claude Code 是 Anthropic 推出的命令行 AI 编程助手。

<v-clicks>

- 🔍 **理解代码库** - 自动分析项目结构
- ✏️ **读写文件** - 直接修改源代码
- 🖥️ **执行命令** - 运行测试、构建、部署
- 🔄 **Git 操作** - 提交、推送、创建 PR
- 🧠 **上下文记忆** - CLAUDE.md 持久化配置

</v-clicks>

---
layout: center
class: text-center
---

# 核心工作流
Core Workflow

<div class="grid grid-cols-5 gap-4 mt-8">
  <div v-click class="workflow-step">
    <div class="text-4xl mb-2">📋</div>
    <div class="font-bold">Plan</div>
    <div class="text-sm opacity-60">需求分析</div>
  </div>
  <div v-click class="workflow-step">
    <div class="text-4xl mb-2">🧪</div>
    <div class="font-bold">Test</div>
    <div class="text-sm opacity-60">编写测试</div>
  </div>
  <div v-click class="workflow-step">
    <div class="text-4xl mb-2">💻</div>
    <div class="font-bold">Code</div>
    <div class="text-sm opacity-60">实现功能</div>
  </div>
  <div v-click class="workflow-step">
    <div class="text-4xl mb-2">🔍</div>
    <div class="font-bold">Review</div>
    <div class="text-sm opacity-60">代码审查</div>
  </div>
  <div v-click class="workflow-step">
    <div class="text-4xl mb-2">🚀</div>
    <div class="font-bold">Ship</div>
    <div class="text-sm opacity-60">部署交付</div>
  </div>
</div>

<style>
.workflow-step {
  @apply p-4 rounded-lg bg-white bg-opacity-5 border border-white border-opacity-10;
}
</style>

---
layout: two-cols
layoutClass: gap-8
---

# Plan 阶段

使用 `/plan` 命令让 Claude 理解需求并制定方案。

```bash
# 启动 Claude Code
claude

# 输入需求
> /plan 实现用户认证模块，
  支持 JWT + OAuth2
```

<v-click>

Claude 会输出：
- 需求分析
- 技术选型
- 实施步骤
- 风险评估

</v-click>

::right::

# Plan Phase

Use `/plan` to let Claude understand requirements.

```bash
# Launch Claude Code
claude

# Input requirements
> /plan Implement user auth module,
  support JWT + OAuth2
```

<v-click>

Claude outputs:
- Requirements analysis
- Tech stack decision
- Implementation steps
- Risk assessment

</v-click>

---

# TDD 工作流
Test-Driven Development

<div class="grid grid-cols-3 gap-8 mt-8">

<div v-click>

### 🔴 Red

```typescript
// 先写测试
describe('auth', () => {
  it('should generate valid JWT', () => {
    const token = generateToken(user)
    expect(token).toBeDefined()
    expect(verifyToken(token)).toEqual(user)
  })
})
```

</div>

<div v-click>

### 🟢 Green

```typescript
// 最小实现
function generateToken(user: User) {
  return jwt.sign(
    { id: user.id, email: user.email },
    SECRET,
    { expiresIn: '24h' }
  )
}
```

</div>

<div v-click>

### 🔵 Refactor

```typescript
// 优化重构
function generateToken(
  user: User,
  options?: TokenOptions
) {
  const { expiresIn = '24h' } = options ?? {}
  return jwt.sign(
    pick(user, ['id', 'email']),
    SECRET,
    { expiresIn }
  )
}
```

</div>

</div>

---
layout: center
---

# 交互式演示
Interactive Demo

<div class="mt-8">
  <PromptDemo />
</div>

---

# 高级技巧
Advanced Tips

<v-clicks>

## 1. CLAUDE.md 配置

在项目根目录放置 `CLAUDE.md`，持久化你的偏好：

```markdown
# CLAUDE.md
- 使用 TypeScript strict mode
- 测试覆盖率 > 80%
- 提交信息使用中文
```

## 2. Agent 委派

利用子 Agent 并行处理任务：

```bash
> 并行执行：安全审查 auth 模块、
  性能测试 cache 系统、类型检查 utils
```

</v-clicks>

---

# 高级技巧（续）
Advanced Tips (cont.)

<v-clicks>

## 3. Hooks 自动化

配置 hooks 实现自动格式化、lint 检查：

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": { "tool": "Write" },
      "command": "npx prettier --write $FILE"
    }]
  }
}
```

## 4. MCP 扩展

通过 MCP Server 扩展 Claude Code 的能力：
- GitHub 操作
- 数据库查询
- 浏览器自动化

</v-clicks>

---
layout: two-cols
---

# 最佳实践
Best Practices

<v-clicks>

### ✅ 推荐

- 先 plan 再 code
- 小步提交、频繁验证
- 利用 CLAUDE.md 沉淀经验
- 善用 `/compact` 管理上下文
- 多用 Agent 并行加速

</v-clicks>

::right::

<div class="mt-12">

<v-clicks>

### ❌ 避免

- 一次性提交大量变更
- 忽略测试直接上线
- 不读代码就让 AI 修改
- 盲目信任 AI 输出
- 在上下文末尾做复杂重构

</v-clicks>

</div>

---
layout: center
class: text-center
---

# AI 辅助编程的五个层次
Five Levels of AI-Assisted Programming

<div class="grid grid-cols-5 gap-3 mt-8 text-sm">
  <div v-click class="level-card">
    <div class="text-2xl mb-2">L0</div>
    <div class="font-bold">手工编程</div>
    <div class="opacity-60">Manual Coding</div>
  </div>
  <div v-click class="level-card">
    <div class="text-2xl mb-2">L1</div>
    <div class="font-bold">代码补全</div>
    <div class="opacity-60">Autocomplete</div>
  </div>
  <div v-click class="level-card active">
    <div class="text-2xl mb-2">L2</div>
    <div class="font-bold">对话编程</div>
    <div class="opacity-60">Chat Coding</div>
  </div>
  <div v-click class="level-card active">
    <div class="text-2xl mb-2">L3</div>
    <div class="font-bold">Agent 编程</div>
    <div class="opacity-60">Agentic Coding</div>
  </div>
  <div v-click class="level-card">
    <div class="text-2xl mb-2">L4</div>
    <div class="font-bold">全自动团队</div>
    <div class="opacity-60">Autonomous Team</div>
  </div>
</div>

<style>
.level-card {
  @apply p-4 rounded-lg bg-white bg-opacity-5 border border-white border-opacity-10;
}
.level-card.active {
  @apply bg-blue-500 bg-opacity-20 border-blue-400 border-opacity-40;
}
</style>

---
layout: center
class: text-center
---

# 谢谢 | Thank You

[GitHub](https://github.com/anthropics/claude-code) | [文档 Docs](https://docs.anthropic.com/en/docs/claude-code)

<div class="mt-8 opacity-60">
  Powered by Slidev
</div>
