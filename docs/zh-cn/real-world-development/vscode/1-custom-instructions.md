---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种关键方式是使用[说明文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将：

- 探索项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 使用当前说明生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能的数据切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合某些标准。Copilot 具有概率性而非确定性。本练习的重点是观察更新说明后行为变化的*趋势*，而不是逐字符匹配输出。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格与需求。这项功能可引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，以及希望代码中包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含项目级信息，即与大多数 Copilot 聊天或 CLI 请求相关的上下文，例如所用技术栈、项目概述、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建。例如，可以为 TypeScript 或 Astro 等语言提供指导，也可以为创建 UI 组件或一组新单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊范围。不过，示例项目展示了一种具有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目概述、结构和全局编码标准。
- 使用 `*.instructions.md` 文件，为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建说明文件没有唯一正确的方法，就像使用 AI 也没有唯一正确的方法。通过不断尝试，可以找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览本项目中的文件时，你可能会发现有许多任务类型的说明，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前环境对应的课程会在相关位置介绍。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义说明文件

花些时间阅读此存储库随附的说明文件，其中包括核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。其中包含针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于决定说明适用于哪些文件。在此例中，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看此项目中有关创建单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成较小的可复用文件，并为 Copilot 提供生成代码时可参照的示例。（其中的路径相对于说明文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中编写文档。接下来的步骤会添加 TSDoc 文档注释和文件头注释规则。

## 为更改创建分支

遵循最佳实践，为更改创建一个分支。

1. 返回上一练习使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

要了解自定义说明的影响，先在当前说明生效时发送提示。稍后会更新说明并重新发送相同提示，观察差异。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 注意，生成的 helper 是带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中的数据层约定（该文件适用于 `src/lib/*.ts`）。
7. 注意，生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；这里要观察的是更新说明后*一致性*的提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意，该部分记录了项目编码标准，但尚未要求在代码中添加文档，因此生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件可以创建在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以创建为 `*.instructions.md` 文件，为特定语言、文件类型或任务提供说明。刚才添加的文档注释规则属于项目级标准，适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话内容。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，生成的文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发布商数据访问辅助函数。
    * 提供从数据库检索发布商信息的函数。
    */
   ```

5. 注意，生成的函数现在会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发布商及其 id 和 name 的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个解析为发布商对象数组的 Promise。
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一练习会继续完善这些代码。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，需要通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和 helper 更改已经存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新及 Copilot 为 helper 基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将其关联到待办事项中的文档标准议题。也可以手动创建，但 Copilot 可以使用项目中已连接的 GitHub 工具代为完成。请 Copilot 找到议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用其中某个工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍这种连接方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中已选择 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中既有说明更新，也有用于后续筛选工作的 publishers helper 基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求你**允许**它代表你执行 GitHub 操作时，请先审查操作，再根据情况选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求你**允许**它执行 GitHub 操作和 shell 命令时，请先审查命令，再根据情况选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你已探索 Copilot 如何从项目说明文件中获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在现有说明下生成的内容。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和 helper 基础提交到 `main`，让下一练习可以继续构建。

接下来，在[智能体模式][next-lesson]中应用这些说明，让 Copilot 在整个代码库中添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
