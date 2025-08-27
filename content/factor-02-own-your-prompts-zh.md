[← 返回 README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 2. 拥有你的提示

不要将你的提示工程外包给框架。

![120-own-your-prompts](https://github.com/humanlayer/12-factor-agents/blob/main/img/120-own-your-prompts.png)

顺便说一下，[这绝不是什么新颖的建议：](https://hamel.dev/blog/posts/prompt/)

![image](https://github.com/user-attachments/assets/575bab37-0f96-49fb-9ce3-9a883cdd420b)

一些框架提供这样的"黑盒"方法：

```python
agent = Agent(
  role="...",
  goal="...",
  personality="...",
  tools=[tool1, tool2, tool3]
)

task = Task(
  instructions="...",
  expected_output=OutputModel
)

result = agent.run(task)
```

这对于引入一些顶级的提示工程来帮助你起步很棒，但通常很难调整和/或逆向工程以将恰当的 token 输入到你的模型中。

相反，拥有你的提示并将它们视为一等代码：

```rust
function DetermineNextStep(thread: string) -> DoneForNow | ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation {
  prompt #"
    {{ _.role("system") }}
    
    你是一个管理前端和后端系统部署的有用助手。
    你勤奋工作以确保安全和成功的部署，遵循最佳实践
    和正确的部署程序。
    
    在部署任何系统之前，你应该检查：
    - 部署环境（暂存 vs 生产）
    - 要部署的正确标签/版本
    - 当前系统状态
    
    你可以使用 deploy_backend、deploy_frontend 和 check_deployment_status
    等工具来管理部署。对于敏感部署，使用 request_approval 来获得
    人工验证。
    
    总是考虑首先要做什么，比如：
    - 检查当前部署状态
    - 验证部署标签存在
    - 如果需要请求批准
    - 先部署到暂存环境再到生产环境
    - 监控部署进度
    
    {{ _.role("user") }}

    {{ thread }}
    
    下一步应该是什么？
  "#
}
```

（上面的示例使用 [BAML](https://github.com/boundaryml/baml) 来生成提示，但你可以使用任何你想要的提示工程工具，甚至只是手动模板化）

如果签名看起来有点奇怪，我们会在[因子 4 - 工具只是结构化输出](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-04-tools-are-structured-outputs.md)中讲到

```typescript
function DetermineNextStep(thread: string) -> DoneForNow | ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation {
```

拥有提示的主要好处：

1. **完全控制**：准确编写你的 agent 需要的指令，没有黑盒抽象
2. **测试和评估**：为你的提示构建测试和评估，就像你对任何其他代码一样
3. **迭代**：基于真实世界的性能快速修改提示
4. **透明度**：准确知道你的 agent 正在使用什么指令
5. **角色黑客**：利用支持 user/assistant 角色非标准用法的 API - 例如，现在已弃用的非聊天风格 OpenAI "completions" API。这包括一些所谓的"模型欺骗"技术

记住：你的提示是你的应用程序逻辑和 LLM 之间的主要接口。

完全控制你的提示为你提供了生产级 agent 所需的灵活性和提示控制。

我不知道什么是最好的提示，但我知道你想要能够尝试一切的灵活性。

[← 自然语言到工具调用](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-01-natural-language-to-tool-calls.md) | [拥有你的上下文窗口 →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md)