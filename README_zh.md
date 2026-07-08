# Awesome 面向自我改进的 Harness 工程 [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | **中文**

> 一份聚焦 **harness 工程**、将其作为 LLM 智能体 **递归自我改进（RSI）** 基底的精选阅读清单。
>
> *Harness*（执行外壳）指包裹在基础模型之外、负责编排模型如何思考与规划、调用工具与行动、感知与管理上下文、存储产物、评估自身结果的系统。本清单**刻意收窄**：只收录那些 **harness 本身——它的上下文、提示词、工作流、代码或脚手架——成为被优化、被搜索、被进化以实现自我改进的对象**的工作，直至 harness 与模型权重联合优化的回路为止。

本仓库受 Lilian Weng 博客 *["Harness Engineering for Self-Improvement"](https://lilianweng.github.io/posts/2026-07-04-harness/)*（2026 年 7 月）启发，并沿用其主线：**Harness 设计模式 → Harness 优化 → 自动化科研 → 挑战**。

### 范围说明（请先读）

**在焦点内** —— 把 harness 作为自我改进的对象：自我修改的智能体、智能体系统的自动化设计、自进化上下文、提示词/工作流优化、对 harness 代码的进化式程序搜索、以及 harness+权重联合回路。此外还包括这些方法赖以运作的设计模式基底、把回路投入实用的自动化科研系统、以及自我改进 harness 特有的评测/安全挑战。

**刻意排除在焦点外** —— 与原博客一致，纯粹的*模型侧*自我改进（自博弈、合成数据、RLVR、测试时权重训练、弱到强）**不是**本清单的主题。它们只以一个简短的边界小节 [§8 毗邻领域](#8-毗邻领域刻意排除在焦点外) 出现，用于标明本清单的边界。

> ⭐ 如果有帮助，欢迎 Star。欢迎通过 PR 补充遗漏论文或修正链接。`†` 表示预印本或非常新的 arXiv 投稿，其元数据可能仍会变动。

---

## 目录

- [概览](#概览)
- [优化阶梯](#优化阶梯)
- [历史脉络](#历史脉络)
- [论文清单](#论文清单)
  - [1. 基础与立场文章](#1-基础与立场文章)
  - [2. Harness 设计模式（基底）](#2-harness-设计模式基底)
    - [2.1 执行循环与自我纠正](#21-执行循环与自我纠正)
    - [2.2 文件系统作为持久记忆与状态](#22-文件系统作为持久记忆与状态)
    - [2.3 子智能体与编排](#23-子智能体与编排)
    - [2.4 Coding Agent Harness（案例研究）](#24-coding-agent-harness案例研究)
    - [2.5 智能体运行时与协议](#25-智能体运行时与协议)
  - [3. Harness 优化（核心）](#3-harness-优化核心)
    - [3.1 上下文工程（自进化上下文）](#31-上下文工程自进化上下文)
    - [3.2 提示词优化](#32-提示词优化)
    - [3.3 工作流设计与智能体系统自动化设计](#33-工作流设计与智能体系统自动化设计)
    - [3.4 自我改进与自我修改的 Harness](#34-自我改进与自我修改的-harness)
    - [3.5 进化搜索与程序搜索](#35-进化搜索与程序搜索)
    - [3.6 与模型权重的联合优化](#36-与模型权重的联合优化)
  - [4. 自动化科研：回路的实战](#4-自动化科研回路的实战)
    - [4.1 自主科研智能体](#41-自主科研智能体)
    - [4.2 科学发现与机器学习工程智能体](#42-科学发现与机器学习工程智能体)
    - [4.3 自动化数据生成与想法生成](#43-自动化数据生成与想法生成)
    - [4.4 批判性分析：智能体是科学家了吗？](#44-批判性分析智能体是科学家了吗)
  - [5. 评测与基准](#5-评测与基准)
    - [5.1 AI 科研与机器学习工程基准](#51-ai-科研与机器学习工程基准)
    - [5.2 编程与终端智能体基准](#52-编程与终端智能体基准)
    - [5.3 验证与验证器](#53-验证与验证器)
  - [6. 挑战：安全、奖励作弊与严谨性](#6-挑战安全奖励作弊与严谨性)
  - [7. 相关综述](#7-相关综述)
  - [8. 毗邻领域（刻意排除在焦点外）](#8-毗邻领域刻意排除在焦点外)
  - [9. 实践报告与工业界洞见](#9-实践报告与工业界洞见)
- [未来方向](#未来方向)
- [贡献](#贡献)
- [引用](#引用)

---

## 概览

主流叙事将智能体能力归功于底层模型。本清单遵循一个互补论点：**位于原始模型与真实世界上下文之间的那一层——harness——与模型的原始智能同等重要**;而且关键在于,这一层是可以*被设计成自我改进*的。

递归自我改进（RSI）可追溯到 I. J. Good（1965），并由 Yudkowsky（2008）命名：AI 用当下智能去改进产生其智能的机器。在现代 AI 中，这个回路很少从模型直接改写权重开始。更务实的近期路径*穿过 harness*：模型改进脚手架、工作流、上下文管理与部署系统，进而催生更强的后继者。

因此本清单的组织问题狭窄而具体：**当 harness 成为一个可执行、可优化的对象时，会发生什么？** 随着模型能力增强，*被优化的对象*沿着一架阶梯往上走——从手写指令，到结构化上下文，到工作流，到 harness 代码，到优化器代码本身，最终到 harness 与权重联合。此处收录的一切，都因其在这架阶梯上（或直接支撑该阶梯）而入选。

---

## 优化阶梯

Harness 系统内部被优化对象的演进，从最手工到最通用。**这架阶梯是本清单的骨架。**

| 层级 | 被优化对象 | 代表工作 | 小节 |
| ---- | --------- | -------- | ---- |
| L0 | **指令提示词** | APE、OPRO、Promptbreeder、ProTeGi | [3.2](#32-提示词优化) |
| L1 | **结构化上下文** | ACE、Dynamic Cheatsheet、ReasoningBank | [3.1](#31-上下文工程自进化上下文) |
| L2 | **工作流 / 图** | ADAS、AFlow、GPTSwarm、AgentSquare | [3.3](#33-工作流设计与智能体系统自动化设计) |
| L3 | **Harness 代码** | STOP、Gödel Agent、DGM、SICA、Self-Harness | [3.4](#34-自我改进与自我修改的-harness) |
| L4 | **优化器 / 元-harness 代码** | Meta-Harness、MCE、Meta Agent Search | [3.4](#34-自我改进与自我修改的-harness) |
| L5 | **Harness + 模型权重（联合）** | SIA、SEAL | [3.6](#36-与模型权重的联合优化) |
| — | *(边界)* 仅模型权重 | *自博弈、RLVR、合成数据* | [§8](#8-毗邻领域刻意排除在焦点外) —— 焦点外 |

*模型越强，领域越往表格下方走。L5 之下是纯粹的模型权重自我改进，本清单刻意将其视为毗邻而非核心。*

---

## 历史脉络

| 年份 | 里程碑 | 意义 |
| ---- | ------ | ---- |
| 1965 | I. J. Good —"超智能机器" | 首次提出借由自我设计实现智能爆炸 |
| 2008 | Yudkowsky —"递归自我改进" | 为 RSI 反馈回路命名 |
| 2023 | Reflexion、Voyager、DSPy、Promptbreeder、STOP、FunSearch | 言语 RL 回路、终身技能、可编译流水线、自指改进器 |
| 2024 | ADAS、AFlow、GPTSwarm、Agent Symbolic Learning、TextGrad | 智能体/工作流自动化设计；对 harness 各部分做文本"反向传播" |
| 2025 | AlphaEvolve、ShinkaEvolve、DGM、SICA、ACE、GEPA、SEAL | 进化式编程智能体；自我修改智能体；上下文进化；自适应权重 |
| 2026 | Meta-Harness、MCE、Self-Harness、AutoHarness、Hyperagents、SIA | 优化 harness 的 harness；harness+权重联合回路 |

---

## 论文清单

### 1. 基础与立场文章

- **Harness Engineering for Self-Improvement** — Lilian Weng. *Lil'Log* 2026. [[博客]](https://lilianweng.github.io/posts/2026-07-04-harness/) — 本清单据以构建的博客；将 harness 视为 RSI 的近期基底。
- **Speculations Concerning the First Ultraintelligent Machine** — I. J. Good. *Advances in Computers* 1965. — 智能爆炸思想的源头。
- **Recursive Self-Improvement** — E. Yudkowsky. *LessWrong* 2008. [[原文]](https://www.lesswrong.com/posts/JBadX7rwdcRFzGuju/recursive-self-improvement) — 命名并分析 RSI 反馈回路。
- **Cognitive Architectures for Language Agents (CoALA)** — Sumers、Yao、Narasimhan、Griffiths. *TMLR* 2024. [[论文]](https://arxiv.org/abs/2309.02427) — 以记忆、动作空间与决策循环组织智能体，为 harness 组件提供概念脚手架。
- **A Survey of Self-Evolving Agents: What, When, How, and Where to Evolve** — Gao 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2507.21046) — 自我进化智能体的分类体系（模型、记忆、工具、架构）。
- **A Comprehensive Survey of Self-Evolving AI Agents** — Fang 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2508.07407) — 连接基础模型与终身智能体系统，提出"自我进化 AI 智能体三定律"。

### 2. Harness 设计模式（基底）

*这些是自我改进方法赖以运作的反复出现的结构。它们本身不是"自我改进"，但定义了优化器所作用的可编辑面。*

#### 2.1 执行循环与自我纠正

- **ReAct** — "Synergizing Reasoning and Acting in Language Models". Yao 等. *ICLR* 2023. [[论文]](https://arxiv.org/abs/2210.03629) — 经典的"思考→行动→观察"循环；多数 harness 的默认循环。
- **Reflexion** — "Language Agents with Verbal Reinforcement Learning". Shinn 等. *NeurIPS* 2023. [[论文]](https://arxiv.org/abs/2303.11366) — 将反馈转为言语反思存入情节记忆——harness 内自我改进循环的原型。
- **Self-Refine** — "Iterative Refinement with Self-Feedback". Madaan 等. *NeurIPS* 2023. [[论文]](https://arxiv.org/abs/2303.17651) — 单回合内"生成→批评→精炼"；被 ADAS 式元智能体复用的自精炼步骤。
- **ReWOO** — "Decoupling Reasoning from Observations for Efficient Augmented Language Models". Xu 等. *arXiv* 2023. [[论文]](https://arxiv.org/abs/2305.18323) — 规划器/执行器/求解器分解，预先规划工具调用。
- **autoresearch** — Andrej Karpathy. *GitHub* 2026. [[代码]](https://github.com/karpathy/autoresearch) — 目标导向的"规划→执行→观察→改进"循环的简洁参考实现。

#### 2.2 文件系统作为持久记忆与状态

- **MemGPT** — "Towards LLMs as Operating Systems". Packer 等. *arXiv* 2023. [[论文]](https://arxiv.org/abs/2310.08560) — OS 式虚拟上下文管理，在上下文与外部存储间分页。
- **Agent Workflow Memory (AWM)** — Wang、Mao、Fried、Neubig. *ICML* 2025. [[论文]](https://arxiv.org/abs/2409.07429) — 归纳并复用可重用"工作流"，作为随经验增长的持久程序性记忆。
- **FS-Researcher** — "Test-Time Scaling for Long-Horizon Research Tasks with File-System-Based Agents". Zhu、Xu 等. *ACL* 2026.† — 以持久文件系统工作区作为深度研究的外部记忆。
- **Memp** — "Exploring Agent Procedural Memory". Fang 等. *ACL Findings* 2026.† [[论文]](https://arxiv.org/abs/2508.06433) — 将轨迹蒸馏为脚本式流程，支持向更弱模型迁移。

#### 2.3 子智能体与编排

- **AutoGen** — "Enabling Next-Gen LLM Applications via Multi-Agent Conversation". Wu 等. *COLM* 2024. [[论文]](https://arxiv.org/abs/2308.08155) — 可对话智能体框架；编排搜索的常用基底。
- **MetaGPT** — "Meta Programming for a Multi-Agent Collaborative Framework". Hong 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2308.00352) — 将 SOP 编码为带可执行反馈的角色流水线。
- **Chain-of-Agents** — "Large Language Models Collaborating on Long-Context Tasks". Zhang 等. *NeurIPS* 2024. [[论文]](https://arxiv.org/abs/2406.02818) — 工人智能体分块处理并向管理者传消息，处理长上下文。
- **AOrchestra** — "Automating Sub-Agent Creation for Agentic Orchestration". Ruan 等. *arXiv* 2026.† — 自动创建子智能体——把编排本身当作可优化面。

#### 2.4 Coding Agent Harness（案例研究）

主流编程智能体的核心接口已在 Claude Code、Codex、OpenHands 与 Cursor 类智能体间趋于稳定：包含文件系统工具（`glob`、`grep`、`read`、`write`、`edit`、`apply_patch`）、shell 执行、git/LSP IO、外部上下文（MCP、Skills）、Web 搜索与智能体委派的循环。这是 harness 自我改进最常见的可编辑面。

- **SWE-agent** — "Agent-Computer Interfaces Enable Automated Software Engineering". Yang、Jimenez 等. *NeurIPS* 2024. [[论文]](https://arxiv.org/abs/2405.15793) — 提出智能体-计算机接口（ACI）；接口设计比原始模型能力更关键。
- **OpenHands / OpenDevin** — "An Open Platform for AI Software Developers as Generalist Agents". Wang 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2407.16741) — 面向代码/CLI/Web 智能体的开放平台，带沙箱执行。
- **CodeAct** — "Executable Code Actions Elicit Better LLM Agents". Wang 等. *ICML* 2024. [[论文]](https://arxiv.org/abs/2402.01030) — 以可执行 Python 代码作为统一动作空间，配自我调试解释器。
- **Agentless** — "Demystifying LLM-based Software Engineering Agents". Xia 等. *FSE* 2025. [[论文]](https://arxiv.org/abs/2407.01489) — 固定的"定位→修复→验证"流水线可以更低成本媲美自主智能体——一次 harness 设计消融。
- **OPENDEV** — "Building Effective AI Coding Agents for the Terminal". Bui. *arXiv* 2026.† — 关于终端编程 harness 设计的实践分析。

#### 2.5 智能体运行时与协议

- **AIOS** — "LLM Agent Operating System". Mei 等. *COLM* 2025. [[论文]](https://arxiv.org/abs/2403.16971) — 面向 LLM 的 OS 内核：调度、上下文/记忆/存储管理、工具服务、访问控制。
- **GoEX** — "Perspectives and Designs Towards a Runtime for Autonomous LLM Applications". Patil 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2404.06921) — 面向安全执行自主 harness 动作的运行时设计（撤销/损害隔离）。
- **Model Context Protocol (MCP)** — "MCP: Landscape, Security Threats, and Future Research Directions". Hou 等. *arXiv* 2025. [[论文]](https://arxiv.org/abs/2503.23278) — 标准化的工具↔harness 接口，自进化智能体（如 Alita）在运行时扩展它。
- **Repo2Run** — "Automated Building Executable Environment for Code Repository at Scale". Hu 等. *arXiv* 2025.† — 自动构建可执行环境，是可评测的 harness 自我改进的前提。

### 3. Harness 优化（核心）

*这是本清单的核心。每个小节对应[优化阶梯](#优化阶梯)的一个层级。*

#### 3.1 上下文工程（自进化上下文）

- **ACE** — "Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models". Zhang 等. *ICLR* 2026. [[论文]](https://arxiv.org/abs/2510.04618) — 将上下文视为不断演化的"playbook"，经 Generator/Reflector/Curator 增量更新，避免上下文坍缩。
- **MCE** — "Meta Context Engineering via Agentic Skill Evolution". Ye 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2601.21557) — 双层框架，协同进化上下文管理*技能*（元层）与*上下文产物*（基层，以文件/代码）——把机制与内容分离。
- **Dynamic Cheatsheet** — "Test-Time Learning with Adaptive Memory". Suzgun 等. *EACL* 2026.† [[论文]](https://arxiv.org/abs/2504.07952) — 推理时自我策划的策略/片段持久记忆；ACE 的直接前身。
- **ReasoningBank** — "Scaling Agent Self-Evolving with Reasoning Memory". Ouyang 等. *ICLR* 2026.† [[论文]](https://arxiv.org/abs/2509.25140) — 从成功*与*失败中蒸馏可泛化策略；提出记忆感知的测试时扩展。
- **ExpeL** — "LLM Agents Are Experiential Learners". Zhao 等. *AAAI* 2024. [[论文]](https://arxiv.org/abs/2308.10144) — 采集经验、把自然语言洞见抽取进不断增长的上下文库，推理时调用而无需微调。
- **MemAct** — "Memory as Action: Autonomous Context Curation for Long-Horizon Agentic Tasks". Zhang 等. *ACL Findings* 2026.† [[论文]](https://arxiv.org/abs/2510.12635) — 将工作记忆管理重构为可端到端训练的策略动作。

#### 3.2 提示词优化

*阶梯的 L0：把 harness 的指令层作为优化对象。*

- **APE** — "Large Language Models Are Human-Level Prompt Engineers". Zhou 等. *ICLR* 2023. [[论文]](https://arxiv.org/abs/2211.01910) — 把指令当作程序，通过搜索提出并打分候选。
- **OPRO** — "Large Language Models as Optimizers". Yang 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2309.03409) — "以提示做优化"：从含历史（解, 分）对的元提示中生成新解。
- **EvoPrompt** — "Connecting LLMs with Evolutionary Algorithms Yields Powerful Prompt Optimizers". Guo 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2309.08532) — 用 LLM 做变异/交叉，在离散提示种群上运行 GA/DE。
- **Promptbreeder** — "Self-Referential Self-Improvement via Prompt Evolution". Fernando 等. *arXiv* 2023.† [[论文]](https://arxiv.org/abs/2309.16797) — 同时进化任务提示*与*改写它们的变异提示——自指式提示改进。
- **ProTeGi** — "Automatic Prompt Optimization with 'Gradient Descent' and Beam Search". Pryzant 等. *EMNLP* 2023. [[论文]](https://arxiv.org/abs/2305.03495) — 提出"文本梯度"：将 LLM 批评作为编辑提示的自然语言梯度。
- **DSPy** — "Compiling Declarative Language Model Calls into Self-Improving Pipelines". Khattab 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2310.03714) — 将 LM 流水线视为可优化的文本变换图的编程模型。
- **MIPROv2** — "Optimizing Instructions and Demonstrations for Multi-Stage Language Model Programs". Opsahl-Ong 等. *EMNLP* 2024. [[论文]](https://arxiv.org/abs/2406.11695) — 用贝叶斯优化联合自举示例并提出指令。
- **TextGrad** — "Automatic 'Differentiation' via Text". Yuksekgonul 等. *Nature* 2025. [[论文]](https://arxiv.org/abs/2406.07496) — 以 PyTorch 式反向传播将文本反馈穿过复合 AI 系统。
- **GEPA** — "Reflective Prompt Evolution Can Outperform Reinforcement Learning". Agrawal 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2507.19457) — 读取完整轨迹的遗传-帕累托反思优化器；以最多 35× 更少 rollout 胜过 RL。

#### 3.3 工作流设计与智能体系统自动化设计

*阶梯的 L2：把智能体工作流/图作为搜索对象。*

- **ADAS / Meta Agent Search** — "Automated Design of Agentic Systems". Hu、Lu、Clune. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2408.08435) — 元智能体在不断增长的归档上以代码编写越来越好的智能体。
- **AFlow** — "Automating Agentic Workflow Generation". Zhang 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2410.10762) — 将工作流优化建模为对代码化图的 MCTS 搜索。
- **GPTSwarm** — "Language Agents as Optimizable Graphs". Zhuge 等. *ICML* 2024. [[论文]](https://arxiv.org/abs/2402.16823) — 将智能体视为计算图；节点级提示 + 边级 REINFORCE 优化。
- **AgentSquare** — "Automatic LLM Agent Search in Modular Design Space". Shang 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2410.06153) — 在模块化的规划/推理/工具/记忆空间中经进化与重组搜索。
- **MaAS** — "Multi-agent Architecture Search via Agentic Supernet". Zhang 等. *ICML* 2025. [[论文]](https://arxiv.org/abs/2502.04180) — 优化概率化"智能体超网"，实现按查询采样、成本自适应。
- **MASS** — "Multi-Agent Design: Optimizing Agents with Better Prompts and Topologies". Zhou 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.02533) — 对提示与拓扑做交错的多阶段搜索。
- **ScoreFlow** — "Mastering LLM Agent Workflows via Score-based Preference Optimization". Wang 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.04306) — 借 Score-DPO 做连续梯度式工作流优化。
- **FlowReasoner** — "Reinforcing Query-Level Meta-Agents". Gao 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2504.15257) — 经 RL 调优的推理型元智能体，为每个查询设计定制多智能体系统。
- **EvoAgent** — "Towards Automatic Multi-Agent Generation via Evolutionary Algorithms". Yuan 等. *NAACL* 2025. [[论文]](https://arxiv.org/abs/2406.14228) — 用变异/交叉/选择把单智能体扩展为多智能体系统。
- **Agent Symbolic Learning** — "Symbolic Learning Enables Self-Evolving Agents". Zhou 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2406.18532) — 用语言化"损失/梯度/反向传播"联合优化提示、工具与流水线。
- **Alita** — "Generalist Agent Enabling Scalable Agentic Reasoning...". Qiu 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2505.20286) — 通过按需自动生成/复用 MCP 工具实现自我进化。

#### 3.4 自我改进与自我修改的 Harness

*阶梯的 L3–L4：把 harness 代码——以及优化它的代码——作为自我修改对象。**本主题最直接的体现。***

- **STOP** — "Self-Taught Optimizer: Recursively Self-Improving Code Generation". Zelikman 等. *COLM* 2024. [[论文]](https://arxiv.org/abs/2310.02304) — 种子改进器递归改进自身脚手架代码（权重不变）；优化目标是改进器而非解。
- **Gödel Agent** — "A Self-Referential Agent Framework for Recursive Self-Improvement". Yin 等. *ACL* 2025. [[论文]](https://arxiv.org/abs/2410.04444) — 用 monkey-patching 在运行时动态改写自身逻辑。
- **Darwin Gödel Machine (DGM)** — "Open-Ended Evolution of Self-Improving Agents". Zhang 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2505.22954) — 编程智能体在开放式归档上改写自身代码；SWE-bench 20%→50%。
- **SICA** — "A Self-Improving Coding Agent". Robeyns 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2504.15228) — 取消元/目标之分；智能体为成本/速度/准确率编辑自身代码。
- **Meta-Harness** — "End-to-End Optimization of Model Harnesses". Lee 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2603.28052) — 智能体式提出器通过文件系统搜索 harness *代码*；输出帕累托前沿的 harness 集合。"优化 harness 的 harness"。
- **Self-Harness** — "Harnesses That Improve Themselves". Zhang 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2606.09498) — 提出-评估-接受循环：弱点挖掘 → 有界 harness 提案 → 在 held-in/held-out 上做回归验证。
- **AutoHarness** — "Improving LLM Agents by Automatically Synthesizing a Code Harness". Lou 等. *arXiv* 2026.† — 用带环境反馈的迭代代码精炼自动合成 harness。
- **Hyperagents** — Zhang 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2603.19461) — 由元智能体控制如何修改任务智能体以创建新智能体。

#### 3.5 进化搜索与程序搜索

*把 harness/解当作进化的种群；代码是搜索空间的通用语言。*

- **AlphaEvolve** — "A Coding Agent for Scientific and Algorithmic Discovery". Novikov 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2506.13131) — 带 LLM 集成 + 评估器的进化式编程智能体；标注 `EVOLVE-BLOCK` 区域；发现 48 次乘法的 4×4 矩阵算法。
- **FunSearch** — "Mathematical Discoveries from Program Search with Large Language Models". Romera-Paredes 等. *Nature* 2023. [[论文]](https://www.nature.com/articles/s41586-023-06924-6) — LLM + 评估器进化回路；获得新的 cap-set 与装箱结果。
- **ShinkaEvolve** — "Towards Open-Ended and Sample-Efficient Program Evolution". Lange 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2509.19349) — 通过父代采样、新颖性拒绝采样与 bandit LLM 选择实现样本高效进化。
- **ThetaEvolve** — "Test-time Learning on Open Problems". Wang 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2511.23473) — 将进化搜索与 RL、上下文学习结合。
- **ELM** — "Evolution through Large Models". Lehman 等. *arXiv* 2022.† [[论文]](https://arxiv.org/abs/2206.08896) — 将 LLM diff 模型作为 MAP-Elites 内的变异算子。
- **EoH** — "Evolution of Heuristics". Liu 等. *ICML* 2024. [[论文]](https://arxiv.org/abs/2401.02051) — 协同进化自然语言"思路"与代码，以更少查询胜过 FunSearch。
- **ReEvo** — "Large Language Models as Hyper-Heuristics with Reflective Evolution". Ye 等. *NeurIPS* 2024. [[论文]](https://arxiv.org/abs/2402.01145) — "言语梯度" + 进化用于组合优化。
- **LLaMEA** — "A Large Language Model Evolutionary Algorithm for Generating Metaheuristics". van Stein & Bäck. *IEEE TEVC* 2025. [[论文]](https://arxiv.org/abs/2405.20132) — LLM 生成/变异/选择优化器代码，在 BBOB 上胜过 CMA-ES/DE。
- **QDAIF** — "Quality-Diversity through AI Feedback". Bradley 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2310.13032) — 带 LLM 算子的 MAP-Elites，用于质量-多样性搜索，是对抗多样性坍缩的一种手段。
- **Eureka** — "Human-Level Reward Design via Coding Large Language Models". Ma 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2310.12931) — 对奖励函数代码做上下文进化搜索，含奖励反思。

#### 3.6 与模型权重的联合优化

*阶梯的 L5：harness 改进与权重更新在同一回路中进行的边界。这是焦点内最深的一档；纯权重的自我改进属于[焦点外](#8-毗邻领域刻意排除在焦点外)。*

- **SIA** — "Self Improving AI with Harness & Weight Updates". Hebbar 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2605.27276) — 由 Feedback-Agent 每次迭代决定更新 harness 还是模型权重。
- **SEAL** — "Self-Adapting Language Models". Zweiger 等. *NeurIPS* 2025. [[论文]](https://arxiv.org/abs/2506.10943) — 模型生成自身"自编辑"（微调数据 + 指令），经 SFT 应用并配 RL 回路——模型编辑自己的更新过程。
- **Voyager** — "An Open-Ended Embodied Agent with Large Language Models". Wang 等. *TMLR* 2024. [[论文]](https://arxiv.org/abs/2305.16291) — 借自动课程 + 自我增长的可执行技能库（harness 侧，无权重更新）实现终身学习；技能积累回路的范本。
- **SkillWeaver** — "Web Agents can Self-Improve by Discovering and Honing Skills". Zheng 等. *COLM* 2025. [[论文]](https://arxiv.org/abs/2504.07079) — 智能体把可复用的已调试 API 技能合成进自身 harness；WebArena +31.8%。

### 4. 自动化科研：回路的实战

*自动化科研是把一整套组装完成的自我改进 harness 投入使用的旗舰应用。博客把它放在"工作流设计"下；此处单列，因为它是评测、记忆与自我纠正必须齐备的场景。*

#### 4.1 自主科研智能体

- **The AI Scientist** — "Towards Fully Automated Open-Ended Scientific Discovery". Lu 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2408.06292) — 端到端"想法→代码→实验→论文→评审"流水线。
- **The AI Scientist-v2** — "Workshop-Level Automated Scientific Discovery via Agentic Tree Search". Yamada 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2504.08066) — 免模板后继版；产出首篇通过 workshop 评审的全 AI 论文。
- **Towards end-to-end automation of AI research** — Lu 等. *Nature* 2026. — AI Scientist 系列背后经同行评审的方法学。
- **Agent Laboratory** — "Using LLM Agents as Research Assistants". Schmidgall 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2501.04227) — 三阶段（文献综述→实验→报告）自主框架。
- **ScientistOne** — "Towards Human-Level Autonomous Research via Chain-of-Evidence". Meng 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2605.26340) — 每条论断都须可追溯到证据源并由 Chain-of-Evidence 审计——把可验证性作为 harness 设计约束。
- **CycleResearcher** — "Improving Automated Research via Automated Review". Weng 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2411.00816) — 用迭代 RL 训练策略模型 + 奖励模型。
- **AgentRxiv** — "Towards Collaborative Autonomous Research". Schmidgall & Moor. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2503.18102) — 让智能体实验室互相积累报告的共享预印本服务器。
- **Dolphin** — "Moving Towards Closed-loop Auto-research through Thinking, Practice, and Feedback". Yuan 等. *ACL* 2025. [[论文]](https://arxiv.org/abs/2501.03916) — 带回溯调试的闭环"想法→实验→反馈"。
- **NovelSeek / InternAgent** — "When Agent Becomes the Scientist". InternAgent Team. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2505.16938) — 跨 12 个科学任务的统一闭环多智能体框架。

#### 4.2 科学发现与机器学习工程智能体

- **AI co-scientist** — "Towards an AI co-scientist". Gottweis 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.18864) — Gemini 多智能体系统，锦标赛式进化生物医学假设。
- **AI-Researcher** — "Autonomous Scientific Innovation". Tang 等. *NeurIPS* 2025. [[论文]](https://arxiv.org/abs/2505.18705) — 全流水线自主系统 + Scientist-Bench。
- **Curie** — "Toward Rigorous and Automated Scientific Experimentation with AI Agents". Kon 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.16069) — 面向可靠性与控制的实验严谨性引擎。
- **AIDE** — "AI-Driven Exploration in the Space of Code". Jiang 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.13138) — 借智能体树搜索把机器学习工程当作代码优化。
- **MLGym** — "A New Framework and Benchmark for Advancing AI Research Agents". Nathani 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.14499) — 首个 Gym 式的机器学习科研 RL 环境。

#### 4.3 自动化数据生成与想法生成

- **Autodata** — "An Agentic Data Scientist to Create High Quality Synthetic Data". Kulikov 等. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2606.25996) — 挑战者/弱求解器/强求解器/裁判角色生成"恰到好处"难度的数据，挑战者提示据反馈迭代更新。
- **Can LLMs Generate Novel Research Ideas?** — Si、Yang、Hashimoto. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2409.04109) — 100+ 专家研究：LLM 想法被判更新颖但可行性略低。
- **ResearchAgent** — "Iterative Research Idea Generation over Scientific Literature". Baek 等. *NAACL* 2025. [[论文]](https://arxiv.org/abs/2404.07738) — 借文献图生成/精炼问题、方法与实验设计。

#### 4.4 批判性分析：智能体是科学家了吗？

- **Why LLMs Aren't Scientists Yet** — "Lessons from Four Autonomous Research Attempts". Trehan & Chopra. *arXiv* 2026.† [[论文]](https://arxiv.org/abs/2601.03315) — 记录六种反复出现的失败模式（训练数据默认偏好、实现漂移、记忆退化、过度乐观、领域智能不足、科研品味薄弱）。
- **Early Science Acceleration Experiments with GPT-5** — Bubeck 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2511.16072) — 指出"p-hacking 与 eureka-ing"：在噪声上宣布胜利。
- **Evaluating Sakana's AI Scientist** — "Wishful Thinking or Emerging Reality?". *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2502.14297) — 独立评测：实验失败率高、结果存在幻觉。

### 5. 评测与基准

*自我改进回路的上限由其评估器决定。以下是用来度量并闭合自我改进科研/编程 harness 回路的基准（对应博客附录）。*

#### 5.1 AI 科研与机器学习工程基准

- **PaperBench** — "Evaluating AI's Ability to Replicate AI Research". Starace 等. *ICML* 2025. [[论文]](https://arxiv.org/abs/2504.01848) — 从零复现 20 篇 ICML 2024 论文；8,316 条评分标准。
- **MLE-bench** — "Evaluating Machine Learning Agents on Machine Learning Engineering". Chan 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2410.07095) — 75 个 Kaggle 竞赛，以人类榜单为基线。
- **RE-Bench** — "Evaluating Frontier AI R&D Capabilities Against Human Experts". Wijk 等. *ICML* 2025. [[论文]](https://arxiv.org/abs/2411.15114) — 7 个开放式机器学习研发环境 vs 61 位人类专家。
- **ScienceAgentBench** — "Toward Rigorous Assessment of Language Agents for Data-Driven Scientific Discovery". Chen 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2410.05080) — 来自 44 篇同行评审论文的 102 个任务。
- **CORE-Bench** — "Fostering the Credibility of Published Research...". Siegel 等. *TMLR* 2024. [[论文]](https://arxiv.org/abs/2409.11363) — 来自 90 篇论文的 270 个计算可复现性任务。
- **EXP-Bench** — "Can AI Conduct AI Research Experiments?". Kon 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2505.24785) — 461 个端到端任务；最佳智能体全流程成功率约 0.5%。
- **KernelBench** — "Can LLMs Write Efficient GPU Kernels?". Ouyang 等. *ICML* 2025. [[论文]](https://arxiv.org/abs/2502.10517) — 250 个 PyTorch 工作负载，以 fast_p 评分——快速、可自动化的验证器，非常适合进化式 harness。

#### 5.2 编程与终端智能体基准

- **SWE-bench** — "Can Language Models Resolve Real-World GitHub Issues?". Jimenez 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2310.06770) — 2,294 个真实 issue→PR 任务；编程 harness 自我改进的标准目标（DGM、SICA）。
- **Terminal-Bench** — "Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces". Merrill 等. *arXiv* 2026.† — 人工验证的容器化终端任务；Meta-Harness / Self-Harness 采用的评测。
- **HAL** — "Holistic Agent Leaderboard: The Missing Infrastructure for AI Agent Evaluation". Kapoor 等. *ICLR* 2026. [[论文]](https://arxiv.org/abs/2510.11977) — 跨 9 个基准的标准化、成本感知、第三方榜单。

#### 5.3 验证与验证器

- **Let's Verify Step by Step** — Lightman 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2305.20050) — 过程监督胜过结果监督；发布 PRM800K。
- **Generative Verifiers (GenRM)** — "Reward Modeling as Next-Token Prediction". Zhang 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2408.15240) — 以下一 token 预测实现 CoT 验证。
- **LLMs Cannot Self-Correct Reasoning Yet** — Huang 等. *ICLR* 2024. [[论文]](https://arxiv.org/abs/2310.01798) — 无外部信号的内在自我纠正常常退化——论证验证器应置于自我改进回路*之外*。

### 6. 挑战：安全、奖励作弊与严谨性

*制约真正 RSI 的失败模式。评估器与权限控制应置于进化 harness 的回路**之外**。*

- **Misevolution** — "Your Agent May Misevolve: Emergent Risks in Self-evolving LLM Agents". Shao 等. *ICLR* 2026. [[论文]](https://arxiv.org/abs/2509.26354) — 首个系统研究模型/记忆/工具/工作流路径上的"误进化"——直接针对自进化 harness 的风险。
- **Defining and Characterizing Reward Hacking** — Skalse 等. *NeurIPS* 2022. [[论文]](https://arxiv.org/abs/2209.13085) — 首个形式化定义；"不可作弊"是很强的条件。
- **Scaling Laws for Reward Model Overoptimization** — Gao 等. *ICML* 2023. [[论文]](https://arxiv.org/abs/2210.10760) — 金标准奖励随 KL 退化的函数形式——针对代理目标优化的核心风险。
- **Specification Gaming: the Flip Side of AI Ingenuity** — Krakovna 等. *DeepMind 博客* 2020. [[博客]](https://deepmind.google/discover/blog/specification-gaming-the-flip-side-of-ai-ingenuity/) — 智能体钻目标漏洞的案例集。
- **Sycophancy to Subterfuge** — "Investigating Reward Tampering in Language Models". Denison 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2406.10162) — 从谄媚泛化到奖励函数篡改——自我改进回路编辑自己的奖励。
- **Monitoring Reasoning Models for Misbehavior** — Baker 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2503.11926) — CoT 监控能检测作弊，但对其训练会导致混淆化。
- **AI Agents That Matter** — Kapoor 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2407.01502) — 智能体基准过度关注准确率而非成本，且留出集薄弱——自我改进信号的有效性危机。
- **Leakage and the Reproducibility Crisis in ML-based Science** — Kapoor & Narayanan. *Patterns* 2023. [[论文]](https://arxiv.org/abs/2207.07048) — 跨 17 个领域/329 篇论文的数据泄漏；关乎能否信任自动化科研结果。
- **AgentHarm** — "A Benchmark for Measuring Harmfulness of LLM Agents". Andriushchenko 等. *ICLR* 2025. [[论文]](https://arxiv.org/abs/2410.09024) — 测试拒绝能力与越狱后保留能力的恶意智能体任务。

### 7. 相关综述

- **A Survey on Self-Evolution of Large Language Models** — Tao 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2404.14387) — 将自我进化刻画为经验获取→精炼→更新→评估的循环。
- **The Rise and Potential of LLM Based Agents: A Survey** — Xi 等. *arXiv* 2023.† [[论文]](https://arxiv.org/abs/2309.07864) — 86 页的"大脑/感知/行动"综述。
- **Understanding the Planning of LLM Agents: A Survey** — Huang 等. *arXiv* 2024.† [[论文]](https://arxiv.org/abs/2402.02716) — 智能体规划的分类。
- **A Survey of Context Engineering for Large Language Models** — Mei 等. *arXiv* 2025.† [[论文]](https://arxiv.org/abs/2507.13334) — 1400+ 篇论文的综述，涵盖检索/处理/管理组件。
- **Agent Harness for Large Language Model Agents: A Survey** — Meng 等. *Preprints.org* 2026.† [[仓库]](https://github.com/Gloriaameng/Awesome-Agent-Harness) — 将 harness 形式化为 H=(E,T,C,S,L,V)；互补的系统视角。
- **Automated Design of Agentic Systems: A Survey** — Madžar & Mekterović. *Preprints.org* 2026.† — 沿目标/搜索/表示/反馈四轴综述 33 种 ADAS 方法。

### 8. 毗邻领域（刻意排除在焦点外）

*与原博客一致，纯粹的**模型权重**自我改进与本清单毗邻——但不是其主题。在阶梯的 L5 之下，harness 不再是被优化的对象。下列里程碑工作仅用于标明边界；此小节刻意简短、并不求全。*

- **自博弈与自训练：** SPIN（[2401.01335](https://arxiv.org/abs/2401.01335)）、Self-Rewarding LMs（[2401.10020](https://arxiv.org/abs/2401.10020)）。
- **从零数据出发的强化自我改进：** Absolute Zero / AZR（[2505.03335](https://arxiv.org/abs/2505.03335)）、R-Zero（[2508.05004](https://arxiv.org/abs/2508.05004)）、TTRL（[2504.16084](https://arxiv.org/abs/2504.16084)）。
- **面向推理的 RL（RLVR）：** DeepSeek-R1（[2501.12948](https://arxiv.org/abs/2501.12948)）、DeepSeekMath / GRPO（[2402.03300](https://arxiv.org/abs/2402.03300)）。
- **自举合成数据：** STaR（[2203.14465](https://arxiv.org/abs/2203.14465)）、Self-Instruct（[2212.10560](https://arxiv.org/abs/2212.10560)）、ReST^EM（[2312.06585](https://arxiv.org/abs/2312.06585)）。
- **测试时权重自适应与持续学习：** Test-Time Training（[1909.13231](https://arxiv.org/abs/1909.13231)）、持续学习综述（[2404.16789](https://arxiv.org/abs/2404.16789)）。

> 这些对*完整*的 RSI 愿景很重要，但它们改进的是**模型**而非 **harness**。当它们与 harness 在同一回路中联合优化时，相关工作归入 [§3.6](#36-与模型权重的联合优化)。

### 9. 实践报告与工业界洞见

- **Building Effective Agents** — Anthropic. *工程博客* 2024. [[博客]](https://www.anthropic.com/engineering/building-effective-agents) — 工作流 vs 智能体模式；编排器-工人。
- **Effective Context Engineering for AI Agents** — Anthropic. *工程博客* 2025. [[博客]](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — 上下文作为核心 harness 资源。
- **How We Built Our Multi-Agent Research System** — Anthropic. *工程博客* 2025. [[博客]](https://www.anthropic.com/engineering/multi-agent-research-system) — 具体的编排器-工人子智能体模式。
- **Harness Engineering: Leveraging Codex in an Agent-First World** — OpenAI. *博客* 2026. — 记录 Codex 的 harness 与智能体循环。
- **Towards Self-Driving Codebases** — Cursor. *博客* 2026. — Cursor 对自主、自维护代码库的看法。
- **We Removed 80% of Our Agent's Tools** — Vercel. *工程博客* 2025. — 工具注册表极简化的收益超过模型升级——harness 侧的胜利。
- **Minions: Stripe's One-Shot, End-to-End Coding Agents** — Stripe. *开发博客* 2026. — 规模化的 harness 优先工程。
- **Many SWE-bench-Passing PRs Would Not Be Merged into Main** — METR. *报告* 2026. — 通过基准的 PR 被人类合并的比例低得多——自我改进编程 harness 的评测有效性警示。

---

## 未来方向

当前 harness 只提供了部分解、尚无通用生产级基础设施的开放问题：

1. **弱而模糊的评估器。** 自我改进回路在可测、客观的指标上效果最好。科研品味、新颖性与长期价值难以验证。
2. **上下文与记忆生命周期。** 记忆随智能体自主性增长；上下文工程应成为智能的核心组成部分，而非软件层的附属品。
3. **负面结果。** 文献偏向成功；科研 harness 应让失败尝试易于保留并从中学习。
4. **多样性坍缩。** 进化与 RL 回路倾向利用已知高奖励模式；开放式搜索需要防止种群坍缩的机制。
5. **奖励作弊。** 自我改进回路会优化它被给予的任何信号；评估器与权限控制应置于进化 harness 的回路*之外*。
6. **抽象边界被打破。** 当程序能编辑自身 OS/harness 时，可编辑面、权限控制与安全层必须位于自我改进回路*之外*。
7. **长期成功。** 沙箱 RLVR 很少能刻画可维护性、归属边界、迁移成本或未来调试负担。
8. **人类的角色。** 人类应当*上移*到栈的更高层——在合适的抽象层次与时机提供监督——而非被移出回路。

---

## 贡献

欢迎贡献，但请尊重[范围](#范围说明请先读)：harness（上下文、提示词、工作流、代码、脚手架）必须是自我改进的对象，或其直接使能者/评估器。纯模型权重的方法最多归入 [§8](#8-毗邻领域刻意排除在焦点外)。

请提交 PR：

- 将论文放到最相关的小节，保持格式：`**名称** — "标题". 作者. 会议 年份. [[论文]](链接) — 一句话说明它与 harness 自我改进的关系。`
- 对预印本或非常新的投稿使用 `†`。
- 优先给出论文正式发表的会议；否则给出 arXiv 摘要页。

> **准确性说明：** 标有 `†` 的条目包含近期（2025–2026）预印本，其 arXiv ID、作者或发表信息可能仍会变动。正式引用前请核对链接。

## 引用

如果本清单对你有帮助，欢迎引用启发它的博客文章：

```bibtex
@article{weng2026harness,
  title  = {Harness Engineering for Self-Improvement},
  author = {Weng, Lilian},
  journal = {lilianweng.github.io},
  year   = {2026},
  month  = {July},
  url    = {https://lilianweng.github.io/posts/2026-07-04-harness/}
}
```
