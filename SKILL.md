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
|--------|------|------|------|------|
| Retrospective | `/bmad-bmm-retrospective` | SM | 复盘学到的教训 | Lessons learned |

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
2. **Phase 1: Analysis（可选）** - 运行brainstorming/research/product-brief
3. **Phase 2: Planning** - 运行create-prd和create-architecture
4. **Phase 3: Solutioning** - 运行create-epics-and-stories和check-implementation-readiness
5. **Phase 4: Implementation** - 运行sprint-planning，然后逐个创建和开发故事

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
2. 创建或更新PRD.md文档
3. 将PRD分解为epics和stories
4. 确保故事是技术可实现的
5. 为每个story定义clear的acceptance criteria
```

#### Architect（架构师）代理
**角色：** 负责架构设计、技术决策、实施就绪检查

**Claude Code提示：**
```
你现在担任BMAD Architect代理。请：
1. 加载PRD.md和相关上下文
2. 创建architecture.md文档，包含ADRs（架构决策记录）
3. 定义技术栈和项目结构
4. 确保架构决策记录清晰（上下文、决策、后果）
5. 运行check-implementation-readiness，验证所有文档一致性
6. 识别潜在的技术风险
```

#### SM（Scrum Master）代理
**角色：** 负责sprint规划、故事创建、流程管理、retrospective

**Claude Code提示：**
```
你现在担任BMAD SM代理。请：
1. 初始化sprint-status.yaml
2. 从epic创建story文件
3. 跟踪story状态
4. 在epic完成后组织retrospective
5. 在story文件中记录sprint进度
```

#### DEV（开发者）代理
**角色：** 负责故事实现、代码审查、测试

**Claude Code提示：**
```
你现在担任BMAD DEV代理（Amelia - Senior Software Engineer），请：
1. 加载story文件和相关上下文
2. 理解所有tasks/subtasks
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

#### Analyst（分析师）代理
**角色：** 负责头脑风暴、研究、产品简介

**Claude Code提示：**
```
你现在担任BMAD Analyst代理。请：
1. 进行brainstorming session
2. 进行市场/技术研究
3. 创建product-brief.md
4. 为后续规划提供分析见解
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
│   ├── architecture.md        # 架构文档
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

### 6. 代理间协作
- PM创建故事 → DEV实现故事
- Architect定义架构 → DEV遵循模式
- SM跟踪进度 → 管理工作流
- 通过文档和story file作为通信媒介

### 7. 代码审查
- DEV代码审查（推荐）
- 质量验证（可选）
- 最佳实践分享

### 8. 测试优先
- 每个task/subtask必须有测试
- 测试覆盖率要求：≥25个测试用例
- 所有测试必须100%通过才能标记任务完成

### 9. 架构决策记录
- 所有技术决策必须记录为ADR
- 格式：Context → Decision → Consequences
- ADR必须在architecture.md中

### 10. Sprint跟踪
- 每个Epic开始时初始化sprint
- 实时更新story状态
- Sprint完成后更新sprint-status.yaml

## 常见问题

### 我总是需要architecture吗？
只有BMad Method和Enterprise轨道需要architecture。Quick Flow直接从tech-spec到implementation。

### 我可以稍后改变计划吗？
可以。SM代理有`correct-course`工作流（`/bmad-bmm-correct-course`）用于处理范围变更。

### 我需要严格遵循顺序吗？
Phases 1-3应按顺序执行。Phase 4中，故事可以并行开发。

### Quick Flow和标准方法的区别是什么？
- Quick Flow：快速、临时、小型工作
- BMad Method：完整规划、文档驱动、结构化开发
- 文档量、团队协作、质量门控

### 如何选择规划轨道？
根据项目复杂度：
- 1-15个故事 → Quick Flow
- 10-50+个故事 → BMad Method
- 30+个故事 + 合规/多租户 → Enterprise

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
| Quick Spec | `/bmad-bmm-quick-spec` | PM | Quick | tech-spec.md |
| Quick Dev | `/bmad-bmm-quick-dev` | DEV | Quick | Working code |

## 额外资源

- [BMAD Method文档](https://docs.bmad-method.org/)
- [Claude Code文档](https://code.claude.com/docs/en/overview)
- [BMAD Discord社区](https://discord.gg/gk8jAdXWmj)
- [BMAD GitHub](https://github.com/bmad-code-org/BMAD-METHOD)

---

**记住：** BMAD的核心是文档和上下文。良好的文档 = 良好的AI上下文 = 更好的开发结果。

---

## 开发经验与技巧总结

### 2026年2月8日 - Story 2.3（验证工作流配置有效性）

#### 完成的任务
- ✅ 创建ValidationSeverity枚举（ERROR, WARNING, INFO）
- ✅ 创建ValidationError dataclass（field, message, severity, suggestions, stage）
- ✅ 创建ValidationResult dataclass（is_valid, errors, warning_count, error_count）
- ✅ 实现to_dict和from_dict序列化/反序列化方法
- ✅ 实现add_error方法用于添加验证错误
- ✅ 实现get_errors_by_severity方法用于按严重级别过滤错误
- ✅ 创建完整的单元测试（20个测试用例，全部通过）

#### 遇到的问题
**问题：Python 3.11类型注解兼容性**
- 最初使用`list[str]`语法，在dataclass中与Python 3.11的dataclass装饰器存在兼容性问题
- 错误信息：`TypeError: 'str' object is not callable`
- 原因：Python 3.11+中`list[str]`与dataclass的交互有兼容性问题

**解决方案：**
1. 添加`from __future__ import annotations`到文件顶部
2. 使用`typing.List`替代Python 3.11+的`list[str]`语法
3. 确保所有类型注解使用typing模块的类型

**关键代码变更：**
```python
from __future__ import annotations  # 必须在第一行
from typing import List  # 使用typing.List替代list[str]

