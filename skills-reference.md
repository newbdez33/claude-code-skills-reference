# Claude Code Skills 参考文档

本文档包含所有可用的 Skills（技能）和 Commands（命令）的详细说明。

---

## 目录

- [概述](#概述)
- [Commands（斜杠命令）](#commands斜杠命令)
- [Skills（技能模式）](#skills技能模式)
- [快速参考表](#快速参考表)

---

## 概述

Claude Code 的扩展能力分为两类：

| 类型 | 位置 | 调用方式 | 用途 |
|------|------|----------|------|
| **Commands** | `~/.claude/commands/` | `/command-name` | 触发特定工作流程 |
| **Skills** | `~/.claude/skills/` | 自动激活或手动引用 | 提供领域知识和最佳实践 |

---

## Commands（斜杠命令）

### 1. /build-fix
**名称：** Build and Fix
**用途：** 增量修复 TypeScript 和构建错误

**工作流程：**
1. 解析错误输出
2. 按严重程度分组排序
3. 逐个修复（带上下文）
4. 重新运行构建
5. 验证修复结果

**使用场景：** 构建失败时使用

---

### 2. /checkpoint
**名称：** Checkpoint Command
**用途：** 创建或验证工作流检查点

**参数：**
- `create <name>` - 创建检查点
- `verify <name>` - 验证检查点
- `list` - 列出所有检查点
- `clear` - 清除检查点

**功能：**
- 记录时间戳
- 比较文件变更
- 跟踪测试通过率
- 记录覆盖率指标

---

### 3. /code-review
**名称：** Code Review
**用途：** 对未提交的代码进行全面的安全和质量审查

**检查级别：**
- **CRITICAL** - 安全问题（硬编码凭证、SQL注入、XSS）
- **HIGH** - 代码质量（变异模式、文件大小）
- **MEDIUM** - 最佳实践（嵌套深度等）

**输出：** 按严重程度分类的报告，包含行号和修复建议

---

### 4. /e2e
**名称：** E2E Command
**用途：** 使用 Playwright 生成和运行端到端测试

**功能：**
- Page Object Model 模式
- 多浏览器测试
- 检测不稳定测试
- 捕获截图/视频/跟踪信息

**输出：**
- HTML 报告
- JUnit XML
- 截图、视频、跟踪文件

---

### 5. /eval
**名称：** Eval Command
**用途：** 管理评估驱动的开发工作流

**参数：**
- `define <name>` - 定义评估
- `check <name>` - 检查评估
- `report <name>` - 生成报告
- `list` - 列出评估
- `clean` - 清理

**指标：** pass@1、pass@3、regression pass^3 率

---

### 6. /learn
**名称：** Extract Reusable Patterns
**用途：** 从会话中提取可复用的模式并保存为技能

**可提取的模式类型：**
- 错误解决方案
- 调试技巧
- 变通方法
- 项目特定模式

**输出位置：** `~/.claude/skills/learned/[pattern-name].md`

---

### 7. /orchestrate
**名称：** Orchestrate Command
**用途：** 复杂任务的顺序 Agent 工作流

**工作流类型：**
- `feature` - 功能开发
- `bugfix` - 错误修复
- `refactor` - 重构
- `security` - 安全审查
- `custom` - 自定义

**示例链：** `planner -> tdd-guide -> code-reviewer -> security-reviewer`

---

### 8. /plan
**名称：** Plan Command
**用途：** 创建包含需求、阶段、风险的全面实施计划

**重要：** 在用户确认前**不会修改任何代码**

**输出：**
- 实施阶段
- 风险评估
- 复杂度估算

---

### 9. /refactor-clean
**名称：** Refactor Clean
**用途：** 安全识别和删除死代码

**使用工具：**
- knip
- depcheck
- ts-prune

**安全措施：** 每次删除前后运行完整测试套件，失败时回滚

**输出：** `.reports/dead-code-analysis.md`

---

### 10. /tdd
**名称：** TDD Command
**用途：** 强制执行测试驱动开发，要求 80%+ 覆盖率

**工作流：**
1. **RED** - 编写失败的测试
2. **GREEN** - 实现代码使测试通过
3. **REFACTOR** - 重构
4. **VERIFY** - 验证覆盖率

---

### 11. /test-coverage
**名称：** Test Coverage
**用途：** 分析测试覆盖率并生成缺失的测试

**关注领域：**
- Happy path（正常流程）
- 错误处理
- 边界情况（null、undefined、空值）
- 边界条件

**输出：** 前后覆盖率对比、生成的新测试

---

### 12. /update-codemaps
**名称：** Update Codemaps
**用途：** 分析代码库结构并更新架构文档

**输出文件：**
- `codemaps/architecture.md`
- `codemaps/backend.md`
- `codemaps/frontend.md`
- `codemaps/data.md`

**验证：** 变更超过 30% 时请求确认

---

### 13. /update-docs
**名称：** Update Documentation
**用途：** 从源文件同步文档

**数据源：** package.json、.env.example

**输出：**
- `docs/CONTRIB.md`
- `docs/RUNBOOK.md`

**功能：** 识别过时文档（90+ 天）

---

### 14. /verify
**名称：** Verification Command
**用途：** 对代码库状态进行全面验证

**检查项目（按顺序）：**
1. 构建
2. 类型检查
3. Lint
4. 测试
5. console.log 审计
6. git 状态

**参数：**
- `quick` - 仅构建+类型
- `full` - 全部检查
- `pre-commit` - 提交前检查
- `pre-pr` - PR 前检查

---

## Skills（技能模式）

### 1. backend-patterns
**描述：** 后端架构模式、API 设计、数据库优化

**涵盖主题：**
- API 设计模式（RESTful、Repository、Service Layer、Middleware）
- 数据库模式（查询优化、N+1 防护、事务）
- 缓存策略（Redis、cache-aside 模式）
- 错误处理（集中处理器、指数退避重试）
- 认证授权（JWT、RBAC）
- 速率限制
- 后台作业与队列
- 日志与监控

---

### 2. clickhouse-io
**描述：** ClickHouse 数据库模式、查询优化、分析

**涵盖主题：**
- 表设计（MergeTree、ReplacingMergeTree、AggregatingMergeTree）
- 查询优化（高效过滤、聚合、窗口函数）
- 数据插入（批量插入、流式处理）
- 物化视图（实时聚合）
- 性能监控
- 分析查询（时间序列、漏斗、同期群分析）
- 数据管道（ETL、CDC 模式）

---

### 3. coding-standards
**描述：** TypeScript、JavaScript、React、Node.js 的通用编码标准

**涵盖主题：**
- 代码质量原则（可读性、KISS、DRY、YAGNI）
- TypeScript/JavaScript 标准（命名、不可变性、错误处理、async/await）
- React 最佳实践（组件结构、自定义 hooks、状态管理）
- API 设计标准（REST 约定、响应格式、输入验证）
- 文件组织（项目结构、命名约定）
- 性能优化（记忆化、懒加载）
- 测试标准（AAA 模式）
- 代码异味检测

---

### 4. continuous-learning
**描述：** 自动从会话中提取可复用模式

**功能：**
- 作为 Stop hook 在会话结束时运行
- 评估 10+ 消息的会话
- 检测模式：错误解决、用户修正、变通方法、调试技巧
- 保存到 `~/.claude/skills/learned/`

---

### 5. example
**描述：** 示例技能，用于学习 skill 的基本用法

**用途：** 创建自定义技能的教学示例

---

### 6. frontend-patterns
**描述：** React、Next.js 前端开发模式

**涵盖主题：**
- 组件模式（组合、复合组件、render props）
- 自定义 Hooks（状态管理、异步数据获取、防抖）
- 状态管理（Context + Reducer 模式）
- 性能优化（记忆化、代码分割、懒加载、虚拟化）
- 表单处理（受控表单、验证）
- Error Boundary 模式
- 动画模式（Framer Motion）
- 可访问性（键盘导航、焦点管理）

---

### 7. security-review
**描述：** 安全审查技能，用于认证、用户输入、敏感功能

**检查清单：**
- 密钥管理（无硬编码、环境变量）
- 输入验证（Zod schemas、文件上传验证）
- SQL 注入防护（参数化查询）
- 认证授权（JWT、RBAC、RLS）
- XSS 防护（HTML 净化、CSP 头）
- CSRF 防护（令牌、SameSite cookies）
- 速率限制
- 敏感数据暴露（日志、错误消息）
- 依赖安全（npm audit）
- 部署前检查清单（17 项）

---

### 8. strategic-compact
**描述：** 在逻辑间隔建议手动上下文压缩

**功能：**
- 在 PreToolUse（Edit/Write）时运行
- 跟踪工具调用，达到阈值时建议（默认：50）
- 最佳实践：计划后压缩、调试后压缩、实现中不压缩

---

### 9. tdd-workflow
**描述：** 强制 80%+ 覆盖率的测试驱动开发工作流

**涵盖主题：**
- 核心原则（代码前先写测试、80% 最低覆盖率）
- 测试类型（单元、集成、E2E）
- 工作流步骤（用户旅程、测试用例、RED→GREEN→REFACTOR）
- 测试模式（Jest/Vitest、Playwright、API 集成）
- Mock 策略（Supabase、Redis、OpenAI mocks）
- 覆盖率验证
- 持续测试（watch 模式、pre-commit、CI/CD）

---

## 快速参考表

### Commands 速查

| 命令 | 用途 | 使用时机 |
|------|------|----------|
| `/build-fix` | 修复构建错误 | 构建失败时 |
| `/checkpoint` | 创建检查点 | 重要阶段完成时 |
| `/code-review` | 代码审查 | 代码修改后 |
| `/e2e` | E2E 测试 | 关键流程验证 |
| `/eval` | 评估管理 | 能力评估 |
| `/learn` | 提取模式 | 会话结束时 |
| `/orchestrate` | Agent 编排 | 复杂任务 |
| `/plan` | 实施计划 | 开始新功能前 |
| `/refactor-clean` | 清理死代码 | 代码维护 |
| `/tdd` | TDD 工作流 | 新功能/修复 |
| `/test-coverage` | 覆盖率分析 | 测试不足时 |
| `/update-codemaps` | 更新架构图 | 结构变更后 |
| `/update-docs` | 更新文档 | 文档过时时 |
| `/verify` | 全面验证 | 提交/PR 前 |

### Skills 速查

| 技能 | 领域 | 自动激活条件 |
|------|------|--------------|
| `backend-patterns` | 后端开发 | 涉及 API/数据库时 |
| `clickhouse-io` | 数据分析 | 涉及 ClickHouse 时 |
| `coding-standards` | 代码规范 | 编写代码时 |
| `continuous-learning` | 学习提取 | 会话结束时 |
| `frontend-patterns` | 前端开发 | 涉及 React/UI 时 |
| `security-review` | 安全审查 | 涉及认证/输入时 |
| `strategic-compact` | 上下文管理 | 工具调用时 |
| `tdd-workflow` | 测试驱动 | 新功能/修复时 |

---

## 文件位置

```
~/.claude/
├── commands/           # 斜杠命令定义
│   ├── build-fix.md
│   ├── checkpoint.md
│   ├── code-review.md
│   ├── e2e.md
│   ├── eval.md
│   ├── learn.md
│   ├── orchestrate.md
│   ├── plan.md
│   ├── refactor-clean.md
│   ├── tdd.md
│   ├── test-coverage.md
│   ├── update-codemaps.md
│   ├── update-docs.md
│   └── verify.md
└── skills/             # 技能模式定义
    ├── backend-patterns/
    ├── clickhouse-io/
    ├── coding-standards/
    ├── continuous-learning/
    ├── example/
    ├── frontend-patterns/
    ├── security-review/
    ├── strategic-compact/
    └── tdd-workflow/
```

---

## 使用示例

以下是每个命令的具体使用场景和示例。

### /build-fix 示例

**场景：** 运行 `npm run build` 后出现多个 TypeScript 错误

```
用户: /build-fix

Claude 会:
1. 运行 npm run build 获取错误列表
2. 解析错误，如：
   - error TS2322: Type 'string' is not assignable to type 'number'
   - error TS2304: Cannot find name 'UserType'
3. 按文件分组，从依赖最少的文件开始修复
4. 修复一个错误后重新构建验证
5. 重复直到所有错误解决
```

**实际对话：**
```
用户: 构建失败了，帮我修复
用户: /build-fix

Claude: 检测到 5 个 TypeScript 错误...
- src/utils/format.ts: 2 个类型错误
- src/components/Button.tsx: 1 个导入错误
- src/api/client.ts: 2 个类型不匹配

正在从 format.ts 开始修复...
```

---

### /checkpoint 示例

**场景：** 大型重构任务，需要保存进度点

```
用户: /checkpoint create before-refactor
Claude: ✓ 检查点 "before-refactor" 已创建
       - 时间: 2026-02-01 14:30:00
       - 文件变更: 0
       - 测试通过: 45/45
       - 覆盖率: 82%

# 进行重构工作...

用户: /checkpoint create after-api-changes
Claude: ✓ 检查点 "after-api-changes" 已创建

用户: /checkpoint verify before-refactor
Claude: 对比 "before-refactor" -> 当前状态:
       - 新增文件: 3
       - 修改文件: 12
       - 删除文件: 2
       - 测试: 45/47 通过 (新增2个)
       - 覆盖率: 82% -> 79%

用户: /checkpoint list
Claude: 可用检查点:
       1. before-refactor (14:30)
       2. after-api-changes (15:45)
```

---

### /code-review 示例

**场景：** 完成功能开发后，提交前进行代码审查

```
用户: 我刚写完用户登录功能，帮我审查一下
用户: /code-review

Claude: 正在审查未提交的变更...

## 审查报告

### CRITICAL (必须修复)
1. **src/api/auth.ts:23** - 硬编码的 API 密钥
   ```typescript
   const API_KEY = "sk-1234567890"  // 危险！
   ```
   建议: 使用环境变量 `process.env.API_KEY`

2. **src/api/auth.ts:45** - SQL 注入风险
   ```typescript
   const query = `SELECT * FROM users WHERE email = '${email}'`
   ```
   建议: 使用参数化查询

### HIGH (强烈建议修复)
3. **src/components/LoginForm.tsx:89** - 直接修改 state
   ```typescript
   user.password = hashedPassword  // 变异!
   ```
   建议: 使用展开运算符创建新对象

### MEDIUM (建议修复)
4. **src/utils/validate.ts** - 文件过长 (892 行)
   建议: 拆分为多个模块
```

---

### /e2e 示例

**场景：** 为电商网站的购物流程编写端到端测试

```
用户: 帮我为购物车到支付的流程写 E2E 测试
用户: /e2e

Claude: 正在生成 E2E 测试...

## 测试计划
用户旅程: 购物车 -> 结账 -> 支付

生成文件: tests/e2e/checkout-flow.spec.ts

```typescript
import { test, expect } from '@playwright/test'

test.describe('购物结账流程', () => {
  test('用户可以完成购买', async ({ page }) => {
    // 1. 添加商品到购物车
    await page.goto('/products/1')
    await page.click('[data-testid="add-to-cart"]')

    // 2. 进入购物车
    await page.click('[data-testid="cart-icon"]')
    await expect(page.locator('.cart-item')).toHaveCount(1)

    // 3. 开始结账
    await page.click('[data-testid="checkout-btn"]')

    // 4. 填写收货地址
    await page.fill('#address', '测试地址123号')
    await page.click('[data-testid="continue"]')

    // 5. 完成支付
    await page.fill('#card-number', '4242424242424242')
    await page.click('[data-testid="pay-now"]')

    // 6. 验证成功
    await expect(page).toHaveURL(/\/order-confirmation/)
  })
})
```

运行测试:
✓ Chromium: 通过
✓ Firefox: 通过
✓ WebKit: 通过

截图已保存: tests/e2e/screenshots/
```

---

### /eval 示例

**场景：** 为 AI 代码助手定义评估指标

```
用户: /eval define code-completion

Claude: 定义评估 "code-completion"...

请提供:
1. 评估目标: 测试代码补全准确性
2. 测试用例数量: 50
3. 评估标准: 语法正确、功能正确、风格一致

✓ 评估定义已保存

用户: /eval check code-completion

Claude: 运行评估 "code-completion"...

结果:
- pass@1: 76% (38/50)
- pass@3: 89% (44/50)
- 平均延迟: 1.2s

失败用例分析:
- 复杂泛型: 4 个失败
- 异步模式: 3 个失败
- 边界情况: 5 个失败

用户: /eval report code-completion

Claude: 生成报告...
报告已保存: .reports/eval-code-completion-2026-02-01.md
```

---

### /learn 示例

**场景：** 会话中解决了一个棘手的问题，想保存经验

```
用户: /learn

Claude: 分析当前会话...

检测到可提取的模式:

## 1. Next.js 13 App Router 缓存问题
**问题:** fetch 请求被意外缓存
**解决方案:**
```typescript
fetch(url, { cache: 'no-store' })
// 或
export const dynamic = 'force-dynamic'
```
**适用场景:** 需要实时数据的 API 路由

是否保存此模式? [Y/n]

用户: Y

Claude: ✓ 模式已保存到:
~/.claude/skills/learned/nextjs-cache-bypass.md

下次遇到类似问题时会自动应用此知识。
```

---

### /orchestrate 示例

**场景：** 实现一个完整的新功能，需要多个 Agent 协作

```
用户: 我要添加用户头像上传功能
用户: /orchestrate feature

Claude: 启动功能开发编排...

## 工作流: feature
Agent 链: planner -> tdd-guide -> code-reviewer -> security-reviewer

---
### 阶段 1/4: planner
正在分析需求并创建实施计划...

实施计划:
1. 创建文件上传 API (/api/upload)
2. 添加图片处理服务 (裁剪、压缩)
3. 更新用户模型添加 avatarUrl 字段
4. 创建前端上传组件
5. 添加存储服务集成 (S3)

[等待确认后继续]

---
### 阶段 2/4: tdd-guide
编写测试用例...
- 单元测试: 图片验证、尺寸调整
- 集成测试: 上传 API
- E2E 测试: 完整上传流程

---
### 阶段 3/4: code-reviewer
审查实现代码...
发现 2 个问题，已自动修复

---
### 阶段 4/4: security-reviewer
安全审查...
✓ 文件类型验证
✓ 文件大小限制
✓ 恶意文件检测
⚠ 建议: 添加病毒扫描集成

## 编排完成
所有阶段已完成，代码已准备好提交。
```

---

### /plan 示例

**场景：** 开始一个复杂功能前，需要制定详细计划

```
用户: 我想给应用添加多语言支持
用户: /plan

Claude: 正在分析需求并创建实施计划...

# 多语言支持实施计划

## 需求分析
- 支持语言: 中文、英文、日文
- 范围: 所有 UI 文本、错误消息、邮件模板

## 技术选型
推荐: next-intl (Next.js 官方推荐)
备选: react-i18next, lingui

## 实施阶段

### 阶段 1: 基础设施 (预计改动 5 个文件)
- 安装 next-intl
- 配置 middleware.ts
- 创建 messages/ 目录结构

### 阶段 2: 提取文本 (预计改动 23 个文件)
- 扫描所有硬编码文本
- 创建翻译键
- 替换为 useTranslations()

### 阶段 3: 翻译内容
- 创建 en.json, zh.json, ja.json
- 设置翻译工作流

### 阶段 4: 语言切换
- 添加语言选择器组件
- 持久化用户偏好

## 风险评估
- 中: 某些动态文本可能遗漏
- 低: 日期/数字格式需要本地化

## 依赖关系
阶段 1 -> 阶段 2 -> 阶段 3 (可并行) -> 阶段 4

---
**是否确认此计划并开始实施?** [确认/修改/取消]
```

---

### /refactor-clean 示例

**场景：** 项目积累了很多未使用的代码，需要清理

```
用户: 项目里有很多没用的代码，帮我清理
用户: /refactor-clean

Claude: 正在分析代码库...

## 死代码分析报告

### 使用工具
- knip: 检测未使用的导出
- depcheck: 检测未使用的依赖
- ts-prune: 检测未使用的 TypeScript 代码

### 发现的死代码

#### 未使用的文件 (5个)
- src/utils/deprecated-helpers.ts (整个文件未被引用)
- src/components/OldButton.tsx (已被 Button.tsx 替代)
- src/hooks/useLegacyAuth.ts (无引用)
- src/types/v1-api.ts (旧版 API 类型)
- src/constants/magic-numbers.ts (无引用)

#### 未使用的导出 (12个)
- src/utils/format.ts: formatLegacyDate()
- src/utils/format.ts: parseOldFormat()
- src/api/client.ts: deprecatedFetch()
...

#### 未使用的依赖 (3个)
- moment (已迁移到 date-fns)
- lodash (只用了 2 个函数，已内联)
- classnames (已用 clsx 替代)

### 安全删除计划
1. 运行完整测试套件 ✓ 47/47 通过
2. 删除 deprecated-helpers.ts
3. 重新运行测试 ✓ 47/47 通过
4. 继续下一个文件...

### 清理结果
- 删除文件: 5 个
- 删除代码行: 847 行
- 移除依赖: 3 个
- 包体积减少: ~120KB

报告已保存: .reports/dead-code-analysis.md
```

---

### /tdd 示例

**场景：** 用测试驱动开发实现一个新功能

```
用户: 我要写一个密码强度检查函数
用户: /tdd

Claude: 启动 TDD 工作流...

## 步骤 1: 定义接口

```typescript
// src/utils/password.ts
interface PasswordStrength {
  score: 0 | 1 | 2 | 3 | 4  // 0=很弱, 4=很强
  feedback: string[]
}

function checkPasswordStrength(password: string): PasswordStrength
```

## 步骤 2: 编写测试 (RED)

```typescript
// src/utils/password.test.ts
describe('checkPasswordStrength', () => {
  test('空密码返回 score 0', () => {
    expect(checkPasswordStrength('')).toEqual({
      score: 0,
      feedback: ['密码不能为空']
    })
  })

  test('少于8位返回 score 1', () => {
    expect(checkPasswordStrength('abc123')).toEqual({
      score: 1,
      feedback: ['密码至少需要8个字符']
    })
  })

  test('包含大小写和数字返回 score 3', () => {
    const result = checkPasswordStrength('Abcd1234')
    expect(result.score).toBe(3)
  })

  test('包含特殊字符返回 score 4', () => {
    const result = checkPasswordStrength('Abcd1234!')
    expect(result.score).toBe(4)
  })
})
```

运行测试:
✗ 4 个测试失败 (预期行为)

## 步骤 3: 实现代码 (GREEN)

```typescript
export function checkPasswordStrength(password: string): PasswordStrength {
  const feedback: string[] = []
  let score = 0

  if (!password) {
    return { score: 0, feedback: ['密码不能为空'] }
  }

  if (password.length >= 8) score++
  else feedback.push('密码至少需要8个字符')

  if (/[a-z]/.test(password) && /[A-Z]/.test(password)) score++
  else feedback.push('需要包含大小写字母')

  if (/\d/.test(password)) score++
  else feedback.push('需要包含数字')

  if (/[!@#$%^&*]/.test(password)) score++
  else feedback.push('建议包含特殊字符')

  return { score: score as 0|1|2|3|4, feedback }
}
```

运行测试:
✓ 4 个测试通过

## 步骤 4: 验证覆盖率

覆盖率: 100% ✓ (超过 80% 要求)
- 行覆盖: 100%
- 分支覆盖: 100%
- 函数覆盖: 100%

TDD 工作流完成!
```

---

### /test-coverage 示例

**场景：** 发现测试覆盖率不足，需要补充测试

```
用户: 测试覆盖率太低了，帮我补充
用户: /test-coverage

Claude: 分析当前测试覆盖率...

## 当前覆盖率: 62%

### 覆盖率详情
| 文件 | 行 | 分支 | 函数 |
|------|-----|------|------|
| src/api/client.ts | 45% | 30% | 50% |
| src/utils/format.ts | 78% | 65% | 80% |
| src/hooks/useAuth.ts | 55% | 40% | 60% |

### 缺失测试分析

#### src/api/client.ts (需要 +35% 覆盖)
缺失场景:
- 网络错误处理
- 401 未授权响应
- 请求超时
- 重试逻辑

#### src/hooks/useAuth.ts (需要 +25% 覆盖)
缺失场景:
- token 过期刷新
- 登出清理
- 并发请求处理

### 生成测试

```typescript
// src/api/client.test.ts (新增)
describe('API Client 错误处理', () => {
  test('网络错误时抛出 NetworkError', async () => {
    fetchMock.mockReject(new Error('Network failed'))
    await expect(apiClient.get('/users')).rejects.toThrow('NetworkError')
  })

  test('401 响应时触发登出', async () => {
    fetchMock.mockResponse('', { status: 401 })
    await apiClient.get('/users')
    expect(authStore.logout).toHaveBeenCalled()
  })

  test('超时后重试 3 次', async () => {
    fetchMock.mockTimeout()
    await apiClient.get('/users')
    expect(fetchMock).toHaveBeenCalledTimes(3)
  })
})
```

### 结果
- 之前覆盖率: 62%
- 之后覆盖率: 84% ✓
- 新增测试: 15 个
- 目标达成: 超过 80%
```

---

### /update-codemaps 示例

**场景：** 项目结构发生变化，需要更新架构文档

```
用户: 最近重构了很多，帮我更新架构文档
用户: /update-codemaps

Claude: 正在分析代码库结构...

## 扫描结果
- 总文件数: 156
- 源代码文件: 89
- 测试文件: 45
- 配置文件: 22

## 生成 Codemaps

### codemaps/architecture.md
```markdown
# 项目架构

## 技术栈
- 框架: Next.js 14 (App Router)
- 语言: TypeScript 5.3
- 样式: Tailwind CSS
- 状态: Zustand
- 数据库: PostgreSQL + Prisma

## 目录结构
src/
├── app/           # 页面路由
├── components/    # UI 组件 (47个)
├── hooks/         # 自定义 hooks (12个)
├── lib/           # 工具库
├── api/           # API 客户端
└── types/         # TypeScript 类型
```

### codemaps/backend.md
```markdown
# 后端架构

## API 路由
- /api/auth/*     - 认证 (登录、注册、刷新)
- /api/users/*    - 用户管理
- /api/products/* - 产品 CRUD
- /api/orders/*   - 订单处理

## 数据模型
- User (1:N Orders)
- Product (M:N Categories)
- Order (N:1 User, M:N Products)
```

### 变更检测
与上次相比变化: 23%
- 新增: api/webhooks/ 目录
- 重命名: services/ -> lib/
- 删除: utils/deprecated/

文档已更新:
✓ codemaps/architecture.md
✓ codemaps/backend.md
✓ codemaps/frontend.md
✓ codemaps/data.md
```

---

### /update-docs 示例

**场景：** package.json 和环境变量更新了，需要同步文档

```
用户: 我更新了一些依赖和环境变量，帮我更新文档
用户: /update-docs

Claude: 正在同步文档...

## 数据源
- package.json (上次修改: 2小时前)
- .env.example (上次修改: 1天前)

## 检测到的变更

### 新增依赖
- @tanstack/react-query: ^5.0.0
- zod: ^3.22.0

### 移除依赖
- swr (已被 react-query 替代)

### 新增环境变量
- REDIS_URL: Redis 连接字符串
- RATE_LIMIT_MAX: 速率限制最大值

### 生成文档

#### docs/CONTRIB.md
```markdown
## 开发环境设置

### 必需环境变量
| 变量 | 描述 | 示例 |
|------|------|------|
| DATABASE_URL | 数据库连接 | postgresql://... |
| REDIS_URL | Redis 连接 | redis://... | ← 新增
| RATE_LIMIT_MAX | 速率限制 | 100 | ← 新增

### 安装依赖
npm install

### 主要依赖说明
- @tanstack/react-query: 服务端状态管理 ← 新增
- zod: 运行时类型验证 ← 新增
```

#### docs/RUNBOOK.md
```markdown
## 运维手册

### 新增: Redis 配置
确保 REDIS_URL 环境变量已设置...

### 更新: 缓存策略
已从 SWR 迁移到 React Query...
```

### 过时文档警告
- docs/API-V1.md (142 天未更新) ⚠️

文档已更新 ✓
```

---

### /verify 示例

**场景：** 准备提交代码或创建 PR 前进行全面检查

```
用户: 我准备提交了，帮我检查一下
用户: /verify pre-commit

Claude: 运行预提交验证...

## 验证报告

### 1. 构建检查
npm run build
✓ 通过 (12.3s)

### 2. 类型检查
npx tsc --noEmit
✓ 通过 (8.1s)

### 3. Lint 检查
npm run lint
⚠ 2 个警告:
  - src/utils/format.ts:23 - 未使用的变量 'temp'
  - src/api/client.ts:45 - 建议使用 const

### 4. 测试
npm run test
✓ 47/47 通过 (15.2s)
覆盖率: 83%

### 5. console.log 审计
⚠ 发现 2 处 console.log:
  - src/hooks/useAuth.ts:34
  - src/api/client.ts:89

### 6. Git 状态
已暂存: 5 个文件
未暂存: 2 个文件 (上述 console.log 文件)

## 总结
| 检查项 | 状态 |
|--------|------|
| 构建 | ✓ |
| 类型 | ✓ |
| Lint | ⚠ 警告 |
| 测试 | ✓ |
| console.log | ⚠ 需清理 |

**建议:** 移除 console.log 后再提交
```

```
用户: /verify quick

Claude: 快速验证...

✓ 构建: 通过
✓ 类型: 通过

快速验证完成，可以继续。
```

---

*文档生成时间：2026-02-01*
