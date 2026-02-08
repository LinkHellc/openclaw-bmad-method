---
name: bmad-method
description: BMAD方法集成 - 在Claude Code中使用结构化的四阶段工作流（分析→规划→解决方案→实现）进行AI驱动开发，包含完整的工作流指南、代理模拟和上下文管理。使用于新项目开发、复杂功能构建、需要文档驱动的场景，以及希望使用BMAD规范的开发流程时。
---

# BMAD方法 - Claude Code集成

## 概述

本SKILL将BMAD方法（Breakthrough Method of Agile AI Driven Development）集成到Claude Code中，提供结构化的、文档驱动的AI开发工作流程。BMAD通过四个阶段（分析、规划、解决方案、实现）和专门的AI代理，帮助团队从想法到实现，构建高质量软件。

## 核心理念

BMAD的核心在于**上下文工程** - AI代理需要清晰、结构化的上下文才能发挥最佳效果。BMAD系统通过四个递进阶段构建上下文，每个阶段和工作流产生的文档为下一阶段提供信息，确保AI始终知道要构建什么以及为什么要构建。

## 使用场景

当你需要：
- 从零开始一个新项目
- 构建复杂功能或平台（10-50+个故事）
- 需要结构化的规划和文档
- 希望AI代理理解完整的上下文
- 企业级合规系统

## 选择规划轨道

根据项目复杂度选择：

### Quick Flow（快速流程）
- **适用：** Bug修复、简单功能、明确范围（1-15个故事）
- **文档：** 仅tech-spec
- **跳过：** Phases 1-3

### BMad Method（BMAD标准方法）
- **适用：** 产品、平台、复杂功能（10-50+个故事）
- **文档：** PRD + Architecture + UX
- **推荐：** 大多数项目的最佳选择

### Enterprise（企业级）
- **适用：** 合规、多租户系统（30+个故事）
- **文档：** PRD + Architecture + Security + DevOps
- **扩展：** 企业级安全、运维文档

## BMAD四阶段工作流

### Phase 1: Analysis（分析阶段）- 可选

探索问题空间，在承诺规划之前验证想法。

| 工作流 | 命令 | 代理 | 目的 | 产出 |
|--------|------|------|------|------|
| Brainstorming | `/bmad-brainstorming` | Analyst | 引导式头脑风暴 | brainstorming-report.md |
| Research | `/bmad-bmm-research` | Analyst | 市场/技术/领域验证 | Research findings |
| Product Brief | `/bmad-bmm-create-product-brief` | Analyst | 捕获战略愿景 | product-brief.md |

**何时使用：**
- 项目还不明确
- 需要市场或技术验证
- 想要结构化的战略文档

### Phase 2: Planning（规划阶段）- 必需

定义要构建什么以及为谁构建。

| 工作流 | 命令 | 代理 | 目的 | 产出 |
|--------|------|------|------|------|
| Create PRD | `/bmad-bmm-create-prd` | PM | 定义需求（FRs/NFRs） | PRD.md |
| Create UX Design | `/bmad-bmm-create-ux-design` | UX | 设计用户体验（可选） | ux-spec.md |

**重要：**
- 对于Quick Flow轨道，跳过PRD，直接使用tech-spec
- 对于BMad Method和Enterprise轨道，PRD是必需的

### Phase 3: Solutioning（解决方案阶段）- BMad Method/Enterprise

决定如何构建并分解为可执行的故事。

| 工作流 | 命令 | 代理 | 目的 | 产出 |
|--------|------|------|------|------|
| Create Architecture | `/bmad-bmm-create-architecture` | Architect | 明确技术决策 | architecture.md with ADRs |
| Create Epics & Stories | `/bmad-bmm-create-epics-and-stories` | PM | 分解需求为故事 | Epic files with stories |
| Check Implementation Readiness | `/bmad-bmm-check-implementation-readiness` | Architect | 实现前的门控检查 | PASS/CONCERNS/FAIL |

**实施就绪检查：**
- 验证所有规划文档之间的一致性
- 识别潜在的实现风险
- 确保故事可执行且完整

