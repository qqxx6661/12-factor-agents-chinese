<!-- [机器翻译] 此文件由机器翻译生成，需要人工审校。原英文内容保留在文末供参考。 -->

# 因素 11：从任何地方触发

> **注意**: 本文档为机器翻译版本，可能包含翻译错误或不准确之处。建议参考文末的英文原文。

---

<details>
<summary>📖 查看英文原文 (View Original English)</summary>

[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 11. Trigger from anywhere, meet users where they are

If you're waiting for the [humanlayer](https://humanlayer.dev) pitch, you made it. If you're doing [factor 6 - launch/pause/resume with simple APIs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-06-launch-pause-resume.md) and [factor 7 - contact humans with tool calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-07-contact-humans-with-tools.md), you're ready to incorporate this factor.

![1b0-trigger-from-anywhere](https://github.com/humanlayer/12-factor-agents/blob/main/img/1b0-trigger-from-anywhere.png)

Enable users to trigger agents from slack, email, sms, or whatever other channel they want. Enable agents to respond via the same channels.

Benefits:

- **Meet users where they are**: This helps you build AI applications that feel like real humans, or at the very least, digital coworkers
- **Outer Loop Agents**: Enable agents to be triggered by non-humans, e.g. events, crons, outages, whatever else. They may work for 5, 20, 90 minutes, but when they get to a critical point, they can contact a human for help, feedback, or approval
- **High Stakes Tools**: If you're able to quickly loop in a variety of humans, you can give agents access to higher stakes operations like sending external emails, updating production data and more. Maintaining clear standards gets you auditability and confidence in agents that [perform bigger better things](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md#what-if-llms-get-smarter)

[← Small Focused Agents](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md) | [Stateless Reducer →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-12-stateless-reducer.md)

</details>
