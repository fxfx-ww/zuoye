# Skill —— 概念深度教学方案

> 基于黑格尔《逻辑学》概念论「普遍性→特殊性→个体性」三环节设计
> 适用学段：AI Agent 入门 / 工程师内训 / 概念理解
> 学习目标：能区分 Skill / MCP / Tool / Prompt 四种概念；理解 Skill 的三层目录结构；能判断何时该创建 Skill；能描述 Skill 的加载与执行机制

---

## ① 诊断报告 —— 学生在哪里"卡"住了？

### 1.1 存在论诊断（学生把概念当成了什么？）

| 学生心中的"影子" | 典型话术 | 真实样貌 |
|:---|:---|:---|
| **影子 A：Skill = 一个工具/命令** | "Skill 不就是 `find-skills` 这种命令吗？" | Skill 是**能力包**，包含文档+脚本+资源；工具只是 Skill 暴露给 Agent 的一个动作 |
| **影子 B：Skill = MCP 连接器** | "Skill 跟那些 connector 不都是接外部服务的吗？" | **MCP 是协议**（对接外部 API/数据库），**Skill 是 Agent 内部的工作流封装**，两者层级不同 |
| **影子 C：Skill = Prompt 模板** | "Skill 不就是写一段提示词吗？" | Skill 可以包含**可执行脚本、参考文档、资产文件**，远不止文本提示 |
| **影子 D：Skill = Function Call / API** | "Skill 不就是函数调用吗？" | Function Call 是单次动作，**Skill 是带上下文加载、决策路径、文件操作的完整工作流** |

> **教学切入点**：让学生打开 `~/.workbuddy/skills/deep-concept-teaching/SKILL.md`，亲眼看到 SKILL.md 里既有"何时使用"的判定逻辑，也有"工作步骤"的执行清单，还有"输出格式"的规范——这就是 Skill 不是一个工具的实证。

### 1.2 本质论诊断（学生漏掉了什么内在结构？）

| 必备要素 | 学生是否意识到 |
|:---|:---|
| **物理层**：SKILL.md + scripts/ + references/ + assets/ 四种文件 | ❌ 以为 Skill 就是一个 .md 文件 |
| **逻辑层**：frontmatter (name/description) → 正文（when to use / how to use）→ 输出规范 | ⚠️ 看过 SKILL.md 但没注意结构 |
| **加载层**：Agent 识别意图 → 调用 Skill 工具 → SKILL.md 进入上下文 → 按指令执行 | ❌ 完全没意识到"上下文注入"机制 |
| **作用域层**：User-level (`~/.workbuddy/skills/`) vs Project-level (`{workspace}/.workbuddy/skills/`) | ❌ 不知道有区分 |
| **生命周期层**：创建 → 使用 → 修改 → 反射优化 → 沉淀 | ❌ 以为装上就用、不用就删 |

> **突破口**：用"菜谱"的隐喻——**SKILL.md 是菜谱，scripts/ 是预先准备好的半成品食材，references/ 是参考书，assets/ 是摆盘的装饰**。Skill = 一本完整的烹饪指南，让 Agent 能复现一道菜。

### 1.3 概念论诊断（学生能用概念做事吗？）

| 任务层级 | 学生能做到吗？ |
|:---|:---|
| 识别：看到一个 Skill 目录能列出所有组成部分 | ⚠️ 只认识 SKILL.md |
| 区分：能说清"Skill / MCP / Tool / Prompt"四种概念差异 | ❌ 混用 |
| 解释：能说清"为什么 Skill 设计核心是 SKILL.md" | ❌ 不知道 |
| 设计：能判断什么场景该写一个 Skill | ❌ 没方法 |
| 评价：能评审一个 Skill 的好坏 | ❌ 没标准 |

---

## ② 教学设计 —— 「普遍性→特殊性→个体性」三环节

### 2.1 普遍性：什么是"Skill"？（建立统一定义）

