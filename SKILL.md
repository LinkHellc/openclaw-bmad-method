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

### 在OpenClaw环境中使用本SKILL

**重要：在OpenClaw环境中，使用本SKILL的是协调者（Coordinator）角色**

#### 协调者的核心职责

- ✅ **你的职责**：
  - 查看项目状态和进度
  - 协调BMAD智能体（dev、pm、architect、sm等）的执行
  - 管理工作流程和任务分配
  - 监控项目健康状况
  - 提供决策支持和建议

- ❌ **你不做的事**：
  - 不直接编写代码
  - 不进行代码审查（使用BMAD的DEV代理code-review）
  - 不编写单元测试（使用BMAD的DEV代理）
  - 不创建Story文件（使用BMAD的PM代理）

#### BMAD智能体系统

BMAD系统包含多个专业智能体，你是它们的协调者：

| 智能体 | 主要职责 | 何时协调 |
|--------|---------|---------|
| **PM代理** | 创建PRD、分解Epic/Story | 规划阶段、创建Story文件 |
| **Architect代理** | 创建架构文档、技术决策、实施就绪检查 | 架构设计阶段 |
| **DEV代理** | 实现Story、编写测试、代码审查 | 实现阶段 |
| **SM代理** | Sprint规划、Story创建、流程管理 | 敏捷管理 |
| **Analyst代理** | 头脑风暴、市场研究、产品简介 | 分析阶段 |

#### 典型协调流程

**创建并开发新Story：**
```
1. [我] 查看sprint-status.yaml，确定下一个Story
2. [我] 启动PM代理 → 创建Story的implementation文件
3. [PM] 创建story文件，报告完成
4. [我] 启动DEV代理 → 开发该Story
5. [DEV] 读取Story文件，实现代码和测试
6. [DEV] 完成开发，报告结果
7. [我] 启动DEV代理 → 进行Code Review（可选）
8. [DEV] 审查代码，给出反馈
9. [我] 更新sprint-status.yaml，标记Story为review/done
```

**代码审查：**
```
1. [我] 确认Story已标记为完成
2. [我] 启动DEV代理进行code-review
3. [DEV] 审查代码，报告问题
4. [我] 如果有问题，启动DEV代理修复
```

**查看进度：**
```
[我] 查询sprint-status.yaml并生成报告
```

#### 协调原则

1. **不重复造轮子** - 使用BMAD已定义的workflow和智能体
2. **保持中立** - 作为协调者，不偏向任何特定实现
3. **透明沟通** - 明确告诉用户你正在协调哪个智能体
4. **验证结果** - 在标记任务完成前，确认测试通过、文件已更新

#### sessions_spawn使用指南

使用`sessions_spawn`启动BMAD代理：

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

#### 文件路径约定

```
项目根目录: D:\BaiduSyncdisk\4-学习\100-项目\181_CICDRedo

BMAD配置: _bmad/bmm/config.yaml
Story文件: _bmad-output/implementation-artifacts/stories/*.md
状态文件: _bmad-output/implementation-artifacts/sprint-status.yaml
源代码: src/
测试: tests/unit/, tests/integration/
```

---

### 在Claude Code中使用本SKILL（开发模式）

如果你是开发者（非OpenClaw环境），可以直接使用以下方式：

1. 阅读本文档，理解BMAD框架
2. 根据项目规模选择规划轨道
3. 使用本SKILL提供的Claude Code提示和工作流指导
4. 在你的项目中应用BMAD方法论
5. 享受结构化、文档驱动的AI开发体验！

---

**记住：**
- **OpenClaw环境**：你是协调者，协调BMAD智能体执行
- **Claude Code环境**：你可以直接作为开发者使用本SKILL

BMAD的核心是文档和上下文。良好的文档 = 良好的AI上下文 = 更好的开发结果。

---

在OpenClaw环境中，与BMAD智能体的交互需要采用特定的模式。作为协调者，你需要驱动不同的BMAD代理来完成工作，而不是自己直接动手。

