---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定实施方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 能够适应组织现有的工作流，帮助我们用合适的方式构建正确的内容。接下来的几课将介绍典型的智能体驱动开发流程：从议题出发生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在后续功能工作中，你将继续使用同一对话和分支。通常会针对不同文件类型使用不同分支或 PR；这里为了聚焦核心概念，会采用更快捷的方式。

本课将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断增长，访问者需要按类别和发布商筛选游戏。待办事项中的议题描述了该功能，但在编写代码前，必须先确定类别组合筛选等细节。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 按照明确的范围实现功能。

## 背景

在开发流程中引入 AI 编码智能体不会改变基本原则，反而会让这些原则更加重要！大多数开发人员遵循的流程大致如下：

1. 从描述待办工作的议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码符合既定要求，并接受与手写代码相同的审查。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并迭代，直至任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能工作前，返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动后再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。使用 `#` 可将 GitHub 上的议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划能让你在交由 Copilot 执行前，先确定实现功能或完成任务的方案。对于复杂工作，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划已经制定并审查完毕，现在可以开始实现！使用 Autopilot 让 Copilot 执行任务。

Autopilot 会让 Copilot 持续迭代，直至认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

生成的代码与其他代码一样，需要在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认提示下方不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目验证结果，并要求 Copilot 解决任何失败项。

## 探索新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发布商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合议题及你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题开始了新的 Copilot 对话。
- 在 Plan 模式中定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，深入了解如何生成遵循既定规范的代码：使用[自定义说明][next-lesson]。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