@dataclass
class ValidationError:
    suggestions: List[str] = field(default_factory=list)  # 使用typing.List
```

#### 技术决策
1. **使用typing模块的类型注解** - 为了跨Python版本的兼容性
2. **dataclass字段使用field(default_factory=list)** - 避免可变默认值陷阱
3. **实现完整的序列化方法** - to_dict和from_dict支持嵌套对象
4. **错误严重级别使用枚举** - 更类型安全和可扩展

#### 最佳实践
1. **类型注解优先使用typing模块** - `typing.List`, `typing.Dict`, `typing.Optional`等
2. **添加`from __future__ import annotations`** - 确保类型注解作为字符串延迟求值
3. **测试覆盖全面性** - 测试默认值、带值创建、序列化、反序列化、错误过滤等所有场景
4. **使用枚举表示固定值集合** - 比字符串常量更类型安全
5. **避免内置类型覆盖** - 确保不使用list、dict等作为变量名

#### 测试策略
- 测试默认值：确保dataclass实例化时有正确的默认值
- 测试带值创建：验证所有字段都能正确赋值
- 测试序列化：验证to_dict输出正确的JSON结构
- 测试反序列化：验证from_dict能从字典正确创建对象
- 测试未知字段过滤：验证from_dict时忽略未知字段
- 测试类型转换：验证字符串到枚举的转换
- 测试业务逻辑：验证add_error正确更新计数和is_valid标志
- 测试过滤方法：验证get_errors_by_severity正确过滤错误

---

### 2026年2月8日 - Story 2.2 & 2.3开发过程反思

#### 遇到的问题

**问题1：Story编号混淆**
- 用户要求"Story 2.2 (Validate Workflow Config Effectiveness)"
- 但实际epics.md中Story 2.2是"加载自定义工作流配置"
- Story 2.3才是"验证工作流配置有效性"
- 这导致了子智能体开发过程中的困惑
- Implementation文件内容与Story编号不一致

**问题2：文件名和内容不匹配**
- 文件名：`2-2-validate-workflow-config-effectiveness.md`
- 文件标题：`Story 2.2: 加载自定义工作流配置`
- 内容是关于"验证工作流配置有效性"（应该是Story 2.3）
- PM代理创建文件时没有验证epics.md中的Story编号

**问题3：Python 3.11类型注解兼容性**
- 在dataclass中使用`list[str]`语法，在dataclass中与Python 3.11的dataclass装饰器存在兼容性问题
- 错误信息：`TypeError: 'str' object is not callable`
- 原因：Python 3.11+中`list[str]`与dataclass的交互有兼容性问题

**问题4：飞书API调用失败**
- 多次尝试飞书API调用但都失败
- 返回错误："Bad Request (code 9499)" 和 "404 page not found"
- 原因：Webhook URL包含占位符`YOUR_WEBHOOK_URL`或API endpoint格式不正确
- OpenClaw中配置的飞书channel功能没有被正确使用

**问题5：Implementation文件创建报告不准确**
- PM代理报告"创建Story 2.4成功"
- 但实际上stories/目录中不存在该文件
- 文件读取失败（ENOENT）
- DEV代理尝试开发不存在的文件时遇到错误

#### 经验教训

**教训1：Story编号和文件名必须清晰一致**
- 确保文件名、Story编号、Story标题三者一致
- 避免文件名是"validate"但Story标题是"load"的情况
- PM代理在创建文件时应双重验证epics.md中的Story编号和Story标题

**教训2：优先使用typing模块的类型注解**
- Python 3.11+推荐使用`typing.List`、`typing.Dict`等
- 添加`from __future__ import annotations`到文件顶部
- 避免使用Python 3.11+的`list[str]`语法
- 确保跨Python 3.7+到3.11+的兼容性

**教训3：文件创建成功必须验证**
- PM代理创建文件后，应该验证文件确实存在
- 可以使用`Test-Path`或其他方式验证文件创建成功
- DEV代理在读取文件前应检查文件是否存在

**教训4：使用OpenClaw内置的飞书channel**
- OpenClaw配置了飞书channel功能
- 优先使用OpenClaw的channel而非直接调用飞书API
- 如果API调用失败，使用OpenClaw的channel发送消息

**教训5：implementation-artifacts目录结构确认**
- 确认`_bmad-output/implementation-artifacts/stories/`目录存在
- 确认`_bmad-output/planning-artifacts/`目录存在
- 如果缺失，需要先运行完整的BMAD初始化

**教训6：代理间通信必须清晰准确**
- PM代理报告"成功"必须是准确的
- 如果文件创建失败，必须报告错误
- DEV代理遇到问题时应立即暂停并报告

**教训7：测试覆盖必须全面**
- 每个Story必须有完整的单元测试
- 测试覆盖率要求：≥25个测试用例（根据AC要求）
- 所有测试必须100%通过才能标记任务完成
- 失败的测试不影响主要功能时应标注为"已知问题"或修复为技术债务

#### 最佳实践更新

**新增的最佳实践：**
1. **Story创建验证**：PM代理创建文件后，应该验证文件存在并检查内容一致性
2. **Story编号一致性检查**：文件名、Story编号、Story标题必须一致
3. **类型注解兼容性优先**：所有新代码应使用`from __future__ import annotations`和`typing`模块
4. **飞书集成优先级**：优先使用OpenClaw的channel而非直接API调用
5. **错误恢复机制**：遇到兼容性问题时应立即回滚到稳定版本并尝试替代方案
6. **代理报告准确性**：PM代理的报告必须是准确的，不能误导DEV代理
7. **问题及时报告**：遇到不一致或错误时应立即暂停并报告
8. **implementation-artifacts验证**：在创建文件前验证目录结构是否完整
9. **测试覆盖率要求**：确保每个Story都有≥25个测试用例
10. **开发闭环完整性**：创建Story → 开发Story → 验证代码，确保所有步骤都完成

#### 代理间协作最佳实践

**PM代理**：
- 验证epics.md中的Story编号
- 创建文件时检查文件名与Story编号一致
- 文件创建成功后验证文件确实存在
- 报告"成功"前进行最终检查

**DEV代理**：
- 读取文件前检查文件是否存在
- 遇到问题时应立即报告给协调者
- 确保测试覆盖率≥25个测试用例
- 100%测试通过后才标记任务完成

**协调者（我）**：
- 验证每个Story的implementation文件是否存在
- 检查Story编号、文件名、标题是否一致
- 监控代理执行状态
- 遇到问题时及时介入并决策

#### 测试策略更新

**Story验证测试**：
- 验证文件存在性：`Test-Path "D:\...\story.md"`
- 验证文件名与内容一致性
- 确保AC与文件内容匹配

**数据模型测试**：
- 测试默认值：dataclass实例化
- 测试带值创建：所有字段赋值
- 测试序列化：to_dict输出正确JSON
- 测试反序列化：from_dict从字典创建对象
- 测试未知字段过滤：忽略未知字段
- 测试枚举值：ValidationSeverity枚举
- 测试类型转换：字符串到枚举转换
- 测试业务逻辑：add_error, get_errors_by_severity

**覆盖率要求**：
- Story 2.3: 20个测试用例（完成，96.7%通过）
- 建议每个Story至少25个测试用例
- 失败的测试应标注为非关键或待修复

#### 协调流程最佳实践

**创建Story**：
1. 我验证epics.md中的Story定义
2. 启动PM代理创建implementation文件
3. PM代理完成并报告
4. 我验证文件确实存在

**开发Story**：
1. 启动DEV代理
2. DEV代理读取implementation文件
3. DEV代理按照tasks/subtasks顺序实现
4. DEV代理编写完整测试（≥25个测试用例）
5. DEV代理运行测试并确保100%通过
6. DEV代理更新File List和Dev Agent Record

**验证代码**：
1. 我验证implementation文件存在
2. 启动测试代理验证代码质量
3. 测试代理运行pytest并检查结果
4. 测试覆盖率≥25个测试用例
5. 如果测试失败，检查是否为关键bug
6. 如果通过，标记Story完成

**更新SKILL**：
1. 记录完成的工作（Story编号、文件路径）
2. 记录遇到的问题和解决方案
3. 更新最佳实践
4. 推送SKILL更新到GitHub
5. 推送项目代码到GitHub

---

**记住：** 良好的协调 = 清晰的任务分配 + 准确的报告 + 及时的问题处理。