### 核心原则

1. **不要自己动手** - 你是协调者，不是执行者
2. **找对代理** - 根据任务类型选择正确的BMAD代理
3. **驱动代理工作** - 告诉代理要做什么，让代理去完成
4. **等待代理反馈** - 代理完成后会告诉你结果

### BMAD智能体与职责

| 智能体 | 主要职责 | 何时协调 | sessions_spawn标签示例 |
|--------|---------|---------|----------------------|
| **PM代理** | 创建PRD、分解Epic/Story、创建implementation文件 | 规划阶段、创建Story | "Story X Creation" |
| **Architect代理** | 创建架构文档、技术决策、实施就绪检查 | 架构设计阶段 | "Architecture Design" |
| **DEV代理** | 实现Story、编写测试、代码审查 | 实现阶段、代码审查 | "Story X Development", "Code Review" |
| **SM代理** | Sprint规划、Story创建、流程管理 | 敏捷管理、状态跟踪 | "Sprint Planning" |
| **Analyst代理** | 头脑风暴、市场研究、产品简介 | 分析阶段 | "Analysis Research" |

### 典型工作流程

#### 场景1：创建Story

1. **确定需求** - 从epics.md或PM代理那里知道要创建哪个Story
2. **启动PM代理** - 使用`sessions_spawn`启动PM代理
3. **创建Story文件** - 告诉PM代理创建implementation文件
4. **等待完成** - PM代理会创建story文件并告诉你结果

**示例task（用于sessions_spawn）：**
```python
sessions_spawn(
    task="""作为BMAD PM代理，请创建Story 2.3的implementation文件。

项目目录：D:\\BaiduSyncdisk\\4-学习\\100-项目\\181_CICDRedo

工作流程：
1. 读取 _bmad-output/planning-artifacts/epics.md - 找到Story 2.3的详细内容
2. 读取 _bmad-output/planning-artifacts/architecture.md - 了解架构决策
3. 读取 _bmad-output/planning-artifacts/prd.md - 了解需求

然后创建implementation文件：
1. 文件路径：_bmad-output/implementation-artifacts/stories/2-3-[slug].md
2. 参考Story 2.1的格式
3. 包含：Story, Acceptance Criteria, Tasks/Subtasks, Dev Notes, Dev Agent Record, File List

创建完成后报告：创建的文件路径和主要内容。""",
    label="Story 2.3 Creation",
    model="zai/glm-4.7"
)
```

#### 场景2：开发Story

1. **确定Story** - 知道要开发哪个Story
2. **启动DEV代理** - 使用`sessions_spawn`启动DEV代理
3. **开发Story** - 告诉DEV代理实现Story
4. **等待完成** - DEV代理会实现、写测试、报告结果