### Phase 4: Implementation（实现阶段）

逐个故事构建。

#### 初始化（每项目一次）

| 工作流 | 命令 | 代理 | 目的 | 产出 |
|--------|------|------|------|------|
| Sprint Planning | `/bmad-bmm-sprint-planning` | SM | 初始化跟踪 | sprint-status.yaml |

#### 实施循环（每个故事）

| 步骤 | 命令 | 代理 | 目的 | 产出 |
|------|------|------|------|------|
| 1. 创建故事 | `/bmad-bmm-create-story` | SM | 从epic创建故事文件 | story-[slug].md |
| 2. 开发故事 | `/bmad-bmm-dev-story` | DEV | 实现故事 | Working code + tests |
| 3. 代码审查 | `/bmad-bmm-code-review` | DEV | 质量验证（推荐） | Approved or changes requested |

#### 完成Epic后

| 工作流 | 命令 | 代理 | 目的 |
|--------|------|------|------|
| Retrospective | `/bmad-bmm-retrospective` | SM | 复盘学到的教训 |

### Quick Flow（并行轨道）

跳过Phases 1-3，适用于小型的、清晰的工作。

| 工作流 | 命令 | 代理 | 目的 | 产出 |
|--------|------|------|------|------|
| Quick Spec | `/bmad-bmm-quick-spec` | PM | 定义临时变更 | tech-spec.md |
| Quick Dev | `/bmad-bmm-quick-dev` | DEV | 从规范实现 | Working code + tests |

## 上下文管理

每个文档成为下一阶段的上下文：
- PRD告诉Architect什么约束重要
- Architecture告诉DEV agent要遵循的模式
- 故事文件为实施提供集中、完整的上下文

### 文档加载规则

| 工作流 | 额外加载 |
|--------|----------|
| create-story | epics, PRD, architecture, UX |
| dev-story | story file |
| code-review | architecture, story file |
| quick-spec | 规划文档（如果存在） |
| quick-dev | tech-spec |

### Project Context

对于已建立的项目，使用`document-project`工作流创建或更新`project-context.md` - 记录代码库中存在的内容以及所有实施工作流必须遵守的规则。

**何时运行：**
- Phase 4之前
- 当发生重大变化（结构、架构或规则）时

你也可以手动编辑`project-context.md`。

所有实施工作流如果存在都会加载`project-context.md`。

## Claude Code集成

### 初始化项目

```bash
# 1. 进入项目目录
cd /path/to/your/project

# 2. 启动Claude Code
claude

# 3. 加载BMAD skill上下文
使用本SKILL的指导进行BMAD工作流
```

### 推荐的Claude Code工作流程

#### 对于新项目（BMad Method轨道）

1. **加载本SKILL** - 理解BMAD框架
2. **Phase 1: Analysis（可选）**
   ```
   "使用BMAD Analyst代理，运行brainstorming工作流来探索项目想法"
   ```
3. **Phase 2: Planning**
   ```
   "使用BMAD PM代理，创建PRD.md文档"
   ```
4. **Phase 3: Solutioning**
   ```
   "使用BMAD Architect代理，创建architecture.md"
   "使用BMAD PM代理，基于PRD和architecture创建epics和stories"
   "使用BMAD Architect代理，检查implementation readiness"
   ```
5. **Phase 4: Implementation**
   ```
   "使用BMAD SM代理，初始化sprint planning"
   "使用BMAD SM代理，创建第一个story文件"
   "使用BMAD DEV代理，实现该story"
   "使用BMAD DEV代理，进行code review"
   ```

#### 对于快速修复（Quick Flow轨道）

```
"使用BMAD PM代理，创建quick-spec for this bug fix"
"使用BMAD DEV代理，从spec实现修复"
```

### Claude Code中的代理模拟

BMAD的代理可以在Claude Code中通过以下方式模拟：

#### PM（产品经理）代理
**角色：** 负责需求、PRD、Epic/Story分解

**Claude Code提示：**
```
你现在担任BMAD PM代理。请：
1. 加载相关的规划文档（PRD, product brief等）
2. 创建或更新PRD.md
3. 将PRD分解为epics和stories
4. 确保故事是技术可实现的
```

