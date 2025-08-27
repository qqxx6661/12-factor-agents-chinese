[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 7. 使用工具调用联系人类

默认情况下，LLM API 依赖于一个根本性的高风险 token 选择：我们是返回纯文本内容，还是返回结构化数据？

![170-contact-humans-with-tools](https://github.com/humanlayer/12-factor-agents/blob/main/img/170-contact-humans-with-tools.png)

你在第一个 token 的选择上投入了很大的权重，在 `the weather in tokyo` 案例中，它是

> "the"

但在 `fetch_weather` 案例中，它是一些表示 JSON 对象开始的特殊 token。

> |JSON>

你可能通过让 LLM *总是* 输出 json，然后用一些自然语言 token 如 `request_human_input` 或 `done_for_now`（与"适当的"工具如 `check_weather_in_city` 相对）声明其意图，来获得更好的结果。

同样，你可能不会从中获得任何性能提升，但你应该实验，并确保你可以自由地尝试奇怪的东西来获得最佳结果。

```python

class Options:
  urgency: Literal["low", "medium", "high"]
  format: Literal["free_text", "yes_no", "multiple_choice"]
  choices: List[str]

# 人类交互的工具定义
class RequestHumanInput:
  intent: "request_human_input"
  question: str
  context: str
  options: Options

# agent 循环中的使用示例
if nextStep.intent == 'request_human_input':
  thread.events.append({
    type: 'human_input_requested',
    data: nextStep
  })
  thread_id = await save_state(thread)
  await notify_human(nextStep, thread_id)
  return # 跳出循环并等待响应带着线程 ID 回来
else:
  # ... 其他情况
```

稍后，你可能会从处理 slack、电子邮件、短信或其他事件的系统收到 webhook。

```python

@app.post('/webhook')
def webhook(req: Request):
  thread_id = req.body.threadId
  thread = await load_state(thread_id)
  thread.events.push({
    type: 'response_from_human',
    data: req.body
  })
  # ... 为了简洁而简化，你可能不想在这里阻塞 web worker
  next_step = await determine_next_step(thread_to_prompt(thread))
  thread.events.append(next_step)
  result = await handle_next_step(thread, next_step)
  # todo - 循环或跳出或任何你想要的

  return {"status": "ok"}
```

上面的内容包括来自[因子 5 - 统一执行状态和业务状态](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-05-unify-execution-state.md)、[因子 8 - 拥有你的控制流](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md)、[因子 3 - 拥有你的上下文窗口](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md)和[因子 4 - 工具只是结构化输出](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-04-tools-are-structured-outputs.md)以及其他几个的模式。

如果我们使用来自[因子 3 - 拥有你的上下文窗口](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md)的 XML 格式，我们在几轮之后的上下文窗口可能看起来像这样：

```xml

(为了简洁而省略)

<slack_message>
    From: @alex
    Channel: #deployments
    Text: Can you deploy backend v1.2.3 to production?
    Thread: []
</slack_message>

<request_human_input>
    intent: "request_human_input"
    question: "Would you like to proceed with deploying v1.2.3 to production?"
    context: "This is a production deployment that will affect live users."
    options: {
        urgency: "high"
        format: "yes_no"
    }
</request_human_input>

<human_response>
    response: "yes please proceed"
    approved: true
    timestamp: "2024-03-15T10:30:00Z"
    user: "alex@company.com"
</human_response>

<deploy_backend>
    intent: "deploy_backend"
    tag: "v1.2.3"
    environment: "production"
</deploy_backend>

<deploy_backend_result>
    status: "success"
    message: "Deployment v1.2.3 to production completed successfully."
    timestamp: "2024-03-15T10:30:00Z"
</deploy_backend_result>
```

好处：

1. **明确指令**：用于不同类型人类联系的工具允许 LLM 更具体
2. **内循环 vs 外循环**：使 agent 工作流程能够在传统的 chatGPT 风格界面**之外**运行，其中控制流程和上下文初始化可能是 `Agent->Human` 而不是 `Human->Agent`（想想，由 cron 或事件启动的 agent）
3. **多人访问**：可以通过结构化事件轻松跟踪和协调来自不同人类的输入
4. **多 Agent**：简单的抽象可以轻松扩展以支持 `Agent->Agent` 请求和响应
5. **持久**：与[因子 6 - 使用简单 API 启动/暂停/恢复](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-06-launch-pause-resume.md)结合，这使得持久、可靠和可内省的多人工作流程成为可能

[更多关于外循环 Agent 的信息在这里](https://theouterloop.substack.com/p/openais-realtime-api-is-a-step-towards)

![175-outer-loop-agents](https://github.com/humanlayer/12-factor-agents/blob/main/img/175-outer-loop-agents.png)

与[因子 11 - 从任何地方触发，在用户所在的地方与他们会面](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-11-trigger-from-anywhere.md)配合得很好

[← 启动/暂停/恢复](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-06-launch-pause-resume.md) | [拥有你的控制流 →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md)