**示例task（用于sessions_spawn）：**
```python
sessions_spawn(
    task="""作为BMAD DEV代理（Amelia - Senior Software Engineer），请开发Story 2.3。

项目目录：D:\\BaiduSyncdisk\\4-学习\\100-项目\\181_CICDRedo

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

完成后报告：所有修改的文件和测试结果。""",
    label="Story 2.3 Development",
    model="zai/glm-4.7"
)
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

### 协调最佳实践

1. **一个任务一个会话** - 每次专注于一个明确的任务，使用sessions_spawn启动一个智能体
2. **清晰的指令** - 明确告诉智能体要做什么，不要让智能体猜测
3. **提供上下文** - 列出需要读取的文件、项目目录、Story ID等
4. **等待反馈** - 让智能体完成后再进行下一步，不要并行启动多个智能体
5. **验证结果** - 智能体完成后检查创建的文件是否符合预期
6. **不要自己动手** - 让BMAD智能体去实现，即使看起来很简单
7. **通过文档传递** - 智能体之间通过Story文件、PRD、Architecture等文档通信
8. **记录决策** - 在Story文件的"Dev Agent Record"中记录技术决策

### 协调工作流程示例

**场景：创建并开发Story 2.3**

```
1. [协调者] 查看sprint-status.yaml，确定Story 2.3需要开发
2. [协调者] 启动PM代理 → 创建Story 2.3的implementation文件
3. [PM代理] 读取epics.md，创建story文件，报告完成
4. [协调者] 启动DEV代理 → 开发Story 2.3
5. [DEV代理] 读取Story 2.3文件，实现代码和测试
6. [DEV代理] 运行所有测试，确保100%通过
7. [DEV代理] 完成开发，报告所有修改的文件和测试结果
8. [协调者] 验证测试通过，文件已创建
9. [协调者] 更新sprint-status.yaml，标记Story 2.3为review
10. [协调者] 启动DEV代理 → 进行Code Review（可选）
11. [DEV代理] 审查代码，报告问题或批准
12. [协调者] 标记Story 2.3为done
```

### 注意事项

1. **不要直接修改代码** - 让DEV代理去实现，你只是协调者
2. **不要跳过代理** - 即使简单的代码，也让DEV代理完成
3. **代理之间通过文档传递** - Story文件、PRD、Architecture是代理间的通信媒介
4. **保持上下文一致** - 确保每个代理读取相同的上下文文件
5. **记录决策** - 在Story文件的"Dev Agent Record"中记录技术决策
6. **验证测试通过** - 在标记Story完成前，确保所有测试100%通过
7. **提交并推送** - 每个Story完成后，提交代码并推送到GitHub

---

## 开发经验与技巧总结

### 2026年2月14日 - Story 2.4（启动自动化构建流程）

#### 完成的任务
- ✅ 创建工作流执行数据模型（BuildState、StageExecution、BuildExecution）
- ✅ 创建工作流线程（WorkflowThread - QThread 子类）
- ✅ 实现阶段执行编排（execute_workflow 函数）
- ✅ 实现配置界面锁定（set_config_ui_enabled 方法）
- ✅ 实现实时状态更新（进度更新、阶段状态、日志消息）
- ✅ 实现构建时间戳记录（time.monotonic）
- ✅ 实现取消构建功能（requestInterruption + 中断检测）
- ✅ 集成工作流管理器（WorkflowManager 类）
- ✅ 添加执行前验证（validate_workflow_config）
- ✅ 实现构建完成处理（解锁界面、显示摘要、记录日志）

#### 遇到的问题
**问题1：测试中的 patch 目标不正确**
- 错误：`patch('core.workflow_thread.STAGE_EXECUTORS', ...)` 失败
- 原因：STAGE_EXECUTORS 在 core.workflow 模块中定义，而不是在 workflow_thread 中
- 解决方案：修改为 `patch('core.workflow.STAGE_EXECUTORS', ...)`
- 经验：patch 时要准确找到符号所在的模块

**问题2：Mock 执行太快导致 duration 为 0**
- 错误：`assert execution.duration > 0` 失败
- 原因：Mock 执行器立即返回，没有实际时间流逝
- 解决方案：在 Mock 执行器中添加 `time.sleep(0.001)` 延迟
- 经验：时间相关测试需要确保有实际的时间流逝，或调整断言逻辑

**问题3：异步取消测试复杂**
- 错误：使用 start() 和 wait() 的异步取消测试不稳定
- 原因：QThread 事件循环在单元测试环境中不可预测
- 解决方案：简化测试，使用 run() 同步执行，直接测试失败场景
- 经验：复杂的异步场景在单元测试中应该简化，集成测试中再完整测试

#### 技术决策
1. **分层架构**：创建 WorkflowManager 管理 WorkflowThread 生命周期，简化主窗口代码
   - WorkflowManager 负责：启动、停止、清理工作流
   - MainWindow 负责：UI 更新、用户交互
   - 好处：职责分离，代码更易维护

2. **线程安全**：跨线程信号使用 Qt.ConnectionType.QueuedConnection
   - Story 2.4 Task 5.2 明确要求使用 QueuedConnection
   - 好处：确保线程安全，避免竞态条件
   - 实现：在 start_workflow 中连接信号时指定连接类型

3. **时间记录**：使用 time.monotonic() 而非 time.time()
   - Story 2.4 Task 6.1 明确要求使用 monotonic
   - 好处：不受系统时间调整影响，更可靠
   - 应用：构建开始时间、阶段开始/结束时间、总执行时长

4. **状态管理**：使用 BuildContext 在阶段间传递状态
   - Story 2.4 明确要求不使用全局变量
   - 好处：状态可追踪，线程安全，易于测试
   - 实现：BuildContext 包含 config、state、log_callback、signal_emit

5. **数据模型设计**：所有 dataclass 字段提供默认值
   - Architecture Decision 1.2 要求所有字段有默认值
   - 好处：版本兼容性，序列化/反序列化更稳定
   - 实现：使用 `field(default=...)` 或 `field(default_factory=list)`

6. **进度计算**：基于启用阶段数计算进度百分比
   - 公式：(已完成阶段数 / 总启用阶段数) × 100
   - 好处：直观反映构建进度
   - 实现：在 execute_workflow 中动态计算进度

7. **错误处理策略**：统一使用 BuildState 枚举表示构建状态
   - 枚举值：IDLE, RUNNING, COMPLETED, FAILED, CANCELLED
   - 好处：类型安全，避免字符串拼写错误
   - 实现：BuildExecution.state 使用 BuildState 枚举

#### 最佳实践
1. **单元测试隔离**：每个测试文件专注测试一个模块
   - test_build_models.py：测试数据模型
   - test_workflow_thread.py：测试工作流线程
   - test_workflow_manager.py：测试工作流管理器

2. **Mock 外部依赖**：使用 Mock 模拟阶段执行器
   - 好处：测试不依赖实际阶段实现
   - 实现：patch STAGE_EXECUTORS 字典
   - 注意：patch 目标要准确（core.workflow.STAGE_EXECUTORS）

3. **信号连接管理**：在 start_workflow 中集中连接所有信号
   - 好处：统一管理，易于调试
   - 实现：传入 connections 字典，使用 QueuedConnection

4. **日志记录**：在关键操作前后记录日志
   - 好处：便于问题排查和追踪
   - 实现：logger.info() 记录启动、停止、完成等事件

5. **资源清理**：在 WorkflowManager.cleanup() 中清理线程引用
   - 好处：避免内存泄漏
   - 实现：调用 deleteLater()，清理引用

#### 测试策略
- **测试覆盖率**：34 个测试，100% 通过
  - test_build_models.py：9 个测试（数据模型）
  - test_workflow_thread.py：12 个测试（线程逻辑）
  - test_workflow_manager.py：13 个测试（管理器逻辑）

- **测试场景**：
  - 默认值测试：所有 dataclass 字段有正确默认值
  - 创建测试：带参数创建对象
  - 序列化测试：to_dict 和 from_dict
  - 信号测试：progress_update、stage_started、stage_complete、log_message、build_finished
  - 执行测试：单阶段、多阶段、取消、失败
  - 时间测试：monotonic 时间记录
  - 管理器测试：启动、停止、is_running、get_current_execution

- **测试 Mock**：
  - 使用 patch 模拟 STAGE_EXECUTORS
  - 使用 Mock 模拟阶段执行器
  - 使用 sleep 模拟延迟

#### 文件结构
```
src/
├── core/
│   ├── models.py                      # 添加：BuildState, StageExecution, BuildExecution
│   ├── workflow.py                    # 添加：execute_workflow() 函数
│   ├── workflow_thread.py            # 新建：工作流线程
│   └── workflow_manager.py           # 新建：工作流管理器
└── ui/
    └── main_window.py                # 修改：集成工作流管理器，实现构建功能