> **定义**：Skill 是 AI Agent 中**预封装的能力包**，由 `SKILL.md`（必选）+ `scripts/`、`references/`、`assets/`（可选）组成，通过 `Skill` 工具按需加载，向 Agent 提供**特定领域的专业工作流**。
>
> 它解决的核心问题是：**让 Agent 在需要时，能像换上一个领域专家的大脑一样工作**。

```
┌─────────────────────────────────────────────────┐
│   Skill 目录结构（典型）                          │
├─────────────────────────────────────────────────┤
│   📄 SKILL.md          ← 必选，包含 frontmatter  │
│      ├── name: xxx                              │
│      ├── description: xxx                       │
│      └── 正文（when/how/output）                 │
│   📁 scripts/  (可选) ← 可执行脚本                │
│   📁 references/ (可选) ← 参考文档/索引           │
│   📁 assets/ (可选)  ← 模板/图片/数据             │
└─────────────────────────────────────────────────┘
        ↓
   Agent 调用 Skill 工具
        ↓
   SKILL.md 注入到 Agent 上下文
        ↓
   Agent 按工作流执行（调用其他工具/读文件/输出）
```

**关键洞见**：Skill = **可复用的工作流文档** + **按需注入的专家知识**。它不是工具，不是 prompt，而是一种让 Agent "角色化"的机制。

### 2.2 特殊性：5 类 Skill（识别差异）

| 类型 | 核心特征 | 典型 Skill | 加载方式 |
|:---|:---|:---|:---|
| **① 教学/方法论类** | 封装概念教学、工作流设计方法 | `deep-concept-teaching`、`skill-creator` | 用户说"教我X""设计Y"时 |
| **② 数据查询类** | 自然语言 → 结构化数据 | `neodata-financial-search`、`westock-data` | 用户问"XX股价""XX数据"时 |
| **③ 工具执行类** | 自动化操作（浏览器/Office/CRM） | `agent-browser`、`expert-manager` | 用户说"打开XX""执行XX"时 |
| **④ 安装/市场类** | 搜索/安装其他 Skill | `find-skills`、`marketplace-skill-installer` | 用户说"装个XX技能"时 |
| **⑤ 多模态生成类** | 图像/视频/3D 生成 | `3D模型与视频特效` | 用户说"生成XX图"时 |

> **教师提问**："如果用户问'帮我查一下腾讯的股价'，Agent 应该用哪类 Skill？"
> 答：**② 数据查询类**（`westock-data` 或 `neodata-financial-search`）。Skill 工具的 description 字段会触发 Agent 自动匹配。

### 2.3 个体性：在真实 Agent 中落地（解剖一个 Skill）

以 `deep-concept-teaching` 为例（学生已经在用的！）：

```
~/.workbuddy/skills/deep-concept-teaching/
└── SKILL.md    ← 唯一的核心文件
    ├── frontmatter:
    │   ├── name: deep-concept-teaching
    │   ├── description: 概念深度教学技能——基于黑格尔《逻辑学》...
    │   └── triggers: 概念教学 / 概念深度 / 大概念...
    └── 正文:
        ├── 适用场景判定（什么情况下用）
        ├── 工作步骤（4 步：诊断→设计→判断→评估）
        ├── 输出规范（4 部分结构）
        └── 注意事项
```

**关键文件 `SKILL.md` 的三要素**：
1. **frontmatter**（YAML）：name + description + 可选 triggers——Agent 据此判断何时加载
2. **正文"何时使用"**：明确场景与不适用情况——决定加载决策
3. **正文"如何使用"**：具体步骤、工具调用、输出格式——决定执行路径

---

## ③ 判断与推理 —— 四层递进训练

### 3.1 质的判断（这是什么？）

