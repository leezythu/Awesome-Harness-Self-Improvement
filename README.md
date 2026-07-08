# Awesome Harness Engineering for Self-Improvement [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

**English** | [中文](README_zh.md)

> A curated reading list on **harness engineering** as the substrate for **recursive self-improvement (RSI)** of LLM agents.
>
> A *harness* is the system that surrounds a base model and orchestrates how it thinks and plans, calls tools and acts, perceives and manages context, stores artifacts, and evaluates its own results. This list is deliberately narrow: it collects work where **the harness itself — its context, prompts, workflow, code, or scaffolding — becomes the object that is optimized, searched, and evolved toward self-improvement**, up to and including the loop where the harness is co-optimized with model weights.

This repository is inspired by Lilian Weng's blog post *["Harness Engineering for Self-Improvement"](https://lilianweng.github.io/posts/2026-07-04-harness/)* (Jul 2026) and mirrors its spine: **Harness Design Patterns → Harness Optimization → Auto-Research → Challenges**.

### Scope (please read)

**In focus** — the harness as the object of self-improvement: self-modifying agents, automated design of agentic systems, evolving context, prompt/workflow optimization, evolutionary program search over harness code, and the joint harness+weights loop. Plus the design-pattern substrate these methods operate on, the auto-research systems that put the loop to work, and the evaluation/safety challenges specific to self-improving harnesses.