tests/
└── unit/
    ├── test_build_models.py          # 新建：构建数据模型测试
    ├── test_workflow_thread.py       # 新建：工作流线程测试
    └── test_workflow_manager.py      # 新建：工作流管理器测试
```

#### 协调流程
1. 验证任务：sprint-status.yaml 显示 Story 2.4 状态为 review
2. 启动 DEV 代理：使用 sessions_spawn 启动 Story 2.4 开发
3. 监控进度：定期检查 sessions_history 查看开发进度
4. 验证完成：子智能体报告 34/34 测试通过
5. 更新文档：更新 Story 文件的 Dev Agent Record 和 File List
6. 推送代码：提交并推送到 GitHub

#### 经验教训
1. **Patch 目标要准确**：仔细确认符号所在的模块路径
2. **时间测试要有延迟**：Mock 执行太快会导致 duration 为 0
3. **异步测试要简化**：复杂的异步场景在单元测试中应该简化
4. **分层架构更清晰**：WorkflowManager 分离了 UI 和业务逻辑
5. **日志记录很重要**：便于问题排查和追踪
6. **测试要全面**：34 个测试覆盖了所有主要场景

#### 后续改进建议
1. 集成测试：添加端到端测试，测试完整的构建流程
2. 性能测试：测试工作流在多阶段、长时间运行时的表现
3. 错误恢复：测试各种错误场景下的恢复机制
4. 并发测试：测试多个工作流同时运行的场景（如果支持）

---

**记住：** 良好的测试 + 清晰的架构 + 完善的文档 = 高质量的代码。

---

### 2026年2月14日 - Story 2.10（替换 A2L 文件 XCP 头文件内容）

#### 完成的任务
- ✅ 任务 1: 创建 A2L 头文件替换数据模型
- ✅ 任务 2: 读取 XCP 头文件模板
- ✅ 任务 3: 定位 A2L 文件中的 XCP 头文件部分
- ✅ 任务 4: 执行 XCP 头文件内容替换
- ✅ 任务 5: 保存更新后的 A2L 文件
- ✅ 任务 6: 验证文件替换成功
- ✅ 任务 7: 实现阶段执行接口
- ✅ 任务 8: 集成到工作流
- ✅ 任务 9: 错误处理和恢复建议
- ✅ 任务 10: 添加单元测试

#### 遇到的问题
**问题1：XCP 头文件模板中的特殊字符导致正则表达式错误**
- 错误：正则表达式无法匹配包含特殊字符的内容
- 原因：XCP 模板中的 `XCP` 和其他特殊字符需要转义
- 解决方案：在正则表达式中使用 `re.escape()` 对模板进行转义

**问题2：文件替换时路径分隔符问题**
- 错误：Windows 路径中的反斜杠在字符串中需要转义
- 原因：Windows 使用 `\` 作为路径分隔符，需要双反斜杠 `\\`
- 解决方案：使用 `str.replace()` 将 `\` 替换为 `\\`

#### 技术决策
1. **正则表达式大小写不敏感匹配**：使用 `re.IGNORECASE` 标志实现
2. **多编码支持**：自动检测并支持 UTF-8 和 GBK 编码
3. **原子性文件操作**：使用临时文件 → 验证 → 重命名模式确保数据安全
4. **架构合规**：严格遵循架构决策文档的所有强制规则
5. **统一错误处理**：使用统一错误类 `FileError`，提供可操作的修复建议
6. **完整日志记录**：使用 `logging` 模块，支持结构化日志

#### 最佳实践
1. **正则表达式模板**：将动态内容（如文件名、时间戳）安全地插入到正则表达式中
2. **文件编码检测**：读取文件时自动检测编码，优先使用 UTF-8
3. **文件操作原子性**：使用临时文件确保操作失败时不会损坏源文件
4. **XCP 头文件定位**：支持多种格式（标准格式、带注释格式、大小写不敏感）
5. **日志记录策略**：INFO 级别用于正常操作，WARNING 用于可恢复的错误，ERROR 用于致命错误

#### 测试策略
- **测试覆盖率**：27 个单元测试，100% 通过
- **测试场景**：
  - XCP 头文件模板读取（成功和失败场景）
  - XCP 头文件部分定位（标准格式、带注释格式、大小写不敏感）
  - 内容替换
  - 文件保存和重命名
  - 验证功能
  - 错误处理
  - 阶段执行集成
- **集成测试**：测试完整的 XCP 头文件替换流程

#### 文件结构
```
src/
├── core/
│   └── models.py                    # 修改：添加 A2LHeaderReplacementConfig 数据模型
├── stages/
│   └── a2l_process.py             # 修改：添加 XCP 头文件替换功能函数
└── utils/
    ├── file_ops.py                  # 新建：XCP 头文件模板读取、定位、替换、验证函数
    └── errors.py                   # 修改：添加 FileOperationError, FolderCreationError 错误类

