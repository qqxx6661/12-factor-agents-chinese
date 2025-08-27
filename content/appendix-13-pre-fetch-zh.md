### 因子 13 - 预取你可能需要的所有上下文

如果你的模型有很大可能会调用工具 X，不要浪费 token 往返告诉模型去获取它，也就是说，与其使用这样的伪提示：

```jinja
在查看部署时，你可能想要获取已发布的 git 标签列表，
这样你就可以用它来部署到生产环境。

到目前为止发生的事情：

{{ thread.events }}

下一步是什么？

用以下意图之一的 JSON 格式回答：

{
  intent: 'deploy_backend_to_prod',
  tag: string
} OR {
  intent: 'list_git_tags'
} OR {
  intent: 'done_for_now',
  message: string
}
```

你的代码看起来像这样

```python
thread = {"events": [initial_message]}
next_step = await determine_next_step(thread)

while True:
  switch next_step.intent:
    case 'list_git_tags':
      tags = await fetch_git_tags()
      thread["events"].append({
        type: 'list_git_tags',
        data: tags,
      })
    case 'deploy_backend_to_prod':
      deploy_result = await deploy_backend_to_prod(next_step.data.tag)
      thread["events"].append({
        "type": 'deploy_backend_to_prod',
        "data": deploy_result,
      })
    case 'done_for_now':
      await notify_human(next_step.message)
      break
    # ...
```

你不如就获取标签并将它们包含在上下文窗口中，像这样：

```diff
- 在查看部署时，你可能想要获取已发布的 git 标签列表，
- 这样你就可以用它来部署到生产环境。

+ 当前的 git 标签是：

+ {{ git_tags }}


到目前为止发生的事情：

{{ thread.events }}

下一步是什么？

用以下意图之一的 JSON 格式回答：

{
  intent: 'deploy_backend_to_prod',
  tag: string
- } OR {
-   intent: 'list_git_tags'
} OR {
  intent: 'done_for_now',
  message: string
}

```

你的代码看起来像这样

```diff
thread = {"events": [initial_message]}
+ git_tags = await fetch_git_tags()

- next_step = await determine_next_step(thread)
+ next_step = await determine_next_step(thread, git_tags)

while True:
  switch next_step.intent:
-    case 'list_git_tags':
-      tags = await fetch_git_tags()
-      thread["events"].append({
-        type: 'list_git_tags',
-        data: tags,
-      })
    case 'deploy_backend_to_prod':
      deploy_result = await deploy_backend_to_prod(next_step.data.tag)
      thread["events"].append({
        "type": 'deploy_backend_to_prod',
        "data": deploy_result,
      })
    case 'done_for_now':
      await notify_human(next_step.message)
      break
    # ...
```

甚至只是在线程中包含标签并从你的提示模板中删除特定参数：

```diff
thread = {"events": [initial_message]}
+ # 添加请求
+ thread["events"].append({
+  "type": 'list_git_tags',
+ })

git_tags = await fetch_git_tags()

+ # 添加结果
+ thread["events"].append({
+  "type": 'list_git_tags_result',
+  "data": git_tags,
+ })

- next_step = await determine_next_step(thread, git_tags)
+ next_step = await determine_next_step(thread)

while True:
  switch next_step.intent:
    case 'deploy_backend_to_prod':
      deploy_result = await deploy_backend_to_prod(next_step.data.tag)
      thread["events"].append(deploy_result)
    case 'done_for_now':
      await notify_human(next_step.message)
      break
    # ...
```

总的来说：

> #### 如果你已经知道你希望模型调用什么工具，就确定性地调用它们，让模型做弄清楚如何使用它们输出的困难部分

再次，AI 工程就是关于[上下文工程](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md)。

[← 无状态归约器](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-12-stateless-reducer.md) | [进一步阅读 →](https://github.com/humanlayer/12-factor-agents/blob/main/README.md#related-resources)