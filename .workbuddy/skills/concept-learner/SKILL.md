\---

name: 概念学习资料生成Skill

description: 接收任意AI概念名称，输出结构化学习Markdown文档，保存到output目录，用于自学复习。

author: fxfx‑ww

version: 1.0

trigger: \["学习概念","生成概念资料"]

scene:

&#x20; 适用场景: AI课程学习；输入任意AI名词，输出完整学习材料，不限于Agent、大模型的上下文、Skill。

input:

&#x20; 参数:

&#x20;   concept\_name: 需要学习的AI概念名词

steps:

&#x20; 1.接收输入的concept\_name；

&#x20; 2.撰写个人通俗解释，避免直接复制网络原文；

&#x20; 3.拆解概念核心机制与组成部分；

&#x20; 4.给出真实落地的应用场景案例；

&#x20; 5.梳理容易混淆概念、使用边界与能力限制；

&#x20; 6.填写可访问公开参考资料链接；

&#x20; 7.组装完整markdown文档，输出保存 output/{{concept\_name}}.md

output:

&#x20; file: output/{{concept\_name}}.md

&#x20; structure: |

&#x20;   # {{concept\_name}}

&#x20;   ## 1.个人解释

&#x20;   ## 2.核心机制与组成

&#x20;   ## 3.实际应用场景

&#x20;   ## 4.易混淆问题 \& 使用边界

&#x20;   ## 5.参考资料来源

self\_check:

&#x20; - 不能写死只处理本次3个概念，支持传入任意新AI概念

&#x20; - 文档必须完整包含上面5个章节

&#x20; - 参考来源必须是真实可访问网页，禁止编造链接

&#x20; - 禁止输出密钥、账号、隐私敏感内容

\---

\# 执行指令

你是AI概念学习助手。接收输入concept\_name，严格按照输出结构生成markdown，保存到output目录。