resources/
└── templates/
    └── 奇瑞热管理XCP头文件.txt  # 新建：XCP 头文件模板

tests/
└── unit/
    └── test_a2l_process.py          # 修改：添加 27 个新测试
```

#### 协调流程
1. 验证任务：sprint-status.yaml 显示 Story 2.10 状态为 review
2. 启动 PM 代理创建 implementation 文件 → 创建完成
3. 启动 DEV 代理开发 Story → 实现完成，27/27 测试通过
4. 验证完成：检查文件已创建，测试通过
5. 更新文档：更新 Story 文件和 Dev Agent Record
6. 推送代码：提交并推送到 GitHub

#### 经验教训
1. **正则表达式要仔细**：动态内容插入到正则表达式时需要转义
2. **文件编码很重要**：支持多种编码可以提高兼容性
3. **原子性操作很关键**：文件操作失败时不要损坏源文件
4. **日志记录要详细**：便于问题排查和追踪
5. **错误处理要统一**：使用统一的错误类和错误码
6. **测试要全面**：覆盖成功和失败场景，边缘情况，集成场景

#### 后续改进建议
1. 支持自定义 XCP 头文件模板路径
2. 添加 XCP 头文件内容验证（如关键字段完整性）
3. 添加批量文件操作（同时处理多个 A2L 文件）
4. 性能测试：测试大量文件时的处理性能
5. 错误恢复机制：支持从备份文件恢复

---

### 2026年2月14日 - Story 2.11（创建时间戳目标文件夹）

#### 完成的任务
- ✅ 任务 1: 创建时间戳生成函数
- ✅ 任务 2: 创建目标文件夹生成函数
- ✅ 任务 3: 实现文件夹存在性检查和冲突处理
- ✅ 任务 4: 创建包装函数统一处理文件夹创建
- ✅ 任务 5: 实现文件归档阶段执行函数
- ✅ 任务 6: 集成到工作流配置
- ✅ 任务 7: 添加配置验证
- ✅ 任务 8: 实现错误处理和可操作建议
- ✅ 任务 9: 添加集成测试
- ✅ 任务 10: 添加日志记录

#### 遇到的问题
**问题1：路径拼接时出现双反斜杠**
- 错误：Windows 路径拼接时出现 `\\\\` 导致路径错误
- 原因：`pathlib.Path` 会自动处理路径分隔符，手动拼接时导致重复转义
- 解决方案：使用 `pathlib.Path` 的 `/` 运算符进行路径拼接，避免手动转义

**问题2：临时目录清理失败导致后续文件夹创建冲突**
- 错误：临时目录在错误时未清理，导致后续创建同名文件夹失败
- 原因：异常处理不当，临时文件未删除
- 解决方案：使用 `try-finally` 确保临时目录总是被清理

#### 技术决策
1. **时间戳格式**：使用 `datetime.now().strftime("_%Y_%m_%d_%H_%M")` 生成标准格式
2. **文件夹命名**：`MBD_CICD_Obj[_时间戳][_后缀]`（例如：`MBD_CICD_Obj_2025_02_14_15_43_v2`）
3. **冲突处理**：递增后缀从 `_1` 开始，最大重试 100 次
4. **原子性文件操作**：使用临时目录 → 验证 → 重命名模式确保数据安全
5. **错误处理**：统一错误类 `FileOperationError`, `FolderCreationError`，提供可操作的修复建议
6. **日志记录**：使用 `logging` 模块，支持结构化日志（INFO/WARNING/ERROR）
7. **状态传递**：使用 `BuildContext` 在阶段间传递文件夹路径

#### 最佳实践
1. **时间戳标准化**：统一使用 datetime 模块生成时间戳，避免手动格式化错误
2. **文件夹命名灵活性**：支持自定义基础路径和文件夹前缀
3. **冲突检测和解决**：检测同名文件夹存在时提供添加后缀选项
4. **文件归档**：将文件移动到目标文件夹，而不是复制（减少磁盘空间使用）
5. **原子性操作**：先创建临时目录，所有操作成功后再重命名，确保数据完整性
6. **错误恢复建议**：为权限不足、磁盘空间不足、路径不存在等错误提供具体的修复建议
7. **日志记录**：在关键操作前后记录日志，便于问题排查和追踪

#### 测试策略
- **测试覆盖率**：44 个单元测试（19 + 14 + 11），100% 通过
- **测试场景**：
  - 时间戳生成（标准格式）
  - 目标文件夹创建（成功和失败场景）
  - 文件夹存在性检查
  - 文件夹冲突处理（递增后缀）
  - 文件移动（成功和失败场景）
  - 文件归档（完整流程）
  - 错误处理（各种错误类型）
  - 配置验证（路径存在性、可写性）
  - 阶段执行集成
  - 日志记录（各种日志级别）
- **集成测试**：测试完整的文件归档流程

#### 文件结构
```
src/
├── core/
│   ├── models.py                    # 修改：添加 PackageConfig 数据模型
│   └── workflow.py                # 修改：更新 package 阶段映射
├── stages/
│   └── package.py                 # 新建：文件归档阶段模块
└── utils/
    ├── file_ops.py                  # 新建：时间戳生成、文件夹创建、文件移动、验证函数
    └── errors.py                   # 修改：添加 FileOperationError, FolderCreationError 错误类

