---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就要确保它能访问这些上下文。[说明文件][instruction-files]可提供相关指导，帮助 Copilot 理解不仅要完成*什么*，还要*如何*完成。

本练习将：

- 探索项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，这里不会运行“前后对比”提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后审查云智能体创建的拉取请求时，可以看到说明文件发挥的作用。

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

## 添加新的存储库标准

接下来要进行本练习中唯一的一处编辑：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释，并为文件添加注释头。稍后云智能体处理你分配的议题时，会读取这项规则。

开始编辑前，先创建并切换到一个新分支（云智能体会读取议题目标分支中的说明，无论如何都应整洁地提交这些编辑）：

1. 在 codespace 终端中运行：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将说明更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后，云智能体处理下一练习中分配的议题时就会读取这些说明。

> [!TIP]
> 如果想在本次工作坊中直接操作 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你已探索 Copilot 如何从本项目的说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。后续练习中可以在云智能体创建的拉取请求里看到这项标准的效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
