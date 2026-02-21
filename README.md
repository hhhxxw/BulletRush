# BulletRush

# 参考Vibe Coding 终极指南 V1.2.2做一个游戏

## 入门指南

要开始 vibe coding（氛围编码）使用工具：  
- **gpt-5.2-codex (high)**，在 Codex CLI 中使用（选择 Plus 订阅，约 $20/月）

本指南适用于 CLI 版本（在终端中使用）和 VSCode 扩展版本（Codex 和 Claude Code 都有，且界面更新）。

正确设置一切是关键。如果你想认真创建一个功能齐全且视觉效果出色的游戏（或应用），请花时间打下坚实的基础。  

**核心原则：** *规划就是一切。* 不要让 AI 自主规划，否则你的代码库将变得无法管理。

---

## 全面设置

### 0. 初始设置
- 下载 Visual Studio Code (https://code.visualstudio.com/)
- 在你的机器上创建一个新文件夹
- 右键单击，用 Code 打开
- 启动终端
  - 对于 Codex CLI：安装 Node，然后 `npm i -g @openai/codex`
- 启动 命令 `codex`

### 1. 游戏设计文档（如果是应用，则是产品需求文档）
- 拿着你的游戏创意，让 **GPT-5.2** 创建一个简单的 Markdown 格式的 **游戏设计文档 (Game Design Document)**：`game-design-document.md`。  
- 审查并完善文档，确保它符合你的愿景。基础一点没关系——目标是给你的 AI 提供关于游戏结构和意图的上下文。不要过度设计，我们稍后会迭代。
- 如果你愿意，也可以让 AI 向你提问，并利用这些回答来编写 GDD 或 PRD。

### 2. 技术栈和 `CLAUDE.md` / `Agents.md`
- 让 **GPT-5.2** 或 **Opus 4.5** 为你的游戏推荐最佳技术栈（例如，用于多人 3D 游戏的 Vite + ThreeJS 和 WebSocket）。将其保存为 `tech-stack.md`。
  
  - 挑战它提出*最简单但最健壮的技术栈*。  
- 在终端中，打开 **Claude Code** 或 **Codex CLI** 并使用 `/init` 命令。它将使用你目前创建的两个 .md 文件。这将创建一套规则，以便正确引导你的 LLM。
- **至关重要的一点是，审查生成的规则。** 确保它们强调 **模块化**（多个文件）并阻止 **单体应用**（一个巨大的文件）。你可能需要手动调整或添加规则。同时审查它们的触发时机。
  - **重要提示：** 一些规则对于保持上下文至关重要，应设置为 **"Always"（始终）** 规则。这确保 AI 在生成代码之前 *始终* 参考它们。考虑添加如下规则并将其标记为 "Always"：
    
    > ```
    > # IMPORTANT:
    > # Always read memory-bank/@architecture.md before writing any code. Include entire database schema.
    > # Always read memory-bank/@game-design-document.md before writing any code.
    > # After adding a major feature or completing a milestone, update memory-bank/@architecture.md.
    > ```
  - 示例：确保其他（非 "Always"）规则引导 AI 遵循你技术栈的最佳实践（如网络、状态管理等）。
  - *如果你想要一个尽可能优化、代码尽可能整洁的游戏，这套整体规则设置是强制性的。*


### 3. 实施计划
- 向 **GPT-5.2** 或 **Opus 4.5** 提供：  
  - 游戏设计文档 (`game-design-document.md`)
  - 技术栈推荐 (`tech-stack.md`)
- 让它创建一个详细的 Markdown (`.md`) 格式的 **实施计划 (Implementation Plan)**，这是给你 AI 开发者的分步说明。  
  - 步骤应小而具体。  
  - 每个步骤必须包含一个测试以验证正确实施。  
  - 不要代码：只需清晰、具体的说明。  
  - 专注于 *基础游戏*，而不是完整功能集（细节稍后补充）。  

### 4. 记忆库 (Memory Bank)
- 为你的项目创建一个新文件夹，然后在 VSCode 中打开它。
- 在项目文件夹内，创建一个名为 `memory-bank` 的子文件夹。  
- 将以下文件添加到 `memory-bank`：  
  - `game-design-document.md`  
  - `tech-stack.md`  
  - `implementation-plan.md`  
  - `progress.md`（创建一个空文件用于跟踪已完成的步骤）  
  - `architecture.md`（创建一个空文件用于记录文件用途）

---

## Vibe Coding 基础游戏
现在乐趣开始了！

### 确保一切清晰
- 在 VSCode 扩展中打开 **Codex** 或 **Claude Code**，或者在项目终端中启动 Claude Code 或 Codex CLI。
- 提示词 (Prompt)：阅读 `/memory-bank` 中的所有文档，`implementation-plan.md` 清晰吗？为了让你 100% 清楚，你有什么问题？
- 它通常会问 9-10 个问题。回答这些问题，并提示它相应地编辑 `implementation-plan.md`，使其更加完善。

### 你的第一个实施提示词
- 在 VSCode 扩展中打开 **Codex** 或 **Claude Code**，或者在项目终端中启动 Claude Code 或 Codex CLI。  
- 提示词：阅读 `/memory-bank` 中的所有文档，并开始实施计划的第 1 步。我将运行测试。在我验证测试之前，不要开始第 2 步。一旦我验证通过，打开 `progress.md` 并记录你所做的工作，以供未来的开发者参考。然后在 `architecture.md` 中添加任何架构见解，解释每个文件的作用。
- **始终** 以 "Ask"（询问）模式或 "Plan Mode"（计划模式）（在 Claude Code 中按 `shift+tab`）开始，一旦你满意，就允许 AI 执行该步骤。
- **极致 vibe：** 安装 [Superwhisper](https://superwhisper.com)，以便与 Claude 或 GPT-5.2 随意交谈，而不是打字。  

### 工作流
- 完成第 1 步后：  
- 将更改提交到 Git（如果不熟悉，请向你的 AI 求助）。  
- 开始一个新的聊天（`/new` 或 `/clear`）。为什么？当上下文窗口有大量空间时，LLM 会产生最好的结果。
- 提示词：现在浏览 memory-bank 中的所有文件，阅读 progress.md 以了解之前的工作，并继续进行第 2 步。在我验证测试之前，不要开始第 3 步。
- 重复此过程，直到整个 `implementation-plan.md` 完成。  

---

## 添加细节
恭喜，你已经构建了基础游戏！它可能很粗糙且缺乏功能，但现在你可以进行实验和完善了。  
- 想要雾气、后期处理、特效或声音？想要更好的飞机/汽车/城堡？绚丽的天空？
- 对于每个主要功能，创建一个新的 `feature-implementation.md` 文件，包含简短的步骤和测试。  
- 增量实施和测试。  

---

## 修复 Bug 和停滞
- 如果提示词失败或破坏了游戏：  
- 在 Claude Code 中使用 `/rewind` 并优化你的提示词直到它工作。如果使用 GPT-5.2，你可以经常提交到 git 并在需要时重置。
- 对于错误：  
    - **如果是 JavaScript：** 打开控制台 (`F12`)，复制错误，并将其粘贴到 VSCode 中，对于视觉故障提供截图。  
    - **懒人选项：** 安装 [BrowserTools](https://browsertools.agentdesk.ai/installation) 以跳过手动复制/截图。  
- 如果卡住了：  
    - 回退到上一次 Git 提交 (`git reset`) 并尝试新的提示词。  
- 如果 *真的* 卡住了：  
    - 使用 [RepoPrompt](https://repoprompt.com/) 或 [uithub](https://uithub.com/) 将整个代码库放入一个文件中，并向 **GPT-5.2 或 Claude** 寻求帮助。  

---

## Claude Code & Codex 技巧
- **终端中的 Codex CLI 或 Claude Code：** 在 VSCode 的终端内运行任一工具，以查看差异并在不离开工作区的情况下提供额外的上下文。
- **Claude Code `/rewind`：** 如果一次迭代未达标，使用此命令将项目回滚到较早的状态。
- **自定义 Claude Code 命令或技能：** 创建像 `/explain $arguments` 这样的助手，触发诸如“深入研究代码并理解 $arguments 是如何工作的。一旦你理解了，告诉我，我将为你提供任务。”这样的提示词，以便模型在编辑前获取丰富的上下文。
- **清除上下文：** 经常使用 `/clear` 或 `/compact` 清除上下文，如果你仍需要之前的对话上下文。保持 `/context` 在 50% 或 60% 以上以获得最佳性能。
- **节省时间（风险自负）：** 使用 `claude --dangerously-skip-permissions` 或 `codex --yolo` 以某种模式启动 Claude Code 或 Codex CLI，在该模式下它永远不会要求你确认。

## 其他技巧
- **小幅修改：** 使用 GPT-5.2 (medium)
- **出色的营销文案：** 使用 Opus 4.5
- **生成出色的精灵图 (2D 图像)：** 使用 ChatGPT 和 Nano Banana Pro
- **生成 3D 资产：** 使用 Trellis, Tripo 或 Hunyuan
- **生成音乐：** 使用 Suno, ElevenLabs
- **生成音效：** 使用 ElevenLabs
- **生成视频：** 使用 Sora 2, Veo 3
- **更好的提示词输出：** 
    - 添加“思考足够长的时间以确保正确，我不着急。重要的是你精确地遵循我的要求并完美地执行它。如果我不够精确，请问我问题。”
    - 对于 Claude Code，使用特定短语触发更深层的推理：`think` < `think hard` < `think harder` < `ultrathink`。

