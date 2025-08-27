[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 4. 工具只是结构化输出

工具不需要复杂。在其核心，它们只是来自 LLM 的结构化输出，触发确定性代码。

![140-tools-are-just-structured-outputs](https://github.com/humanlayer/12-factor-agents/blob/main/img/140-tools-are-just-structured-outputs.png)

例如，假设你有两个工具 `CreateIssue` 和 `SearchIssues`。要求 LLM "使用多个工具中的一个"只是要求它输出我们可以解析为表示这些工具的对象的 JSON。

```python

class Issue:
  title: str
  description: str
  team_id: str
  assignee_id: str

class CreateIssue:
  intent: "create_issue"
  issue: Issue

class SearchIssues:
  intent: "search_issues"
  query: str
  what_youre_looking_for: str
```

模式很简单：
1. LLM 输出结构化 JSON
3. 确定性代码执行适当的操作（如调用外部 API）
4. 捕获结果并反馈到上下文中

这在 LLM 的决策制定和你的应用程序的行动之间创建了清晰的分离。LLM 决定做什么，但你的代码控制如何做。仅仅因为 LLM "调用了工具"并不意味着你必须每次都以相同的方式执行特定的对应函数。

如果你回想我们上面的 switch 语句

```python
if nextStep.intent == 'create_payment_link':
    stripe.paymentlinks.create(nextStep.parameters)
    return # 或者你想要的任何东西，见下文
elif nextStep.intent == 'wait_for_a_while': 
    # 做一些单子的事情 idk
else: #... 模型没有调用我们知道的工具
    # 做其他事情
```

**注意**：关于"纯提示"与"工具调用"与"JSON 模式"的好处以及各自的性能权衡已经有很多讨论。我们很快会链接一些关于这些内容的资源，但在这里不会深入讨论。参见[提示 vs JSON 模式 vs 函数调用 vs 约束生成 vs SAP](https://www.boundaryml.com/blog/schema-aligned-parsing)、[我应该何时使用函数调用、结构化输出或 JSON 模式？](https://www.vellum.ai/blog/when-should-i-use-function-calling-structured-outputs-or-json-mode#:~:text=We%20don%27t%20recommend%20using%20JSON,always%20use%20Structured%20Outputs%20instead)和[OpenAI JSON vs 函数调用](https://docs.llamaindex.ai/en/stable/examples/llm/openai_json_vs_function_calling/)。

"下一步"可能不像"运行纯函数并返回结果"那样原子化。当你将"工具调用"视为只是模型输出 JSON 描述确定性代码应该做什么时，你会释放很多灵活性。将这与[因子 8 拥有你的控制流](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md)结合起来。

[← 拥有你的上下文窗口](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md) | [统一执行状态 →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-05-unify-execution-state.md)