> ❓ **问题**：下面哪个是 Skill，哪个不是？
>
> 1. `~/.workbuddy/skills/deep-concept-teaching/SKILL.md`
> 2. 一段写在 prompt 里的"请用费曼技巧解释"
> 3. 一个 MCP server（如 `tencent-docs` 连接器）
> 4. 一个 Python 函数 `def find_files(): ...`

**✅ 答案**：
- ① ✅ 是 Skill（完整的能力包）
- ② ❌ 不是（只是 prompt 文本，没有目录结构、加载机制）
- ③ ❌ 不是（MCP 是外部服务协议，Skill 是 Agent 内部封装）
- ④ ❌ 不是（函数是 Skill 内部 scripts/ 里的零件，不是 Skill 本身）

### 3.2 反思判断（它和相似概念有什么不同？）

> ❓ **问题**：请把 Skill 与以下概念区分清楚。

**对照表**：

| 维度 | Skill | MCP 连接器 | Tool (工具) | Prompt (提示词) |
|:---|:---|:---|:---|:---|
| **本质** | 能力包/工作流 | 外部服务协议 | 单一动作函数 | 文本指令 |
| **作用域** | Agent 内部 | 跨 Agent 通用 | 单次调用 | 当次对话 |
| **结构** | SKILL.md + 资源 | server config | 函数签名 | 字符串 |
| **复用性** | 高（封装完整工作流） | 中（封装服务） | 低（单点能力） | 低（一次性） |
| **示例** | `deep-concept-teaching` | `tencent-docs` | `Read`, `Bash`, `WebFetch` | "请帮我写一首诗" |
| **关系** | 可调用 Tools/MCP | 通过 Tool 调用 | 原子能力 | Skill 的输入之一 |

**关键洞见**：
- **Skill ⊃ Tool**：Skill 内部会调用多个 Tool
- **Skill ⊥ MCP**：MCP 是另一种能力来源（外部服务），Skill 可以选择性地通过 Tool 调用 MCP
- **Skill ⊃ Prompt**：Skill 包含 prompt 设计，但远不止 prompt

### 3.3 必然判断（为什么会这样设计？）

> ❓ **问题**：为什么 Skill 设计核心是 **SKILL.md** 这个 Markdown 文件，而不是可执行代码？

**三层原因**：
1. **上下文注入原理**：LLM Agent 是基于上下文的——SKILL.md 注入上下文后，Agent 才能"读懂"工作流；纯代码 Agent 看不到。
2. **可读性优先**：Markdown 是人类与 LLM 都能理解的格式。Agent 需要先"读懂"才能"执行"，不能像人一样只看代码注释。
3. **决策灵活性**：工作流里往往有"判断分支"——Agent 需要根据上下文决定走哪条路，纯代码无法承担决策（只能脚本化执行）。

> 💡 **反例**：如果是纯脚本（如一段 Python），Agent 只能"执行它"，不能"理解它"；遇到需要判断的场景就卡住。SKILL.md 让 Agent **既看得见又能执行**。

### 3.4 概念判断（如何应用与设计？）

> ❓ **任务**：判断下列场景是否应该创建一个 Skill，以及为什么。

| 场景 | 是否创建 Skill | 理由 |
|:---|:---|:---|
| 1. 一次性翻译一篇文档 | ❌ | 单次任务，无需封装为可复用能力 |
| 2. 每月自动生成竞品分析报告 | ✅ | 周期性任务，工作流固定，适合封装 |
| 3. 每天根据股价生成交易信号 | ✅ | 重复执行 + 多步骤决策，Skill 可降低使用门槛 |
| 4. 临时调试一段代码 | ❌ | 临时任务，没有复用价值 |
| 5. 公司内部"周报生成器" | ✅ | 多人/多次复用，需要统一规范 |

**Skill 创建决策清单**：
- [ ] 任务是**重复性**的（≥3 次/月）
- [ ] 工作流是**多步骤**的（≥3 个工具调用）
- [ ] 流程是**稳定**的（不会每周变）
- [ ] 团队/个人**都需要**（不只是个人一次性）
- [ ] **SKILL.md 能写清楚**（如果不能用 Markdown 表达，就不该是 Skill）

