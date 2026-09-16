# Harness 工程速览（一页纸）

> 🎯 一句话：Harness 是「Agent 边界内、模型之外」的运行与治理层，负责把 LLM 能力安全、可靠地接上外部环境。
> ⏱️ 速读 3 分钟 · 考前 1 分钟看「回忆卡」
> 📚 来源：`../ai-agent-book/book/chapter1.md`

## ⚡ 考前回忆卡

1. `Agent = Model + Harness`
2. `Harness = 上下文管理 + 工具接口 + 约束 + 验证 + 纠正`
3. 位置：**Agent 内、Model 外**；不是 Environment。
4. 五要素口诀：**看、做、管、查、修**。
5. 范式演进：提示 → 上下文 → Harness → Loop → Graph。
6. 三原则：简单、透明、设计好 ACI。
7. 模型越强，Harness 不会消失，只会**转移职责**。

## 一句话理解

Harness 是 **Agent 边界内、模型之外** 的运行与治理层。
它负责把 LLM 的能力安全、可靠地接上外部环境。

## 核心公式

```text
Agent = Model + Harness

Harness = 上下文管理 + 工具接口 + 约束 + 验证 + 纠正

Agent ↔ Environment
```

## 五要素速记

| 要素 | 一句话 | 例子 |
| --- | --- | --- |
| 上下文管理 | 让模型看到该看的信息 | 系统提示词、记忆、状态栏 |
| 工具接口 | 让模型能观察和行动 | MCP 工具、代码解释器 |
| 约束 | 不让模型做不该做的事 | 默认关闭能力、显式授权 |
| 验证 | 检查做得对不对 | Linter、类型系统、结果校验 |
| 纠正 | 错了怎么恢复 | 重试、回退、熔断 |

> 💡 口诀：**看（上下文）、做（工具）、管（约束）、查（验证）、修（纠正）**。

## 控制循环骨架

```python
observation = Environment.observe()
trajectory = [observation]

while true:
    actions = Model(Harness.build_context(trajectory))
    if len(actions) == 0:
        break

    allowed_actions = Harness.constrain(actions)
    observation = Environment.apply(allowed_actions)

    if not Harness.verify(Environment):
        observation = Harness.correct(Environment)

    trajectory.append(allowed_actions)
    trajectory.append(observation)
```

## 为什么 Harness 重要

- 早期框架的重点是「能做事」：给模型上下文和工具。
- 生产级系统的重点是「可靠地做事」：加入约束、验证、纠正。
- 模型能力正在商品化，真正的竞争力在 Harness。

## 一个关键判断

- Harness 很重要，不等于 Harness 越复杂越好。
- 好结构不是替 Agent 预演全部思考，而是守住边界，把边界内决策空间还给模型。

## 工程范式演进

```text
提示工程 → 上下文工程 → Harness 工程 → Loop 工程 → Graph 工程
```

## 构建有效 Agent 三原则

1. 保持简单。
2. 保持透明。
3. 设计好工具接口 ACI，用设计消除错误。

## 最容易被混淆的边界

| | Harness | Environment |
| --- | --- | --- |
| 位置 | Agent 内、模型外 | Agent 外的真实或仿真世界 |
| 包含 | 工具定义、权限、沙箱规则 | 沙箱里变化的文件、外部数据库、网页、用户 |

## 复习自测

- 如果模型越来越强，Harness 会消失吗？
- 为什么生产级 Harness 的大部分代码是约束、验证和纠正，而不是工具本身？
- 为什么安全验证必须放在上下文之外？

## 答案提示

- **不会消失，只会转移职责**：模型每内化一层，Harness 就卸下一层，再兜底新的能力前沿。
- 因为「能做事」已经不够，关键是「可靠地做事」。
- 因为处在同一上下文中的 Agent 很难判断自己是否已被提示注入。
