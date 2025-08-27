# 12-Factor Agents 中文版 - 构建可靠 LLM 应用的原则

<div align="center">
<a href="https://www.apache.org/licenses/LICENSE-2.0">
        <img src="https://img.shields.io/badge/Code-Apache%202.0-blue.svg" alt="代码许可证: Apache 2.0"></a>
<a href="https://creativecommons.org/licenses/by-sa/4.0/">
        <img src="https://img.shields.io/badge/Content-CC%20BY--SA%204.0-lightgrey.svg" alt="内容许可证: CC BY-SA 4.0"></a>
<a href="https://humanlayer.dev/discord">
    <img src="https://img.shields.io/badge/chat-discord-5865F2" alt="Discord 服务器"></a>
<a href="https://www.youtube.com/watch?v=8kMaTybvDUw">
    <img src="https://img.shields.io/badge/aidotengineer-conf_talk_(17m)-white" alt="YouTube 深入讲解"></a>
<a href="https://www.youtube.com/watch?v=yxJDyQ8v6P0">
    <img src="https://img.shields.io/badge/youtube-deep_dive-crimson" alt="YouTube 深入讲解"></a>
</div>

<p></p>

*仿照 [12 Factor Apps](https://12factor.net/) 的精神*。*本项目的源代码在 https://github.com/humanlayer/12-factor-agents 公开，我欢迎你的反馈和贡献。让我们一起弄清楚这些！*

## 12 因子简介

即使 LLM [继续变得指数级更强大](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md#what-if-llms-get-smarter)，仍然存在使 LLM 驱动的软件更可靠、更可扩展和更易维护的核心工程技术。

## 中文翻译目录

### 基础理论
- [我们是如何走到今天的：软件简史](./content/brief-history-of-software-zh.md)

### 12 个因子

- [因子 1：自然语言到工具调用](./content/factor-01-natural-language-to-tool-calls-zh.md)
- [因子 2：拥有你的提示](./content/factor-02-own-your-prompts-zh.md)
- [因子 3：拥有你的上下文窗口](./content/factor-03-own-your-context-window-zh.md)
- [因子 4：工具只是结构化输出](./content/factor-04-tools-are-structured-outputs-zh.md)
- [因子 5：统一执行状态和业务状态](./content/factor-05-unify-execution-state-zh.md)
- [因子 6：使用简单 API 启动/暂停/恢复](./content/factor-06-launch-pause-resume-zh.md)
- [因子 7：使用工具调用联系人类](./content/factor-07-contact-humans-with-tools-zh.md)
- [因子 8：拥有你的控制流](./content/factor-08-own-your-control-flow-zh.md)
- [因子 9：将错误压缩到上下文窗口](./content/factor-09-compact-errors-zh.md)
- [因子 10：小而专注的 Agent](./content/factor-10-small-focused-agents-zh.md)
- [因子 11：从任何地方触发，在用户所在的地方与他们会面](./content/factor-11-trigger-from-anywhere-zh.md)
- [因子 12：让你的 Agent 成为无状态归约器](./content/factor-12-stateless-reducer-zh.md)

### 荣誉提及/其他建议
- [因子 13：预取你可能需要的所有上下文](./content/appendix-13-pre-fetch-zh.md)

## 为什么需要 12-factor agents？

在构建 HumanLayer 时，我与至少 100 位 SaaS 构建者（主要是技术创始人）交谈过，他们希望让现有产品更具 Agent 特色。旅程通常是这样的：

1. 决定要构建 Agent
2. 产品设计、UX 映射、解决什么问题
3. 想要快速行动，所以抓住 $FRAMEWORK 并*开始构建*
4. 达到 70-80% 的质量标准
5. 意识到 80% 对大多数面向客户的功能来说还不够好
6. 意识到超过 80% 需要逆向工程框架、提示、流程等
7. 从头开始重新来过

### 优秀 LLM 应用的设计模式

经过数百个 AI 库和与数十位创始人合作后，我的直觉是：

1. 有一些让 Agent 变得优秀的核心要素
2. 完全投入框架并构建本质上是绿地重写可能适得其反
3. 有一些让 Agent 变得优秀的核心原则，如果你引入框架，你会获得其中的大部分/全部
4. 但是，我见过构建者让高质量 AI 软件进入客户手中的最快方式是采用 Agent 构建中的小型模块化概念，并将它们整合到现有产品中
5. 即使没有 AI 背景的大多数熟练软件工程师也可以定义和应用 Agent 构建中的这些模块化概念

> #### 我见过构建者让优秀 AI 软件进入客户手中的最快方式是采用 Agent 构建中的小型模块化概念，并将它们整合到现有产品中

## 翻译说明

本翻译努力保持技术准确性，同时使用中国技术人员容易理解的术语：

- 保留所有代码示例的原始格式
- 保持所有链接和图片引用的功能性
- 使用适合技术分享的中文表达
- 对于通用技术术语，在适当时保留英文原词

## 原版资源

- 原版英文仓库：https://github.com/humanlayer/12-factor-agents
- 我在 [The Outer Loop](https://theouterloop.substack.com) 写一些相关内容
- 在 [got-agents/agents](https://github.com/got-agents/agents) 下使用这种方法构建 OSS agent

## 许可证

所有内容和图片都按 <a href="https://creativecommons.org/licenses/by-sa/4.0/">CC BY-SA 4.0 许可证</a> 许可

代码按 <a href="https://www.apache.org/licenses/LICENSE-2.0">Apache 2.0 许可证</a> 许可