#### Architect（架构师）代理
**角色：** 负责架构设计、技术决策、实施就绪检查

**Claude Code提示：**
```
你现在担任BMAD Architect代理。请：
1. 加载PRD.md和相关上下文
2. 创建architecture.md，包含ADRs（架构决策记录）
3. 检查implementation readiness，验证所有文档的一致性
4. 识别潜在的技术风险
```

#### SM（Scrum Master）代理
**角色：** 负责sprint规划、故事创建、流程管理

**Claude Code提示：**
```
你现在担任BMAD SM代理。请：
1. 初始化sprint-status.yaml
2. 从epic创建story文件
3. 跟踪story状态
4. 在epic完成后组织retrospective
```

#### DEV（开发者）代理
**角色：** 负责故事实现、代码审查、测试

**Claude Code提示：**
```
你现在担任BMAD DEV代理。请：
1. 加载story文件和相关上下文
2. 实现story中的需求
3. 编写相应的测试
4. 进行code review确保质量
5. 遵循architecture中定义的模式
```

#### Analyst（分析师）代理
**角色：** 负责头脑风暴、研究、产品简介

**Claude Code提示：**
```
你现在担任BMAD Analyst代理。请：
1. 进行brainstorming session
2. 进行市场/技术研究
3. 创建product-brief.md
4. 为后续阶段提供分析见解
```

## 目录结构

使用BMAD后，你的项目结构如下：

```
your-project/
├── _bmad/                    # BMAD配置
│   ├── agents/               # 代理配置
│   ├── workflows/            # 工作流定义
│   ├── tasks/                # 任务定义
│   └── config/               # 配置文件
├── _bmad-output/            # BMAD输出目录
│   ├── PRD.md               # 产品需求文档
│   ├── architecture.md      # 架构文档
│   ├── ux-spec.md           # UX规范（可选）
│   ├── epics/               # Epic和故事文件
│   │   ├── epic-001-name.md
│   │   ├── story-001-name.md
│   │   └── ...
│   ├── sprint-status.yaml   # Sprint跟踪
│   └── project-context.md   # 项目上下文
└── ...                      # 你的项目代码
```

## 最佳实践

### 1. 使用新鲜会话
- 每个工作流使用新的Claude Code会话
- 这确保上下文清晰，避免混淆

### 2. 文档优先
- 不要跳过文档创建
- 文档是AI上下文的基石
- 文档比"记住"持久

### 3. 迭代改进
- 计划不是一成不变的
- 使用`correct-course`工作流处理中期变更
- 在每个epic完成后进行retrospective

### 4. 上下文连续性
- 每个阶段引用前一阶段的文档
- 保持文档同步更新
- 文档变更时通知所有相关代理

### 5. 质量门控
- 不要跳过implementation readiness check
- 使用code review工作流
- 每个epic后进行retrospective

## 常见问题

### 我总是需要architecture吗？
只有BMad Method和Enterprise轨道需要。Quick Flow直接从tech-spec到implementation。

### 我可以稍后改变计划吗？
可以。SM代理有`correct-course`工作流（`/bmad-bmm-correct-course`）用于处理范围变更。

### 如果我想先brainstorm怎么办？
在开始PRD之前，加载Analyst代理并运行brainstorming工作流。

### 我需要严格遵循顺序吗？
不需要严格顺序。一旦你掌握了流程，可以直接使用工作流命令。

### Quick Flow和标准方法的区别是什么？
- Quick Flow：快速、临时、小型工作
- BMad Method：完整规划、文档驱动、结构化开发

## 工作流快速参考

