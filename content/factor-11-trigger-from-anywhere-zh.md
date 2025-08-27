[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 11. 从任何地方触发，在用户所在的地方与他们会面

如果你正在等待 [humanlayer](https://humanlayer.dev) 的推介，你做到了。如果你正在做[因子 6 - 使用简单 API 启动/暂停/恢复](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-06-launch-pause-resume.md)和[因子 7 - 使用工具调用联系人类](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-07-contact-humans-with-tools.md)，你已经准备好包含这个因子了。

![1b0-trigger-from-anywhere](https://github.com/humanlayer/12-factor-agents/blob/main/img/1b0-trigger-from-anywhere.png)

使用户能够从 slack、电子邮件、短信或他们想要的任何其他渠道触发 Agent。使 Agent 能够通过相同的渠道响应。

好处：

- **在用户所在的地方与他们会面**：这帮助你构建感觉像真实人类的 AI 应用程序，或者至少是数字同事
- **外循环 Agent**：使 Agent 能够被非人类触发，例如事件、cron、故障、其他任何事情。它们可能工作 5、20、90 分钟，但当它们到达关键点时，它们可以联系人类寻求帮助、反馈或批准
- **高风险工具**：如果你能够快速引入各种人类，你可以给 Agent 访问更高风险的操作，如发送外部电子邮件、更新生产数据等。维持明确的标准为你提供审计能力和对[执行更大更好事情](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md#what-if-llms-get-smarter)的 Agent 的信心

[← 小而专注的 Agent](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md) | [无状态归约器 →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-12-stateless-reducer.md)