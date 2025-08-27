[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 10. 小而专注的 Agent

与其构建试图做所有事情的单体 Agent，不如构建小而专注的 Agent，把一件事做好。Agent 只是更大的、主要是确定性系统中的一个构建块。

![1a0-small-focused-agents](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a0-small-focused-agents.png)

这里的关键洞察是关于 LLM 的局限性：任务越大越复杂，需要的步骤就越多，这意味着更长的上下文窗口。随着上下文的增长，LLM 更容易迷失或失去焦点。通过保持 Agent 专注于特定领域，最多 3-10 步，也许最多 20 步，我们保持上下文窗口可管理，LLM 性能高。

> #### 随着上下文的增长，LLM 更容易迷失或失去焦点

小而专注的 Agent 的好处：

1. **可管理的上下文**：较小的上下文窗口意味着更好的 LLM 性能
2. **明确的职责**：每个 Agent 都有明确定义的范围和目的
3. **更好的可靠性**：在复杂工作流程中迷失的可能性较小
4. **更容易测试**：更容易测试和验证特定功能
5. **改进的调试**：出现问题时更容易识别和修复

### 如果 LLM 变得更聪明怎么办？

如果 LLM 变得足够聪明以处理 100+ 步的工作流程，我们还需要这个吗？

简而言之，是的。随着 Agent 和 LLM 的改进，它们**可能**自然地扩展到能够处理更长的上下文窗口。这意味着处理更大 DAG 的更多部分。这种小而专注的方法确保你今天就能获得结果，同时为随着 LLM 上下文窗口变得更可靠而慢慢扩展 Agent 范围做好准备。（如果你以前重构过大型确定性代码库，你现在可能正在点头）。

[![gif](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a5-agent-scope-grow.gif)](https://github.com/user-attachments/assets/0cd3f52c-046e-4d5e-bab4-57657157c82f)

<details>
<summary><a href="https://github.com/humanlayer/12-factor-agents/blob/main/img/1a5-agent-scope-grow.gif">GIF 版本</a></summary>
![gif](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a5-agent-scope-grow.gif)
</details>

对 Agent 的大小/范围有意图，并且只以允许你维持质量的方式增长，这里是关键。正如[构建 NotebookLM 的团队所说](https://open.substack.com/pub/swyx/p/notebooklm?selection=08e1187c-cfee-4c63-93c9-71216640a5f8&utm_campaign=post-share-selection&utm_medium=web)：

> 我觉得一致地，对我来说，AI 构建中最神奇的时刻出现在我真的、真的、真的就在模型能力的边缘时

无论这个边界在哪里，如果你能找到这个边界并一致地做对，你就会构建神奇的体验。这里有许多护城河要建立，但一如既往，它们需要一些工程严谨性。

[← 压缩错误](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-09-compact-errors.md) | [从任何地方触发 →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-11-trigger-from-anywhere.md)