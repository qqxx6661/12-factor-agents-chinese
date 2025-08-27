[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 3. 拥有你的上下文窗口

你不一定需要使用标准的基于消息的格式来向 LLM 传达上下文。

> #### 在任何给定时点，你在 agent 中对 LLM 的输入都是"到目前为止发生了什么，下一步是什么"

一切都是上下文工程。[LLM 是无状态函数](https://thedataexchange.media/baml-revolution-in-ai-engineering/)，将输入转换为输出。要获得最佳输出，你需要给它们最佳输入。

创建优秀上下文意味着：

- 你给模型的提示和指令
- 你检索的任何文档或外部数据（如 RAG）
- 任何过去的状态、工具调用、结果或其他历史记录
- 来自相关但独立历史/对话的任何过去消息或事件（内存）
- 关于输出什么类型结构化数据的指令

![image](https://github.com/user-attachments/assets/0f1f193f-8e94-4044-a276-576bd7764fd0)

### 关于上下文工程

本指南的重点是从今天的模型中获得尽可能多的收益。值得注意的是，没有提到的是：

- 对模型参数的更改，如 temperature、top_p、frequency_penalty、presence_penalty 等
- 训练你自己的完成或嵌入模型
- 微调现有模型

同样，我不知道向 LLM 传递上下文的最佳方式是什么，但我知道你希望有灵活性来尝试一切。

#### 标准 vs 自定义上下文格式

大多数 LLM 客户端使用这样的标准基于消息的格式：

```yaml
[
  {
    "role": "system",
    "content": "You are a helpful assistant..."
  },
  {
    "role": "user",
    "content": "Can you deploy the backend?"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "1",
        "name": "list_git_tags",
        "arguments": "{}"
      }
    ]
  },
  {
    "role": "tool",
    "name": "list_git_tags",
    "content": "{\"tags\": [{\"name\": \"v1.2.3\", \"commit\": \"abc123\", \"date\": \"2024-03-15T10:00:00Z\"}, {\"name\": \"v1.2.2\", \"commit\": \"def456\", \"date\": \"2024-03-14T15:30:00Z\"}, {\"name\": \"v1.2.1\", \"commit\": \"abe033d\", \"date\": \"2024-03-13T09:15:00Z\"}]}",
    "tool_call_id": "1"
  }
]
```

虽然这对大多数用例都很有效，但如果你想真正从今天的 LLM 中获得最大收益，你需要以最 token 和注意力高效的方式将上下文传入 LLM。

#### 自定义上下文格式示例

你可以尝试这样的自定义 XML 格式：

```xml
<user_input>
你能部署后端吗？
</user_input>

<list_git_tags>
</list_git_tags>

<list_git_tags_result>
tags:
  - name: v1.2.3
    commit: abc123
    date: 2024-03-15T10:00:00Z
  - name: v1.2.2
    commit: def456
    date: 2024-03-14T15:30:00Z
  - name: v1.2.1
    commit: abe033d
    date: 2024-03-13T09:15:00Z
</list_git_tags_result>
```

这种格式更紧凑，token 效率更高，同时保持了相同的信息密度。

### 上下文窗口管理策略

1. **Token 效率**：为 token 效率和 LLM 理解优化上下文格式
2. **信息密度**：将更多信息打包到更少的 token 中
3. **结构化数据**：使用结构化格式使 LLM 更容易解析
4. **历史管理**：策略性地包含相关历史记录，同时修剪不必要的详细信息
5. **分层优先级**：将最重要的信息放在最突出的位置

上下文包括：提示、指令、RAG 文档、历史记录、工具调用、内存

记住：上下文窗口是你与 LLM 的主要接口。控制如何构造和呈现信息可以显著提高你的 agent 性能。

示例 - 信息密度 - 相同消息，更少 token：

![Loom Screenshot 2025-04-22 at 09 00 56](https://github.com/user-attachments/assets/5cf041c6-72da-4943-be8a-99c73162b12a)

### 实施建议

1. **测试不同格式**：比较标准消息格式与自定义格式的性能
2. **度量 Token 使用**：跟踪 token 消耗并优化效率
3. **A/B 测试**：针对你的特定用例测试不同的上下文策略
4. **迭代改进**：基于性能结果持续优化你的上下文窗口

拥有你的上下文窗口给你提供了构建生产级 agent 所需的灵活性和控制力。

[← 拥有你的提示](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-02-own-your-prompts.md) | [工具只是结构化输出 →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-04-tools-are-structured-outputs.md)