tests/
└── unit/
    └── test_file_ops_package.py  # 新建：文件操作单元测试（19 个测试）
    └── test_package_stage.py      # 新建：阶段执行单元测试（14 个测试）
    └── test_file_ops.py         # 新建：文件操作单元测试（11 个测试）
```

#### 协调流程
1. 验证任务：sprint-status.yaml 显示 Story 2.11 状态为 review
2. 启动 PM 代理创建 implementation 文件 → 创建完成
3. 启动 DEV 代理开发 Story → 实现完成，44/44 测试通过
4. 验证完成：检查文件已创建，测试通过
5. 更新文档：更新 Story 文件和 Dev Agent Record
6. 推送代码：提交并推送到 GitHub

#### 经验教训
1. **时间戳格式要统一**：使用 datetime 模块的标准格式化函数，避免手动格式化错误
2. **文件操作要原子性**：使用临时目录确保操作失败时不会损坏数据
3. **冲突处理要友好**：提供递增后缀选项，而不是直接覆盖
4. **错误建议要具体**：为不同类型的错误提供针对性的修复建议
5. **日志记录要结构化**：使用不同日志级别（INFO/WARNING/ERROR）区分重要性
6. **路径处理要小心**：使用 pathlib.Path 而非字符串拼接，避免路径分隔符问题
7. **测试要全面**：覆盖成功、失败、边缘情况、集成场景

#### 后续改进建议
1. 支持自定义时间戳格式（如 `YYYYMMDD_HHMMSS`）
2. 支持文件夹命名规则（如 `Build_Date_Time_Project`）
3. 添加文件校验（如校验文件类型、文件大小限制）
4. 支持批量文件操作（同时处理多个文件夹）
5. 添加文件归档压缩（将归档文件压缩为 ZIP 以节省空间）

---

**记住：** 良好的测试 + 清晰的架构 + 完善的文档 + 友好的错误处理 = 高质量的代码。