| 工作流 | 命令 | 代理 | 阶段 | 产出 |
|--------|------|------|------|------|
| Help | `/bmad-help` | Any | - | 下一步指导 |
| Brainstorming | `/bmad-brainstorming` | Analyst | 1 | brainstorming-report.md |
| Research | `/bmad-bmm-research` | Analyst | 1 | Research findings |
| Create Product Brief | `/bmad-bmm-create-product-brief` | Analyst | 1 | product-brief.md |
| Create PRD | `/bmad-bmm-create-prd` | PM | 2 | PRD.md |
| Create UX Design | `/bmad-bmm-create-ux-design` | UX | 2 | ux-spec.md |
| Create Architecture | `/bmad-bmm-create-architecture` | Architect | 3 | architecture.md |
| Create Epics & Stories | `/bmad-bmm-create-epics-and-stories` | PM | 3 | Epic files |
| Check Implementation Readiness | `/bmad-bmm-check-implementation-readiness` | Architect | 3 | PASS/CONCERNS/FAIL |
| Sprint Planning | `/bmad-bmm-sprint-planning` | SM | 4 | sprint-status.yaml |
| Create Story | `/bmad-bmm-create-story` | SM | 4 | story-[slug].md |
| Dev Story | `/bmad-bmm-dev-story` | DEV | 4 | Working code |
| Code Review | `/bmad-bmm-code-review` | DEV | 4 | Approved/rejected |
| Correct Course | `/bmad-bmm-correct-course` | SM | 4 | Updated plan |
| Retrospective | `/bmad-bmm-retrospective` | SM | 4 | Lessons learned |
| Quick Spec | `/bmad-bmm-quick-spec` | PM | Quick Flow | tech-spec.md |
| Quick Dev | `/bmad-bmm-quick-dev` | DEV | Quick Flow | Working code |

## 额外资源

