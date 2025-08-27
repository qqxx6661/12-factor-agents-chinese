[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 9. 将错误压缩到上下文窗口

这个有点短，但值得一提。Agent 的好处之一是"自我修复" - 对于短任务，LLM 可能调用失败的工具。好的 LLM 有相当好的机会读取错误消息或堆栈跟踪，并找出在后续工具调用中要更改的内容。

大多数框架都实现了这一点，但你可以在不做其他 11 个因子中的任何一个的情况下只做这一点。这是一个例子：

```python
thread = {"events": [initial_message]}

while True:
  next_step = await determine_next_step(thread_to_prompt(thread))
  thread["events"].append({
    "type": next_step.intent,
    "data": next_step,
  })
  try:
    result = await handle_next_step(thread, next_step) # 我们的 switch 语句
  except Exception as e:
    # 如果我们得到错误，我们可以将其添加到上下文窗口并重试
    thread["events"].append({
      "type": 'error',
      "data": format_error(e),
    })
    # 循环，或在这里做任何其他事情来尝试恢复
```

你可能想为特定工具调用实现一个 errorCounter，限制单个工具的尝试次数约为 3 次，或者任何其他对你的用例有意义的逻辑。

```python
consecutive_errors = 0

while True:

  # ... 现有代码 ...

  try:
    result = await handle_next_step(thread, next_step)
    thread["events"].append({
      "type": next_step.intent + '_result',
      data: result,
    })
    # 成功！重置错误计数器
    consecutive_errors = 0
  except Exception as e:
    consecutive_errors += 1
    if consecutive_errors < 3:
      # 进行循环并重试
      thread["events"].append({
        "type": 'error',
        "data": format_error(e),
      })
    else:
      # 跳出循环，重置上下文窗口的部分，升级给人类，或任何你想做的其他事情
      break
  }
}
```

达到某些连续错误阈值可能是[升级给人类](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-07-contact-humans-with-tools.md)的好地方，无论是通过模型决策还是通过确定性接管控制流。

[![195-factor-09-errors](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-09-errors.gif)](https://github.com/user-attachments/assets/cd7ed814-8309-4baf-81a5-9502f91d4043)

<details>
<summary>[GIF 版本](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-09-errors.gif)</summary>

![195-factor-09-errors](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-09-errors.gif)

</details>

好处：

1. **自我修复**：LLM 可以读取错误消息并找出在后续工具调用中要更改的内容
2. **持久**：即使一个工具调用失败，agent 也可以继续运行

我相信你会发现，如果你这样做得太多，你的 agent 会开始失控，可能会一遍又一遍地重复同样的错误。

这就是[因子 8 - 拥有你的控制流](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md)和[因子 3 - 拥有你的上下文构建](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md)发挥作用的地方 - 你不需要只是把原始错误放回去，你可以完全重构它的表示方式，从上下文窗口中删除以前的事件，或者任何你发现有效的确定性事情来让 agent 回到正轨。

但防止错误失控的第一方法是拥抱[因子 10 - 小而专注的 agent](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md)。

[← 拥有你的控制流](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md) | [小而专注的 Agent →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md)