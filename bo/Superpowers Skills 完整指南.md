# Superpowers Skills 指南：从入门到精通

欢迎阅读 **Superpowers 指南**！

本文档融合了“技能全解”与“深度教程”，旨在为您提供一份最完整、最实战的开发手册。我们将按照软件开发的生命周期，带您从**场景入门**（什么时候用）深入到**核心流程**（具体怎么做）。

---

## 目录

1.  **第一阶段：启动与规划 (The Start)**
    - [Using Superpowers (超能力总纲)](#1-using-superpowers)
    - [Brainstorming (头脑风暴)](#2-brainstorming)
    - [Writing Plans (编写计划)](#3-writing-plans)
    - [Using Git Worktrees (工作区隔离)](#4-using-git-worktrees)
2.  **第二阶段：核心开发 (The Grind)**
    - [Test Driven Development (测试驱动开发)](#5-test-driven-development)
    - [Systematic Debugging (系统性调试)](#6-systematic-debugging)
    - [Verification Before Completion (完成前验证)](#7-verification-before-completion)
3.  **第三阶段：复杂任务管理 (The Manager)**
    - [Dispatching Parallel Agents (分发并行 Agent)](#8-dispatching-parallel-agents)
    - [Subagent Driven Development (子 Agent 驱动开发)](#9-subagent-driven-development)
    - [Executing Plans (执行计划)](#10-executing-plans)
4.  **第四阶段：审查与交付 (The Review & Finish)**
    - [Requesting Code Review (请求代码审查)](#11-requesting-code-review)
    - [Receiving Code Review (接收代码审查)](#12-receiving-code-review)
    - [Finishing a Development Branch (完成分支)](#13-finishing-a-development-branch)
5.  **第五阶段：元技能与扩展 (Meta Skills)**
    - [Find Skills (查找技能)](#14-find-skills)
    - [Writing Skills (编写技能)](#15-writing-skills)

---

## 第一阶段：启动与规划 (The Start)

在写任何代码之前，先想清楚、计划好。

### <a id="1-using-superpowers"></a>1. `using-superpowers` (超能力总纲)

- **我是谁**：这是所有技能的"大脑"，自动驾驶仪。
- **什么时候用**：**始终在线**。你不需要显式调用。
- **我会做什么**：
  - 分析你的请求。
  - 自动路由到合适的技能（如 Brainstorming 或 TDD）。
  - 建立“先查技能，再行动”的原则。

### <a id="2-brainstorming"></a>2. `brainstorming` (头脑风暴)

- **我是谁**：你的产品经理 + 架构师。
- **什么时候用**：当你只有一个模糊的想法（"我想做一个贪吃蛇游戏"）或者想加个新功能但不知道怎么设计时。
- **一句话指令**：
  > "Brainstorm ideas for a Snake game."
- **核心流程**：
  1.  **理解现状**：检查项目当前状态（文件、文档）。
  2.  **逐步提问**：一次只问一个问题，优先使用选择题，理清目的和约束。
  3.  **探索方案**：提出 2-3 种方案并分析权衡，给出推荐意见。
  4.  **设计呈现**：分段（200-300字）展示设计，每段确认是否符合预期。
  5.  **文档化**：将验证后的设计写入 `docs/plans/YYYY-MM-DD-<topic>-design.md`。

### <a id="3-writing-plans"></a>3. `writing-plans` (编写计划)

- **我是谁**：你的技术主管（Tech Lead）。
- **什么时候用**：需求明确了，准备动手前。
- **一句话指令**：
  > "Based on our brainstorm, use writing-plans to create a detailed implementation plan."
- **核心流程**：
  1.  **创建计划文件**：在 `docs/plans/` 下创建 Markdown 文件。
  2.  **结构化内容**：
      - **Goal**: 目标简述。
      - **Proposed Plan**: 详细步骤，包含文件路径、验证命令。
  3.  **用户确认**：必须获得用户明确批准。
  4.  **执行模式**：简单任务直接执行，复杂任务交给 `executing-plans`。
- **关键原则**：每一步都必须包含“如何验证完成”（如运行特定测试）。

### <a id="4-using-git-worktrees"></a>4. `using-git-worktrees` (工作区隔离)

- **我是谁**：你的洁癖助手。
- **什么时候用**：当你正在修 Bug，突然要开发新功能；或者想尝试一个破坏性的重构时。
- **一句话指令**：
  > "Start a new feature 'login-page' using git worktrees."
- **核心流程**：
  1.  **选择目录**：推荐在 `../<project-name>-<feature-name>` 创建工作树。
  2.  **创建工作树**：`git worktree add ../my-feature -b feature/my-feature`
  3.  **迁移环境**：复制 `.env` 等必要配置。
  4.  **验证环境**：运行测试确保新环境正常。

---

## 第二阶段：核心开发 (The Grind)

这是你每天用的最多的技能，也是高质量代码的保证。

### <a id="5-test-driven-development"></a>5. `test-driven-development` (TDD / 测试驱动开发)

- **我是谁**：你的严师（必须先考试，再上课）。
- **什么时候用**：**每一次**写功能代码或修 Bug 时。
- **一句话指令**：
  > "Implement the sorting function using TDD."
- **核心流程（红-绿-重构）**：
  1.  **RED (红)**：编写一个失败的测试。
      - _验证_：确认它因为“功能缺失”而失败。
  2.  **GREEN (绿)**：编写最小量的实现代码让测试通过。
      - _验证_：确认通过。
  3.  **REFACTOR (重构)**：优化代码，保持测试通过。
- **铁律**：禁止“先写代码后补测试”。

### <a id="6-systematic-debugging"></a>6. `systematic-debugging` (系统性调试)

- **我是谁**：你的福尔摩斯。
- **什么时候用**：代码报错了、测试挂了、API 500 了。
- **一句话指令**：
  > "The login test is failing. Use systematic-debugging to fix it."
- **核心流程**：
  1.  **复现**：创建一个最小复现案例（失败的测试）。
  2.  **分析**：观察现象，提出假设。
  3.  **验证假设**：通过日志、修改代码等方式验证假设。
  4.  **修复**：实施修复方案。
  5.  **验证修复**：运行复现测试确保通过，并运行回归测试。
- **禁止**：盲目尝试修复（Shotgun debugging）。

### <a id="7-verification-before-completion"></a>7. `verification-before-completion` (完成前验证)

- **我是谁**：你的质检员。
- **什么时候用**：你想说"我做完了"之前。
- **一句话指令**：
  > "I think I'm done. Verify everything."
- **核心流程**：
  1.  **运行证据**：必须执行命令（测试、Linter、构建）。
  2.  **检查输出**：确认输出为成功（exit code 0）。
  3.  **自我质询**：真的解决了吗？有没有引入新问题？
  4.  **提交**：只有在验证通过后，才进行代码提交或汇报。

---

## 第三阶段：复杂任务管理 (The Manager)

当你面对的是一个庞大的项目，而不是一个小功能时。

### <a id="8-dispatching-parallel-agents"></a>8. `dispatching-parallel-agents` (分发并行 Agent)

- **我是谁**：你的包工头。
- **什么时候用**：有 5 个独立的 API 接口要写，它们互不依赖。
- **一句话指令**：
  > "We need to create 5 utility functions. Dispatch parallel agents to do them at the same time."
- **核心流程**：
  1.  **验证独立性**：确认任务之间没有重叠的文件修改。
  2.  **创建任务板**：编写 `docs/tasks/YYYY-MM-DD-batch-name.md`。
  3.  **分发任务**：启动子 Agent 并行处理。
  4.  **收集结果**：等待所有 Agent 完成并汇报。

### <a id="9-subagent-driven-development"></a>9. `subagent-driven-development` (子 Agent 驱动开发)

- **我是谁**：你的项目经理。
- **什么时候用**：按照计划表（Plan）执行复杂任务时。
- **一句话指令**：
  > "Execute the plan using subagent-driven-development."
- **核心流程**：
  1.  **准备环境**：确保在独立的工作树中。
  2.  **分解任务**：将计划拆分为独立的子任务。
  3.  **逐个指派**：启动子 Agent 处理子任务 1 -> 验证 -> 处理子任务 2...
  4.  **集成**：所有子任务完成后，进行集成测试。

### <a id="10-executing-plans"></a>10. `executing-plans` (执行计划)

- **我是谁**：你的执行导演。
- **什么时候用**：你有一份写好的 `plan.md`，需要严格按步骤执行。
- **一句话指令**：
  > "Follow the instructions in docs/plans/migration.md using executing-plans."
- **核心流程**：
  1.  **加载计划**：读取计划文件。
  2.  **按序执行**：执行步骤 1 -> 验证 -> 执行步骤 2 -> 验证。
  3.  **检查点**：在关键节点暂停，确认状态。
  4.  **完成**：运行最终验证。

---

## 第四阶段：审查与交付 (The Review & Finish)

代码写完了，怎么合入主干？

### <a id="11-requesting-code-review"></a>11. `requesting-code-review` (请求代码审查)

- **我是谁**：你的公关经理。
- **什么时候用**：准备提 PR 之前。
- **一句话指令**：
  > "Prepare this branch for code review."
- **核心流程**：
  1.  **自我审查**：先运行 `verification-before-completion`。
  2.  **生成摘要**：列出变更点、设计决策、潜在风险。
  3.  **发起请求**：请求用户或其他 Agent 进行审查。

### <a id="12-receiving-code-review"></a>12. `receiving-code-review` (接收代码审查)

- **我是谁**：你的外交官。
- **什么时候用**：同事给你提了修改意见时。
- **一句话指令**：
  > "Here is the feedback. Use receiving-code-review to handle it."
- **核心流程**：
  1.  **理解反馈**：不要盲目接受，如果有疑问则提出。
  2.  **验证建议**：在实施建议前，先验证其可行性。
  3.  **实施变更**：使用 TDD 方式实施修改。
  4.  **确认解决**：回复审查意见，说明处理结果。

### <a id="13-finishing-a-development-branch"></a>13. `finishing-a-development-branch` (完成分支)

- **我是谁**：你的收纳师。
- **什么时候用**：一切尘埃落定，准备合并代码时。
- **一句话指令**：
  > "I'm done with this feature. Finish the branch."
- **核心流程**：
  1.  **最终验证**：运行所有测试。
  2.  **选择策略**：
      - Option 1: 本地合并 (Merge locally)。
      - Option 2: 推送并创建 PR (Push & PR)。
      - Option 3: 保持现状 (Keep as-is)。
      - Option 4: 丢弃 (Discard)。
  3.  **执行清理**：根据选择，删除不再需要的分支或工作树。

---

## 第五阶段：元技能与扩展 (Meta Skills)

关于技能的技能。

### <a id="14-find-skills"></a>14. `find-skills` (查找技能)

- **我是谁**：你的应用商店搜索。
- **什么时候用**：你想知道"有没有技能能帮我做数据库迁移？"
- **一句话指令**：
  > "Find a skill for database migration."

### <a id="15-writing-skills"></a>15. `writing-skills` (编写技能)

- **我是谁**：你的技能铸造师。
- **什么时候用**：你想把自己的独门绝技（比如"公司内部的代码规范检查流程"）教给 AI 时。
- **一句话指令**：
  > "Help me write a new skill called 'enforce-company-style'."
- **核心流程 (TDD for Skills)**：
  1.  **RED**: 定义“没有这个技能时 Agent 会犯的错误”（压力测试）。
  2.  **GREEN**: 编写技能文档，纠正 Agent 行为。
  3.  **REFACTOR**: 完善技能描述，堵住漏洞。

---

## 最佳实践总结：一套"组合拳"

遇到一个中型任务（比如"开发一个用户注册功能"），请牢记这套连招：

1.  **Brainstorming**: 聊聊需求，产出设计。
2.  **Writing Plans**: 定好步骤，产出计划。
3.  **Using Git Worktrees**: 开新坑，环境隔离。
4.  **TDD**: 红-绿-重构，写出高质量代码。
5.  **Verification**: 跑全套测试，确保无误。
6.  **Finishing Branch**: 提 PR 或合并，完美收工。

这就叫 **Superpowers**！

---

## 参考文档

- [Superpowers 详细用法教程 - 博客园](https://www.cnblogs.com/gyc567/p/19510203)
- [开源Superpowers库：3步解锁AI编程超能力 - CSDN](https://aicoding.csdn.net/696da84b7c1d88441d8de283.html)