**Intentionally out of focus** — following the source blog, purely *model-side* self-improvement (self-play, synthetic data, RLVR, test-time weight training, weak-to-strong) is **not** the subject here. It is listed only as a brief boundary section, [§8 Adjacent Areas](#8-adjacent-areas-intentionally-out-of-focus), so readers know where this list stops.

> ⭐ If you find this useful, please star the repo. PRs for missing papers and corrected links are welcome. `†` denotes a preprint or a very recent arXiv posting whose metadata may still change.

---

## Table of Contents

- [Overview](#overview)
- [The Optimization Ladder](#the-optimization-ladder)
- [Historical Lineage](#historical-lineage)
- [Paper List](#paper-list)
  - [1. Foundations & Position Pieces](#1-foundations--position-pieces)
  - [2. Harness Design Patterns (the substrate)](#2-harness-design-patterns-the-substrate)
    - [2.1 Execution Loop & Self-Correction](#21-execution-loop--self-correction)
    - [2.2 File System as Persistent Memory & State](#22-file-system-as-persistent-memory--state)
    - [2.3 Sub-Agents & Orchestration](#23-sub-agents--orchestration)
    - [2.4 Coding Agent Harnesses (Case Study)](#24-coding-agent-harnesses-case-study)
    - [2.5 Agent Runtimes & Protocols](#25-agent-runtimes--protocols)
  - [3. Harness Optimization (the core)](#3-harness-optimization-the-core)
    - [3.1 Context Engineering (self-evolving context)](#31-context-engineering-self-evolving-context)
    - [3.2 Prompt Optimization](#32-prompt-optimization)
    - [3.3 Workflow Design & Automated Design of Agentic Systems](#33-workflow-design--automated-design-of-agentic-systems)
    - [3.4 Self-Improving & Self-Modifying Harnesses](#34-self-improving--self-modifying-harnesses)
    - [3.5 Evolutionary & Program Search](#35-evolutionary--program-search)
    - [3.6 Joint Optimization with Model Weights](#36-joint-optimization-with-model-weights)
  - [4. Auto-Research: The Loop in Action](#4-auto-research-the-loop-in-action)
    - [4.1 Autonomous Research Agents](#41-autonomous-research-agents)
    - [4.2 Scientific Discovery & ML Engineering Agents](#42-scientific-discovery--ml-engineering-agents)
    - [4.3 Automated Data Generation & Idea Generation](#43-automated-data-generation--idea-generation)
    - [4.4 Critical Analyses: Are Agents Scientists Yet?](#44-critical-analyses-are-agents-scientists-yet)
  - [5. Evaluation & Benchmarks](#5-evaluation--benchmarks)
    - [5.1 AI Research & ML Engineering Benchmarks](#51-ai-research--ml-engineering-benchmarks)
    - [5.2 Coding & Terminal Agent Benchmarks](#52-coding--terminal-agent-benchmarks)
    - [5.3 Verification & Verifiers](#53-verification--verifiers)
  - [6. Challenges: Safety, Reward Hacking & Rigor](#6-challenges-safety-reward-hacking--rigor)
  - [7. Related Surveys](#7-related-surveys)
  - [8. Adjacent Areas (Intentionally Out of Focus)](#8-adjacent-areas-intentionally-out-of-focus)
  - [9. Practitioner Reports & Industry Insights](#9-practitioner-reports--industry-insights)
- [Future Directions](#future-directions)
- [Contributing](#contributing)
- [Citation](#citation)

---

## Overview

The dominant narrative attributes agent capability to the underlying model. This reading list follows a complementary thesis: **the layer between the raw model and the real-world context — the harness — is as decisive as the model's raw intelligence**, and, crucially, it is a layer that can be *made to improve itself*.

Recursive Self-Improvement (RSI) dates back to I. J. Good (1965) and was named by Yudkowsky (2008): an AI uses its current intelligence to improve the machinery that produces its intelligence. In modern AI this loop rarely starts with a model rewriting its own weights. A more practical near-term path runs *through the harness*: the model improves the scaffolding, workflow, context management, and deployment system, which in turn enables a better successor.

The organizing question of this list is therefore narrow and specific: **what happens when the harness becomes an executable, optimizable object?** As models grow more capable, the *object being optimized* climbs a ladder — from hand-written instructions, to structured context, to workflows, to harness code, to the optimizer code itself, and finally to the harness and weights jointly. Everything here is selected for its place on (or direct support of) that ladder.

---

## The Optimization Ladder

The progression of the object being optimized inside a harness system, from most manual to most general. **This ladder is the backbone of the list.**

| Level | Optimized Object | Representative Work | Section |
| ----- | ---------------- | ------------------- | ------- |
| L0 | **Instruction prompts** | APE, OPRO, Promptbreeder, ProTeGi | [3.2](#32-prompt-optimization) |
| L1 | **Structured context** | ACE, Dynamic Cheatsheet, ReasoningBank | [3.1](#31-context-engineering-self-evolving-context) |
| L2 | **Workflow / graph** | ADAS, AFlow, GPTSwarm, AgentSquare | [3.3](#33-workflow-design--automated-design-of-agentic-systems) |
| L3 | **Harness code** | STOP, Gödel Agent, DGM, SICA, Self-Harness | [3.4](#34-self-improving--self-modifying-harnesses) |
| L4 | **Optimizer / meta-harness code** | Meta-Harness, MCE, Meta Agent Search | [3.4](#34-self-improving--self-modifying-harnesses) |
| L5 | **Harness + model weights (jointly)** | SIA, SEAL | [3.6](#36-joint-optimization-with-model-weights) |
| — | *(boundary)* model weights **only** | *self-play, RLVR, synthetic data* | [§8](#8-adjacent-areas-intentionally-out-of-focus) — out of focus |

*As the model becomes more capable, the field moves down this table: toward more complex targets and more general mechanisms, with fewer heuristic rules. Below L5 lies pure model-weight self-improvement, which this list deliberately treats as adjacent rather than core.*

---

## Historical Lineage

| Year | Milestone | Significance |
| ---- | --------- | ------------ |
| 1965 | I. J. Good — "ultraintelligent machine" | First articulation of an intelligence explosion via self-design |
| 2008 | Yudkowsky — "Recursive Self-Improvement" | Names the RSI feedback loop |
| 2023 | Reflexion, Voyager, DSPy, Promptbreeder, STOP, FunSearch | Verbal-RL loops, lifelong skills, compiled pipelines, self-referential improvers |
| 2024 | ADAS, AFlow, GPTSwarm, Agent Symbolic Learning, TextGrad | Automated agent/workflow design; textual "backprop" over harness parts |
| 2025 | AlphaEvolve, ShinkaEvolve, DGM, SICA, ACE, GEPA, SEAL | Evolutionary coding agents; self-modifying agents; evolving context; self-adapting weights |
| 2026 | Meta-Harness, MCE, Self-Harness, AutoHarness, Hyperagents, SIA | Harnesses that optimize harnesses; joint harness+weight loops |

---

## Paper List

### 1. Foundations & Position Pieces

- **Harness Engineering for Self-Improvement** — Lilian Weng. *Lil'Log* 2026. [[blog]](https://lilianweng.github.io/posts/2026-07-04-harness/) — The essay this list is built around; frames the harness as the near-term substrate for RSI.
- **Speculations Concerning the First Ultraintelligent Machine** — I. J. Good. *Advances in Computers* 1965. — Origin of the intelligence-explosion idea.
- **Recursive Self-Improvement** — E. Yudkowsky. *LessWrong* 2008. [[post]](https://www.lesswrong.com/posts/JBadX7rwdcRFzGuju/recursive-self-improvement) — Names and analyzes the RSI feedback loop.
- **Cognitive Architectures for Language Agents (CoALA)** — Sumers, Yao, Narasimhan, Griffiths. *TMLR* 2024. [[paper]](https://arxiv.org/abs/2309.02427) — Organizes agents by memory, action space, and decision loop; a conceptual scaffold for harness components.
- **A Survey of Self-Evolving Agents: What, When, How, and Where to Evolve** — Gao et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2507.21046) — Taxonomy of self-evolving agents across models, memory, tools, and architecture.
- **A Comprehensive Survey of Self-Evolving AI Agents** — Fang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2508.07407) — Bridges foundation models and lifelong agentic systems; proposes "Three Laws of Self-Evolving AI Agents".

### 2. Harness Design Patterns (the substrate)

*These are the recurring structures that self-improving methods operate on. They are not "self-improvement" by themselves, but they define the editable surface the optimizers act upon.*

#### 2.1 Execution Loop & Self-Correction

- **ReAct** — "ReAct: Synergizing Reasoning and Acting in Language Models". Yao et al. *ICLR* 2023. [[paper]](https://arxiv.org/abs/2210.03629) — The canonical Thought→Action→Observation loop; the default loop of most harnesses.
- **Reflexion** — "Language Agents with Verbal Reinforcement Learning". Shinn et al. *NeurIPS* 2023. [[paper]](https://arxiv.org/abs/2303.11366) — Converts feedback into verbal self-reflections stored in episodic memory across trials — the archetypal in-harness self-improvement loop.
- **Self-Refine** — "Iterative Refinement with Self-Feedback". Madaan et al. *NeurIPS* 2023. [[paper]](https://arxiv.org/abs/2303.17651) — Generate→critique→refine within one episode; the self-refine step reused by ADAS-style meta-agents.
- **ReWOO** — "Decoupling Reasoning from Observations for Efficient Augmented Language Models". Xu et al. *arXiv* 2023. [[paper]](https://arxiv.org/abs/2305.18323) — Planner/Worker/Solver decomposition that plans tool calls upfront.
- **autoresearch** — Andrej Karpathy. *GitHub* 2026. [[code]](https://github.com/karpathy/autoresearch) — A clean minimal reference for a goal-oriented plan→execute→observe→improve loop.

#### 2.2 File System as Persistent Memory & State

- **MemGPT** — "Towards LLMs as Operating Systems". Packer et al. *arXiv* 2023. [[paper]](https://arxiv.org/abs/2310.08560) — OS-inspired virtual context management, paging between context and external storage.
- **Agent Workflow Memory (AWM)** — Wang, Mao, Fried, Neubig. *ICML* 2025. [[paper]](https://arxiv.org/abs/2409.07429) — Induces and reuses reusable "workflows" as durable procedural memory that grows with experience.
- **FS-Researcher** — "Test-Time Scaling for Long-Horizon Research Tasks with File-System-Based Agents". Zhu, Xu et al. *ACL* 2026.† — Uses a persistent filesystem workspace as durable external memory for deep research.
- **Memp** — "Exploring Agent Procedural Memory". Fang et al. *ACL Findings* 2026.† [[paper]](https://arxiv.org/abs/2508.06433) — Distills trajectories into script-like procedures with build/retrieve/update strategies; supports transfer to weaker models.

#### 2.3 Sub-Agents & Orchestration

- **AutoGen** — "Enabling Next-Gen LLM Applications via Multi-Agent Conversation". Wu et al. *COLM* 2024. [[paper]](https://arxiv.org/abs/2308.08155) — Conversable-agent framework; a common substrate for orchestration search.
- **MetaGPT** — "Meta Programming for a Multi-Agent Collaborative Framework". Hong et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2308.00352) — Encodes SOPs into an assembly line of role agents with executable feedback.
- **Chain-of-Agents** — "Large Language Models Collaborating on Long-Context Tasks". Zhang et al. *NeurIPS* 2024. [[paper]](https://arxiv.org/abs/2406.02818) — Worker agents process chunks and pass messages to a manager for long context.
- **AOrchestra** — "Automating Sub-Agent Creation for Agentic Orchestration". Ruan et al. *arXiv* 2026.† — Automatically creates sub-agents for orchestration — orchestration as an optimizable surface.

#### 2.4 Coding Agent Harnesses (Case Study)

The core interface of mainstream coding agents has stabilized across Claude Code, Codex, OpenHands, and Cursor-style agents: a loop with file-system tools (`glob`, `grep`, `read`, `write`, `edit`, `apply_patch`), shell execution, git/LSP IO, external context (MCP, Skills), web search, and agent delegation. This is the most common editable surface for harness self-improvement.

- **SWE-agent** — "Agent-Computer Interfaces Enable Automated Software Engineering". Yang, Jimenez et al. *NeurIPS* 2024. [[paper]](https://arxiv.org/abs/2405.15793) — Introduces the agent-computer interface (ACI); shows interface design outweighs raw model capability.
- **OpenHands / OpenDevin** — "An Open Platform for AI Software Developers as Generalist Agents". Wang et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2407.16741) — Open platform for code/CLI/web agents with sandboxed execution.
- **CodeAct** — "Executable Code Actions Elicit Better LLM Agents". Wang et al. *ICML* 2024. [[paper]](https://arxiv.org/abs/2402.01030) — Uses executable Python code as a unified action space with a self-debugging interpreter.
- **Agentless** — "Demystifying LLM-based Software Engineering Agents". Xia et al. *FSE* 2025. [[paper]](https://arxiv.org/abs/2407.01489) — A fixed localize→repair→validate pipeline can rival autonomous agents at lower cost — a harness-design ablation.
- **OPENDEV** — "Building Effective AI Coding Agents for the Terminal: Scaffolding, Harness, Context Engineering, and Lessons Learned". Bui. *arXiv* 2026.† — Practitioner analysis of terminal coding harness design.

#### 2.5 Agent Runtimes & Protocols

- **AIOS** — "LLM Agent Operating System". Mei et al. *COLM* 2025. [[paper]](https://arxiv.org/abs/2403.16971) — LLM-specific OS kernel: scheduling, context/memory/storage management, tool service, access control.
- **GoEX** — "Perspectives and Designs Towards a Runtime for Autonomous LLM Applications". Patil et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2404.06921) — Runtime designs (undo / damage-confinement) for safely executing self-directed harness actions.
- **Model Context Protocol (MCP)** — "MCP: Landscape, Security Threats, and Future Research Directions". Hou et al. *arXiv* 2025. [[paper]](https://arxiv.org/abs/2503.23278) — The standardizing tool↔harness interface that self-evolving agents (e.g. Alita) extend at runtime.
- **Repo2Run** — "Automated Building Executable Environment for Code Repository at Scale". Hu et al. *arXiv* 2025.† — Automatically builds executable environments, a prerequisite for evaluable harness self-improvement.

### 3. Harness Optimization (the core)

*This is the heart of the list. Each subsection corresponds to a rung of the [optimization ladder](#the-optimization-ladder).*

#### 3.1 Context Engineering (self-evolving context)

- **ACE** — "Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models". Zhang et al. *ICLR* 2026. [[paper]](https://arxiv.org/abs/2510.04618) — Treats context as an evolving playbook via Generator/Reflector/Curator with incremental delta updates, avoiding context collapse.
- **MCE** — "Meta Context Engineering via Agentic Skill Evolution". Ye et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2601.21557) — Bi-level framework co-evolving context-management *skills* (meta) and *context artifacts* (base, as files/code) — separates the mechanism from the content.
- **Dynamic Cheatsheet** — "Test-Time Learning with Adaptive Memory". Suzgun et al. *EACL* 2026.† [[paper]](https://arxiv.org/abs/2504.07952) — Persistent self-curated memory of strategies/snippets at inference; a direct precursor to ACE.
- **ReasoningBank** — "Scaling Agent Self-Evolving with Reasoning Memory". Ouyang et al. *ICLR* 2026.† [[paper]](https://arxiv.org/abs/2509.25140) — Distills generalizable reasoning strategies from successes *and* failures; introduces memory-aware test-time scaling.
- **ExpeL** — "LLM Agents Are Experiential Learners". Zhao et al. *AAAI* 2024. [[paper]](https://arxiv.org/abs/2308.10144) — Gathers experiences, extracts NL insights into a growing context store, recalls at inference without fine-tuning.
- **MemAct** — "Memory as Action: Autonomous Context Curation for Long-Horizon Agentic Tasks". Zhang et al. *ACL Findings* 2026.† [[paper]](https://arxiv.org/abs/2510.12635) — Reframes working-memory management as learnable policy actions trained end-to-end.

#### 3.2 Prompt Optimization

*L0 of the ladder: the harness's instruction layer as the object of optimization.*

- **APE** — "Large Language Models Are Human-Level Prompt Engineers". Zhou et al. *ICLR* 2023. [[paper]](https://arxiv.org/abs/2211.01910) — Treats the instruction as a program, proposing/scoring candidates via search.
- **OPRO** — "Large Language Models as Optimizers". Yang et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2309.03409) — "Optimization by PROmpting": generate new solutions from a meta-prompt of prior (solution, score) pairs.
- **EvoPrompt** — "Connecting LLMs with Evolutionary Algorithms Yields Powerful Prompt Optimizers". Guo et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2309.08532) — Runs GA/DE over a population of discrete prompts with LLM mutation/crossover.
- **Promptbreeder** — "Self-Referential Self-Improvement via Prompt Evolution". Fernando et al. *arXiv* 2023.† [[paper]](https://arxiv.org/abs/2309.16797) — Evolves both task-prompts *and* the mutation-prompts that modify them — self-referential prompt improvement.
- **ProTeGi** — "Automatic Prompt Optimization with 'Gradient Descent' and Beam Search". Pryzant et al. *EMNLP* 2023. [[paper]](https://arxiv.org/abs/2305.03495) — Coined "textual gradients": LLM critiques as NL gradients editing prompts.
- **DSPy** — "Compiling Declarative Language Model Calls into Self-Improving Pipelines". Khattab et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.03714) — Programming model treating LM pipelines as optimizable text-transformation graphs.
- **MIPROv2** — "Optimizing Instructions and Demonstrations for Multi-Stage Language Model Programs". Opsahl-Ong et al. *EMNLP* 2024. [[paper]](https://arxiv.org/abs/2406.11695) — Jointly bootstraps few-shot examples and proposes instructions via Bayesian optimization.
- **TextGrad** — "Automatic 'Differentiation' via Text". Yuksekgonul et al. *Nature* 2025. [[paper]](https://arxiv.org/abs/2406.07496) — Backpropagates textual feedback through compound AI systems, PyTorch-style.
- **GEPA** — "Reflective Prompt Evolution Can Outperform Reinforcement Learning". Agrawal et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2507.19457) — Genetic-Pareto reflective optimizer reading full traces; beats RL with up to 35× fewer rollouts.

#### 3.3 Workflow Design & Automated Design of Agentic Systems

*L2 of the ladder: the agentic workflow/graph as the object of search.*

- **ADAS / Meta Agent Search** — "Automated Design of Agentic Systems". Hu, Lu, Clune. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2408.08435) — A meta-agent programs ever-better agents in code over a growing archive.
- **AFlow** — "Automating Agentic Workflow Generation". Zhang et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.10762) — Workflow optimization as MCTS over code-represented graphs.
- **GPTSwarm** — "Language Agents as Optimizable Graphs". Zhuge et al. *ICML* 2024. [[paper]](https://arxiv.org/abs/2402.16823) — Agents as computational graphs; node-level prompt + edge-level REINFORCE optimization.
- **AgentSquare** — "Automatic LLM Agent Search in Modular Design Space". Shang et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.06153) — Searches a modular Planning/Reasoning/Tool-use/Memory space via evolution + recombination.
- **MaAS** — "Multi-agent Architecture Search via Agentic Supernet". Zhang et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2502.04180) — Optimizes a probabilistic "agentic supernet" for cost-adaptive, query-dependent systems.
- **MASS** — "Multi-Agent Design: Optimizing Agents with Better Prompts and Topologies". Zhou et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.02533) — Interleaved multi-stage search over prompts and topologies.
- **ScoreFlow** — "Mastering LLM Agent Workflows via Score-based Preference Optimization". Wang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.04306) — Continuous gradient-based workflow optimization via Score-DPO.
- **FlowReasoner** — "Reinforcing Query-Level Meta-Agents". Gao et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2504.15257) — A reasoning meta-agent (RL-tuned) that designs a bespoke multi-agent system per query.
- **EvoAgent** — "Towards Automatic Multi-Agent Generation via Evolutionary Algorithms". Yuan et al. *NAACL* 2025. [[paper]](https://arxiv.org/abs/2406.14228) — Mutation/crossover/selection to extend one agent into a multi-agent system.
- **Agent Symbolic Learning** — "Symbolic Learning Enables Self-Evolving Agents". Zhou et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2406.18532) — Language "loss/gradients/back-prop" to jointly optimize prompts, tools, and pipeline.
- **Alita** — "Generalist Agent Enabling Scalable Agentic Reasoning with Minimal Predefinition and Maximal Self-Evolution". Qiu et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.20286) — Self-evolves by autonomously generating/reusing MCP tools on the fly.

#### 3.4 Self-Improving & Self-Modifying Harnesses

*L3–L4 of the ladder: the harness code — and the code that optimizes it — as the object of self-modification. **The most direct realization of the theme.***

- **STOP** — "Self-Taught Optimizer: Recursively Self-Improving Code Generation". Zelikman et al. *COLM* 2024. [[paper]](https://arxiv.org/abs/2310.02304) — A seed improver recursively improves its own scaffolding code (weights fixed); the improver, not the solution, is the target.
- **Gödel Agent** — "A Self-Referential Agent Framework for Recursive Self-Improvement". Yin et al. *ACL* 2025. [[paper]](https://arxiv.org/abs/2410.04444) — Uses monkey-patching to dynamically rewrite its own logic at runtime.
- **Darwin Gödel Machine (DGM)** — "Open-Ended Evolution of Self-Improving Agents". Zhang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.22954) — A coding agent rewrites its own codebase over an open-ended archive; SWE-bench 20%→50%.
- **SICA** — "A Self-Improving Coding Agent". Robeyns et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2504.15228) — Removes the meta/target distinction; the agent edits its own codebase for cost/speed/accuracy.
- **Meta-Harness** — "End-to-End Optimization of Model Harnesses". Lee et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2603.28052) — An agentic proposer searches over harness *code* via the file system; returns a Pareto frontier of harnesses. "A harness for optimizing harnesses."
- **Self-Harness** — "Harnesses That Improve Themselves". Zhang et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2606.09498) — A propose-evaluate-accept loop: weakness mining → bounded harness proposal → regression validation on held-in/held-out splits.
- **AutoHarness** — "Improving LLM Agents by Automatically Synthesizing a Code Harness". Lou et al. *arXiv* 2026.† — Uses iterative code refinement with environment feedback to auto-synthesize a code harness.
- **Hyperagents** — Zhang et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2603.19461) — A meta-agent controls how to modify task agents to create new ones.

#### 3.5 Evolutionary & Program Search

*The harness/solution as an evolving population; code is the universal language for the search space.*

- **AlphaEvolve** — "A Coding Agent for Scientific and Algorithmic Discovery". Novikov et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2506.13131) — Evolutionary coding agent with LLM ensemble + evaluators; marked `EVOLVE-BLOCK` regions; discovered a 48-mult 4×4 matrix algorithm.
- **FunSearch** — "Mathematical Discoveries from Program Search with Large Language Models". Romera-Paredes et al. *Nature* 2023. [[paper]](https://www.nature.com/articles/s41586-023-06924-6) — LLM + evaluator in an evolutionary loop; new cap-set and bin-packing results.
- **ShinkaEvolve** — "Towards Open-Ended and Sample-Efficient Program Evolution". Lange et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2509.19349) — Parent sampling, novelty rejection sampling, and bandit LLM selection for sample-efficient evolution.
- **ThetaEvolve** — "Test-time Learning on Open Problems". Wang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2511.23473) — Combines evolutionary search with RL and in-context learning.
- **ELM** — "Evolution through Large Models". Lehman et al. *arXiv* 2022.† [[paper]](https://arxiv.org/abs/2206.08896) — LLM diff model as a mutation operator inside MAP-Elites.
- **EoH** — "Evolution of Heuristics: Efficient Automatic Algorithm Design Using LLMs". Liu et al. *ICML* 2024. [[paper]](https://arxiv.org/abs/2401.02051) — Coevolves NL "thoughts" and code, beating FunSearch with fewer queries.
- **ReEvo** — "Large Language Models as Hyper-Heuristics with Reflective Evolution". Ye et al. *NeurIPS* 2024. [[paper]](https://arxiv.org/abs/2402.01145) — "Verbal gradients" + evolution for combinatorial optimization.
- **LLaMEA** — "A Large Language Model Evolutionary Algorithm for Generating Metaheuristics". van Stein & Bäck. *IEEE TEVC* 2025. [[paper]](https://arxiv.org/abs/2405.20132) — LLM generates/mutates/selects optimizer code beating CMA-ES/DE on BBOB.
- **QDAIF** — "Quality-Diversity through AI Feedback". Bradley et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.13032) — MAP-Elites with LLM operators for QD search, a defense against diversity collapse.
- **Eureka** — "Human-Level Reward Design via Coding Large Language Models". Ma et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.12931) — In-context evolutionary search over reward-function code with reward reflection.

#### 3.6 Joint Optimization with Model Weights

*L5 of the ladder: the boundary where harness improvement and weight updates happen in the same loop. This is the deepest in-focus rung; pure weight-only self-improvement is [out of focus](#8-adjacent-areas-intentionally-out-of-focus).*

- **SIA** — "Self Improving AI with Harness & Weight Updates". Hebbar et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2605.27276) — A Feedback-Agent decides, per iteration, whether to update the harness or the model weights.
- **SEAL** — "Self-Adapting Language Models". Zweiger et al. *NeurIPS* 2025. [[paper]](https://arxiv.org/abs/2506.10943) — The model generates its own "self-edits" (finetuning data + directives), applied via SFT with an RL loop — the model editing its own update process.
- **Voyager** — "An Open-Ended Embodied Agent with Large Language Models". Wang et al. *TMLR* 2024. [[paper]](https://arxiv.org/abs/2305.16291) — Lifelong learning via automatic curriculum + a self-growing executable skill library (harness-side, no weight updates); the canonical skill-accumulation loop.
- **SkillWeaver** — "Web Agents can Self-Improve by Discovering and Honing Skills". Zheng et al. *COLM* 2025. [[paper]](https://arxiv.org/abs/2504.07079) — Agents synthesize reusable, debugged API skills into their harness; +31.8% on WebArena.

### 4. Auto-Research: The Loop in Action

*Auto-research is the flagship application where a fully assembled self-improving harness is put to work. The blog treats this under "Workflow Design"; here it is its own section because it is where evaluation, memory, and self-correction must all come together.*

#### 4.1 Autonomous Research Agents

- **The AI Scientist** — "Towards Fully Automated Open-Ended Scientific Discovery". Lu et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2408.06292) — End-to-end idea→code→experiment→paper→review pipeline.
- **The AI Scientist-v2** — "Workshop-Level Automated Scientific Discovery via Agentic Tree Search". Yamada et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2504.08066) — Template-free successor; produced the first fully-AI paper to pass workshop peer review.
- **Towards end-to-end automation of AI research** — Lu et al. *Nature* 2026. — Peer-reviewed methodology behind the AI Scientist line.
- **Agent Laboratory** — "Using LLM Agents as Research Assistants". Schmidgall et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2501.04227) — Three-stage (lit review → experimentation → report) autonomous framework.
- **ScientistOne** — "Towards Human-Level Autonomous Research via Chain-of-Evidence". Meng et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2605.26340) — Every claim must trace to an evidence source, audited by Chain-of-Evidence checks — verifiability as a harness design constraint.
- **CycleResearcher** — "Improving Automated Research via Automated Review". Weng et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2411.00816) — Policy model + reward model trained with iterative RL on review/research datasets.
- **AgentRxiv** — "Towards Collaborative Autonomous Research". Schmidgall & Moor. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2503.18102) — A shared preprint server letting agent labs build on each other's reports.
- **Dolphin** — "Moving Towards Closed-loop Auto-research through Thinking, Practice, and Feedback". Yuan et al. *ACL* 2025. [[paper]](https://arxiv.org/abs/2501.03916) — Closed-loop idea→experiment→feedback with traceback-guided debugging.
- **NovelSeek / InternAgent** — "When Agent Becomes the Scientist". InternAgent Team. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.16938) — Unified closed-loop multi-agent framework across 12 scientific tasks.

#### 4.2 Scientific Discovery & ML Engineering Agents

- **AI co-scientist** — "Towards an AI co-scientist". Gottweis et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.18864) — Gemini multi-agent system that tournament-evolves biomedical hypotheses.
- **AI-Researcher** — "Autonomous Scientific Innovation". Tang et al. *NeurIPS* 2025. [[paper]](https://arxiv.org/abs/2505.18705) — Full-pipeline autonomous system + Scientist-Bench.
- **Curie** — "Toward Rigorous and Automated Scientific Experimentation with AI Agents". Kon et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.16069) — An Experimental Rigor Engine for reliability and control.
- **AIDE** — "AI-Driven Exploration in the Space of Code". Jiang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.13138) — ML engineering as code optimization via agentic tree search.
- **MLGym** — "A New Framework and Benchmark for Advancing AI Research Agents". Nathani et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.14499) — First Gym-style RL environment for ML research tasks.

#### 4.3 Automated Data Generation & Idea Generation

- **Autodata** — "An Agentic Data Scientist to Create High Quality Synthetic Data". Kulikov et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2606.25996) — Challenger/weak-solver/strong-solver/judge roles produce "just right" difficulty data, with the challenger prompt updated iteratively from feedback.
- **Can LLMs Generate Novel Research Ideas?** — Si, Yang, Hashimoto. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2409.04109) — 100+ expert study: LLM ideas judged more novel but slightly less feasible.
- **ResearchAgent** — "Iterative Research Idea Generation over Scientific Literature". Baek et al. *NAACL* 2025. [[paper]](https://arxiv.org/abs/2404.07738) — Generates/refines problems, methods, and designs via a literature graph.

#### 4.4 Critical Analyses: Are Agents Scientists Yet?

- **Why LLMs Aren't Scientists Yet** — "Lessons from Four Autonomous Research Attempts". Trehan & Chopra. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2601.03315) — Documents six recurring failure modes (training-data defaults, implementation drift, memory degradation, over-optimism, insufficient domain intelligence, weak scientific taste).
- **Early Science Acceleration Experiments with GPT-5** — Bubeck et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2511.16072) — Notes "p-hacking and eureka-ing": declaring victory on noise.
- **Evaluating Sakana's AI Scientist** — "Wishful Thinking or Emerging Reality?". *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.14297) — Independent eval: high experiment failure rate, hallucinated results.

### 5. Evaluation & Benchmarks

*A self-improvement loop is only as good as its evaluator. These are the benchmarks used to measure — and to close the loop of — self-improving research/coding harnesses (the blog's appendix).*

#### 5.1 AI Research & ML Engineering Benchmarks

- **PaperBench** — "Evaluating AI's Ability to Replicate AI Research". Starace et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2504.01848) — Replicate 20 ICML 2024 papers from scratch; 8,316 rubrics.
- **MLE-bench** — "Evaluating Machine Learning Agents on Machine Learning Engineering". Chan et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.07095) — 75 Kaggle competitions with human-leaderboard baselines.
- **RE-Bench** — "Evaluating Frontier AI R&D Capabilities Against Human Experts". Wijk et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2411.15114) — 7 open-ended ML R&D environments vs 61 human experts.
- **ScienceAgentBench** — "Toward Rigorous Assessment of Language Agents for Data-Driven Scientific Discovery". Chen et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.05080) — 102 tasks from 44 peer-reviewed papers.
- **CORE-Bench** — "Fostering the Credibility of Published Research...". Siegel et al. *TMLR* 2024. [[paper]](https://arxiv.org/abs/2409.11363) — 270 computational-reproducibility tasks from 90 papers.
- **EXP-Bench** — "Can AI Conduct AI Research Experiments?". Kon et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.24785) — 461 end-to-end tasks; best agents ~0.5% full success.
- **KernelBench** — "Can LLMs Write Efficient GPU Kernels?". Ouyang et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2502.10517) — 250 PyTorch workloads scored by fast_p — a fast, automatable verifier well-suited to evolutionary harnesses.

#### 5.2 Coding & Terminal Agent Benchmarks

- **SWE-bench** — "Can Language Models Resolve Real-World GitHub Issues?". Jimenez et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.06770) — 2,294 real issue→PR tasks; the standard target for coding-harness self-improvement (DGM, SICA).
- **Terminal-Bench** — "Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces". Merrill et al. *arXiv* 2026.† — Human-verified containerized terminal tasks; the eval used by Meta-Harness / Self-Harness.
- **HAL** — "Holistic Agent Leaderboard: The Missing Infrastructure for AI Agent Evaluation". Kapoor et al. *ICLR* 2026. [[paper]](https://arxiv.org/abs/2510.11977) — Standardized, cost-aware, third-party leaderboard across 9 benchmarks.

#### 5.3 Verification & Verifiers

- **Let's Verify Step by Step** — Lightman et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2305.20050) — Process supervision beats outcome supervision; releases PRM800K.
- **Generative Verifiers (GenRM)** — "Reward Modeling as Next-Token Prediction". Zhang et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2408.15240) — CoT verification via next-token prediction.
- **LLMs Cannot Self-Correct Reasoning Yet** — Huang et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.01798) — Intrinsic self-correction often degrades without an external signal — motivates verifiers *outside* the self-improvement loop.

### 6. Challenges: Safety, Reward Hacking & Rigor

*The failure modes that gate real RSI. The evaluator and permission control should sit **outside** the loop that evolves the harness.*

- **Misevolution** — "Your Agent May Misevolve: Emergent Risks in Self-evolving LLM Agents". Shao et al. *ICLR* 2026. [[paper]](https://arxiv.org/abs/2509.26354) — First systematic study of misevolution across model/memory/tool/workflow paths — directly about self-evolving harness risk.
- **Defining and Characterizing Reward Hacking** — Skalse et al. *NeurIPS* 2022. [[paper]](https://arxiv.org/abs/2209.13085) — First formal definition; "unhackability" is a strong condition.
- **Scaling Laws for Reward Model Overoptimization** — Gao et al. *ICML* 2023. [[paper]](https://arxiv.org/abs/2210.10760) — Functional forms for gold-reward degradation vs KL — the core risk of optimizing against a proxy.
- **Specification Gaming: the Flip Side of AI Ingenuity** — Krakovna et al. *DeepMind blog* 2020. [[blog]](https://deepmind.google/discover/blog/specification-gaming-the-flip-side-of-ai-ingenuity/) — Catalog of agents exploiting objective loopholes.
- **Sycophancy to Subterfuge** — "Investigating Reward Tampering in Language Models". Denison et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2406.10162) — Generalization from sycophancy to reward-function tampering — a self-improvement loop editing its own reward.
- **Monitoring Reasoning Models for Misbehavior** — Baker et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2503.11926) — CoT monitoring detects hacking, but training against the monitor yields obfuscation.
- **AI Agents That Matter** — Kapoor et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2407.01502) — Agent benchmarks over-focus on accuracy vs cost with weak holdouts — a validity crisis for self-improvement signals.
- **Leakage and the Reproducibility Crisis in ML-based Science** — Kapoor & Narayanan. *Patterns* 2023. [[paper]](https://arxiv.org/abs/2207.07048) — Data leakage across 17 fields / 329 papers; relevant to trusting auto-research results.
- **AgentHarm** — "A Benchmark for Measuring Harmfulness of LLM Agents". Andriushchenko et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.09024) — Malicious agent tasks testing refusal + retained capability.

### 7. Related Surveys

- **A Survey on Self-Evolution of Large Language Models** — Tao et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2404.14387) — Self-evolution as cycles of experience acquisition → refinement → updating → evaluation.
- **The Rise and Potential of LLM Based Agents: A Survey** — Xi et al. *arXiv* 2023.† [[paper]](https://arxiv.org/abs/2309.07864) — 86-page brain/perception/action survey.
- **Understanding the Planning of LLM Agents: A Survey** — Huang et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2402.02716) — Taxonomy of agent planning.
- **A Survey of Context Engineering for Large Language Models** — Mei et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2507.13334) — 1400+ paper survey of retrieval/processing/management components.
- **Agent Harness for Large Language Model Agents: A Survey** — Meng et al. *Preprints.org* 2026.† [[repo]](https://github.com/Gloriaameng/Awesome-Agent-Harness) — Formalizes the harness as H=(E,T,C,S,L,V); complementary systems-oriented view.
- **Automated Design of Agentic Systems: A Survey** — Madžar & Mekterović. *Preprints.org* 2026.† — Surveys 33 ADAS methods along target/search/representation/feedback axes.

### 8. Adjacent Areas (Intentionally Out of Focus)

*Following the source blog, purely **model-weight** self-improvement is adjacent to — but not the subject of — this list. Below L5 on the ladder, the harness is no longer the object being optimized. These landmark works are listed only to mark the boundary; the list is intentionally short and non-exhaustive.*

- **Self-play & self-training:** SPIN ([2401.01335](https://arxiv.org/abs/2401.01335)), Self-Rewarding LMs ([2401.10020](https://arxiv.org/abs/2401.10020)).
- **Reinforced self-improvement from zero data:** Absolute Zero / AZR ([2505.03335](https://arxiv.org/abs/2505.03335)), R-Zero ([2508.05004](https://arxiv.org/abs/2508.05004)), TTRL ([2504.16084](https://arxiv.org/abs/2504.16084)).
- **RL for reasoning (RLVR):** DeepSeek-R1 ([2501.12948](https://arxiv.org/abs/2501.12948)), DeepSeekMath / GRPO ([2402.03300](https://arxiv.org/abs/2402.03300)).
- **Bootstrapped synthetic data:** STaR ([2203.14465](https://arxiv.org/abs/2203.14465)), Self-Instruct ([2212.10560](https://arxiv.org/abs/2212.10560)), ReST^EM ([2312.06585](https://arxiv.org/abs/2312.06585)).
- **Test-time weight adaptation & continual learning:** Test-Time Training ([1909.13231](https://arxiv.org/abs/1909.13231)), continual-learning survey ([2404.16789](https://arxiv.org/abs/2404.16789)).

> These matter for the *full* RSI vision, but they improve the **model**, not the **harness**. When they are co-optimized with the harness in one loop, the relevant work lives in [§3.6](#36-joint-optimization-with-model-weights) instead.

### 9. Practitioner Reports & Industry Insights

- **Building Effective Agents** — Anthropic. *Engineering blog* 2024. [[blog]](https://www.anthropic.com/engineering/building-effective-agents) — Workflow vs agent patterns; orchestrator-worker.
- **Effective Context Engineering for AI Agents** — Anthropic. *Engineering blog* 2025. [[blog]](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — Context as the core harness resource.
- **How We Built Our Multi-Agent Research System** — Anthropic. *Engineering blog* 2025. [[blog]](https://www.anthropic.com/engineering/multi-agent-research-system) — Concrete orchestrator-worker subagent pattern.
- **Harness Engineering: Leveraging Codex in an Agent-First World** — OpenAI. *Blog* 2026. — Documents the Codex harness and agent loop.
- **Towards Self-Driving Codebases** — Cursor. *Blog* 2026. — Cursor's view on autonomous, self-maintaining repos.
- **We Removed 80% of Our Agent's Tools** — Vercel. *Engineering blog* 2025. — Tool-registry minimalism outperformed model upgrades — a harness-side win.
- **Minions: Stripe's One-Shot, End-to-End Coding Agents** — Stripe. *Dev blog* 2026. — Harness-first engineering at scale.
- **Many SWE-bench-Passing PRs Would Not Be Merged into Main** — METR. *Report* 2026. — Benchmark-passing PRs have far lower human merge rates — an evaluation-validity warning for self-improving coding harnesses.

---

## Future Directions

Open problems where current harnesses provide partial solutions but no general, production-grade infrastructure:

1. **Weak and fuzzy evaluators.** Self-improvement loops work best with measurable, objective metrics. Research taste, novelty, and long-term value are hard to verify.
2. **Context and memory lifecycle.** Memory grows with agent autonomy; context engineering should become a core part of intelligence rather than a software-layer afterthought.
3. **Negative results.** Literature is biased toward successes; a research harness should make failed attempts easy to preserve and learn from.
4. **Diversity collapse.** Evolutionary and RL loops exploit known high-reward patterns; open-ended search needs mechanisms against population collapse.
5. **Reward hacking.** A self-improvement loop optimizes whatever signal it is given; evaluators and permission control should sit *outside* the loop that evolves the harness.
6. **Broken abstraction boundaries.** When a program can edit its own OS/harness, the editable surface, permission control, and security layers must live *outside* the self-improvement loop.
7. **Long-term success.** Sandbox RLVR rarely captures maintainability, ownership boundaries, migration cost, or future debugging burden.
8. **The role of humans.** Humans should move *up* the stack — providing oversight at the right abstraction level and the right time — not be removed from the loop.

---

## Contributing

Contributions are welcome, but please respect the [scope](#scope-please-read): the harness (context, prompts, workflow, code, scaffolding) must be the object of self-improvement, or a direct enabler/evaluator of it. Purely model-weight methods belong in [§8](#8-adjacent-areas-intentionally-out-of-focus) at most.

Please open a PR that:

- Adds the paper under the most relevant subsection, keeping the format: `**Name** — "Title". Authors. Venue Year. [[paper]](link) — one-line description tying it to harness self-improvement.`
- Uses `†` for preprints or very recent postings.
- Prefers the canonical venue where a paper is published; otherwise the arXiv abstract page.

> **Accuracy note:** entries marked `†` include recent (2025–2026) preprints whose arXiv IDs, authorship, or venues may still change. Please verify links before citing in formal work.

## Citation

If you find this list useful, please consider citing the blog post that inspired it:

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
