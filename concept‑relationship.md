# Agent、大模型上下文、Skill三者关系
## 文字说明
Agent是具备自主规划、调用能力的智能主体；上下文是大模型对话记忆窗口，承载历史对话、任务信息；Skill是封装好的可复用能力包，给Agent提供工具能力。

三者协作流程：
1. 用户输入进入**上下文**，保存历史任务信息；
2. **Agent**读取上下文，分析任务目标；
3. Agent根据任务调度对应的**Skill**执行具体操作；
4. Skill执行结果写回上下文，交给大模型整理输出回答。

## 关系流程图（Mermaid）
```mermaid
graph LR
User[用户提问] --> Context[上下文Context]
Context --> Agent[Agent智能体]
Agent --> Skill[Skill能力包]
Skill --> Agent
Agent --> Context
Context --> Output[输出回答]