- [BMAD Method文档](https://docs.bmad-method.org/)
- [Claude Code文档](https://code.claude.com/docs/en/overview)
- [BMAD Discord社区](https://discord.gg/gk8jAdXWmj)
- [BMAD GitHub](https://github.com/bmad-code-org/BMAD-METHOD)

## 开始使用

1. 阅读本文档，理解BMAD框架
2. 根据项目规模选择规划轨道
3. 使用本SKILL提供的Claude Code提示和工作流指导
4. 在你的项目中应用BMAD方法论
5. 享受结构化、文档驱动的AI开发体验！

---

**记住：** BMAD的核心是文档和上下文。良好的文档 = 良好的AI上下文 = 更好的开发结果。

---

## OpenClaw环境中的BMAD代理交互指南

在OpenClaw环境中，与BMAD智能体的交互需要采用特定的模式。作为协调者，你需要驱动不同的BMAD代理来完成工作，而不是自己直接动手。

### 核心原则

1. **不要自己动手** - 你是协调者，不是执行者
2. **找对代理** - 根据任务类型选择正确的BMAD代理
3. **驱动代理工作** - 告诉代理要做什么，让代理去完成
4. **等待代理反馈** - 代理完成后会告诉你结果

### BMAD代理与职责

| 代理 | 命令前缀 | 主要职责 | 何时使用 |
|------|---------|---------|---------|
| **PM代理** | `bmad-agent-bmm-sm` | 创建PRD、分解Epic/Story、创建implementation文件 | 规划阶段、创建故事 |
| **Architect代理** | `bmad-agent-bmm-architect` | 创建架构文档、技术决策、实施就绪检查 | 架构设计阶段 |
| **DEV代理** | `bmad-agent-bmm-dev` | 实现Story、编写测试、代码审查 | 实现阶段 |
| **SM代理** | `bmad-agent-bmm-sm` | Sprint规划、Story创建、流程管理 | 敏捷管理 |
| **Analyst代理** | `bmad-agent-bmm-analyst` | 头脑风暴、市场研究、产品简介 | 分析阶段 |

### 典型工作流程

#### 场景1：创建Story

1. **确定需求** - 从epics.md或PM代理那里知道要创建哪个Story
2. **启动PM代理** - 使用`sessions_spawn`启动PM代理
3. **创建Story文件** - 告诉PM代理创建implementation文件
4. **等待完成** - PM代理会创建story文件并告诉你结果

**示例提示：**
```
作为BMAD PM代理，请创建Story 2.3的implementation文件。

项目目录：D:\BaiduSyncdisk\4-学习\100-项目\181_CICDRedo

首先读取：
1. _bmad-output/planning-artifacts/epics.md - 找到Story 2.3的详细内容
2. _bmad-output/planning-artifacts/architecture.md - 了解架构决策
3. _bmad-output/planning-artifacts/prd.md - 了解需求

然后创建implementation文件：
1. 文件路径：_bmad-output/implementation-artifacts/stories/2-3-[slug].md
2. 参考Story 2.1的格式
3. 包含：Story, Acceptance Criteria, Tasks/Subtasks, Dev Notes, Dev Agent Record, File List

创建完成后告诉我：创建的文件路径和主要内容。
```

#### 场景2：开发Story

1. **确定Story** - 知道要开发哪个Story
2. **启动DEV代理** - 使用`sessions_spawn`启动DEV代理
3. **开发Story** - 告诉DEV代理实现Story
4. **等待完成** - DEV代理会实现、写测试、报告结果

**示例提示：**
```
作为BMAD DEV代理（Amelia - Senior Software Engineer），请开发Story 2.3。

项目目录：D:\BaiduSyncdisk\4-学习\100-项目\181_CICDRedo

工作流程：
1. 读取Story 2.3文件：_bmad-output/implementation-artifacts/stories/2-3-[slug].md
2. 仔细阅读story文件，理解所有tasks/subtasks
3. 按照tasks/subtasks的顺序严格执行实现（不要跳过任何步骤，不要重新排序）
4. 为每个task/subtask编写完整的单元测试
5. 确保所有测试100%通过后才能标记任务为完成[x]
6. 每完成一个task/subtask后，运行完整测试套件
7. 在story文件的"Dev Agent Record"部分记录：实现了什么、创建了哪些测试、做了哪些技术决策
8. 更新story文件的"File List"部分，列出所有修改的文件

DEV代理原则：
- 超级简洁：用文件路径和AC ID说话
- 所有现有和新增测试必须100%通过
- 每个task/subtask必须有全面的单元测试覆盖
- 严格遵循story文件中的tasks/subtasks顺序

完成后请报告所有修改的文件和测试结果。
```

### sessions_spawn使用指南

在OpenClaw中使用`sessions_spawn`来启动BMAD代理：

```python
# 启动PM代理创建Story
sessions_spawn(
    task="作为BMAD PM代理，请创建Story 2.3的implementation文件...",
    label="Story 2.3 Creation",
    model="zai/glm-4.7"
)

# 启动DEV代理开发Story
sessions_spawn(
    task="作为BMAD DEV代理，请开发Story 2.3...",
    label="Story 2.3 Development",
    model="zai/glm-4.7"
)
```

### 错误处理

**问题：Story文件不存在**
- 可能Story还没有被创建
- 解决：先启动PM代理创建Story文件

**问题：Claude Code PTY错误**
- Claude Code需要PTY模式
- 解决：使用`exec pty:true`，但推荐用`sessions_spawn`

**问题：代理卡住或超时**
- 可能任务太复杂或模型问题
- 解决：kill会话，重新启动，简化提示

### 最佳实践

1. **一个任务一个会话** - 每次专注于一个明确的任务
2. **清晰的指令** - 明确告诉代理要做什么，不要让代理猜测
3. **提供上下文** - 列出需要读取的文件、项目目录等
4. **等待反馈** - 让代理完成后再进行下一步
5. **验证结果** - 代理完成后检查创建的文件是否符合预期

### 代理间协作示例

```
1. [我] 启动PM代理 → 创建Story 2.3
2. [PM] 创建implementation文件
3. [我] 启动DEV代理 → 开发Story 2.3
4. [DEV] 读取Story 2.3文件，实现代码和测试
5. [DEV] 完成开发，报告结果
6. [我] 启动DEV代理 → 进行Code Review（可选）
7. [DEV] 审查代码，给出反馈
```

### 注意事项

1. **不要直接修改代码** - 让DEV代理去实现
2. **不要跳过代理** - 即使简单的代码，也让DEV代理完成
3. **代理之间通过文档传递** - 文档是代理间的通信媒介
4. **保持上下文一致** - 确保每个代理读取相同的上下文文件
5. **记录决策** - 在Dev Agent Record中记录技术决策