> 💡 **何时不该用 Skill**：临时任务、单步任务、变化快的任务——直接用普通工具调用更高效。

---

## ④ 评估工具

### 4.1 教师备课自检清单

- [ ] 我能用一句话说出 Skill 的统一定义
- [ ] 我能列出 Skill 目录的 4 种文件类型
- [ ] 我能区分 Skill / MCP / Tool / Prompt 四种概念
- [ ] 我能解释"为什么 SKILL.md 是 Skill 的核心"
- [ ] 我能描述 Skill 的加载与执行机制
- [ ] 我能为一个真实场景判断"该不该创建 Skill"
- [ ] 我能写出合格的 SKILL.md frontmatter

### 4.2 学生自评表

| 维度 | 完全不懂 | 听说过 | 能解释 | 能应用 |
|:---|:---|:---|:---|:---|
| 1. Skill 的统一定义 | □ | □ | □ | □ |
| 2. Skill 目录的 4 种文件 | □ | □ | □ | □ |
| 3. Skill vs MCP vs Tool vs Prompt | □ | □ | □ | □ |
| 4. SKILL.md 三要素 | □ | □ | □ | □ |
| 5. Skill 加载机制 | □ | □ | □ | □ |
| 6. User-level vs Project-level | □ | □ | □ | □ |
| 7. 何时该创建 Skill | □ | □ | □ | □ |
| 8. 写出一个最小可用 SKILL.md | □ | □ | □ | □ |

> 全部 ≥ "能解释" 即达标，"能应用" ≥ 6 项即优秀。

### 4.3 DeepSeek 辅助 Prompt

**学生自查 Prompt**：
```
我正在学习"Skill"这个概念（指 AI Agent 中的能力包）。请按以下结构帮我自查：

1. 用一句话定义"Skill"，不超过 30 字
2. 列出 Skill 目录的 4 种文件类型及其作用
3. 区分 Skill / MCP / Tool / Prompt 四种概念（用对照表）
4. 解释为什么 Skill 设计核心是 SKILL.md
5. 设计一个"每周自动生成竞品报告"的 Skill 草图（包含 frontmatter、3 个核心步骤、输出格式）

我会给你我的回答，请你打分（0-10）并指出"影子理解"。
```

**教师备课 Prompt**：
```
我要给学生讲"Skill"概念（学段：AI Agent 入门工程师，1 课时 45 分钟）。

请帮我设计：
1. 一个 5 分钟的导入故事（用"菜谱"或"乐高积木"的隐喻）
2. 3 个递进式课堂提问（从定义 → 区分 → 设计）
3. 1 个分组任务（4 人小组，30 分钟）：为一个真实场景设计 Skill 目录结构
4. 1 道课后思考题：评估"是否值得把现有 prompt 升级为 Skill"的标准

要求：贴合"黑格尔概念论"——让学生先识别"影子理解"（特别是把 Skill 当成 Prompt 的误区），再突破，最后能独立设计一个 Skill。
```

---

## ⑤ 核心教学观点（一句话总结）

> **"Skill"不是工具、不是命令、不是 prompt，而是 AI Agent 的"能力包"——以 SKILL.md 为核心，按需注入上下文，把特定领域的工作流封装为可复用的"专家大脑"。Skill ⊃ Tool、Skill ⊥ MCP、Skill ⊃ Prompt。**

---

**教学反思**：本方案刻意选了"Skill"这个**自指**的概念——学生正在使用的工具本身。教学关键突破点是：**让学生打开自己电脑上的 `~/.workbuddy/skills/` 目录，亲眼看 SKILL.md 长什么样**。这比任何抽象讲解都有效。建议第一节课做"解剖 Skill 目录"实验，第二节课做"为自己的任务设计 Skill"任务。