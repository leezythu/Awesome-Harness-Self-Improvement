# Awesome Harness Engineering for Self-Improvement [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contributing)

**English** | [中文](README_zh.md)

> A curated reading list on **harness engineering** as the substrate for **recursive self-improvement (RSI)** of LLM agents.
>
> A *harness* is the system that surrounds a base model and orchestrates how it thinks and plans, calls tools and acts, perceives and manages context, stores artifacts, and evaluates its own results. This list is deliberately narrow: it collects work where **the harness itself — its context, prompts, workflow, tools, or code — is the object that improves itself**, i.e. where the system optimizes, searches, or evolves its own scaffolding, up to and including the loop where the harness is co-optimized with model weights.

This repository is inspired by Lilian Weng's blog post *["Harness Engineering for Self-Improvement"](https://lilianweng.github.io/posts/2026-07-04-harness/)* (Jul 2026).

### Scope (please read)

**In focus** — the harness *improving itself*: self-modifying agents, automated optimization/search of agentic workflows, self-evolving context and memory, prompt/workflow optimization, evolutionary program search over harness/agent code, and the joint harness+weights loop. Plus the auto-research systems that close a self-improvement loop, the evaluators that such loops optimize against, and the safety/rigor failure modes specific to self-evolving harnesses.

**Out of focus** — this list distinguishes **self-improvement** from **design**:

- *Harness design patterns* (how to hand-build a good harness — ReAct, coding-agent harnesses, multi-agent frameworks, runtimes/protocols) are the editable *substrate* that self-improving methods act on, but are **design, not self-improvement**. They are noted only as a boundary in [§7 Adjacent Areas](#7-adjacent-areas-out-of-focus).
- *Purely model-side* self-improvement (self-play, synthetic data, RLVR, test-time weight training) improves the **model, not the harness**; also noted only in [§7](#7-adjacent-areas-out-of-focus).

> ⭐ If you find this useful, please star the repo. PRs for missing papers and corrected links are welcome. `†` denotes a preprint or a very recent arXiv posting whose metadata may still change.

---

## Table of Contents

- [Overview](#overview)
- [The Optimization Ladder](#the-optimization-ladder)
- [Historical Lineage](#historical-lineage)
- [Paper List](#paper-list)
  - [1. Foundations & Position Pieces](#1-foundations--position-pieces)
  - [2. Harness Optimization (the core)](#2-harness-optimization-the-core)
    - [2.1 Self-Evolving Context & Memory](#21-self-evolving-context--memory)
    - [2.2 Prompt Optimization](#22-prompt-optimization)
    - [2.3 Automated Optimization of Agentic Workflows](#23-automated-optimization-of-agentic-workflows)
    - [2.4 Self-Improving & Self-Modifying Harnesses](#24-self-improving--self-modifying-harnesses)
    - [2.5 Evolutionary & Program Search](#25-evolutionary--program-search)
    - [2.6 Joint Optimization with Model Weights](#26-joint-optimization-with-model-weights)
  - [3. Auto-Research: A Self-Improvement Loop in Action](#3-auto-research-a-self-improvement-loop-in-action)
    - [3.1 Closed-Loop & Self-Improving Research Agents](#31-closed-loop--self-improving-research-agents)
    - [3.2 Self-Improving Idea & Data Generation](#32-self-improving-idea--data-generation)
    - [3.3 Reality Checks: Are Agents Scientists Yet?](#33-reality-checks-are-agents-scientists-yet)
  - [4. Evaluators That Close the Loop](#4-evaluators-that-close-the-loop)
    - [4.1 AI Research & ML Engineering Benchmarks](#41-ai-research--ml-engineering-benchmarks)
    - [4.2 Coding & Terminal Agent Benchmarks](#42-coding--terminal-agent-benchmarks)
    - [4.3 Verification & Verifiers](#43-verification--verifiers)
  - [5. Challenges: When Self-Improvement Goes Wrong](#5-challenges-when-self-improvement-goes-wrong)
  - [6. Related Surveys](#6-related-surveys)
  - [7. Adjacent Areas (Out of Focus)](#7-adjacent-areas-out-of-focus)
- [Future Directions](#future-directions)
- [Contributing](#contributing)
- [Citation](#citation)

---

## Overview

The dominant narrative attributes agent capability to the underlying model. This reading list follows a complementary thesis: **the layer between the raw model and the real-world context — the harness — is as decisive as the model's raw intelligence**, and, crucially, it is a layer that can be *made to improve itself*.

Recursive Self-Improvement (RSI) dates back to I. J. Good (1965) and was named by Yudkowsky (2008): an AI uses its current intelligence to improve the machinery that produces its intelligence. In modern AI this loop rarely starts with a model rewriting its own weights. A more practical near-term path runs *through the harness*: the agent improves its own scaffolding, workflow, context management, and tools, which in turn enables a better successor.

The organizing question of this list is therefore narrow and specific: **when does a harness improve itself, and how?** Everything here is selected because the harness — not just the model, and not merely a human-authored design — is the thing being optimized, searched, or evolved.

---

## The Optimization Ladder

The progression of the object being *self-improved* inside a harness system, from most manual to most general. **This ladder is the backbone of the list.**

| Level | Self-Improved Object | Representative Work | Section |
| ----- | -------------------- | ------------------- | ------- |
| L0 | **Instruction prompts** | APE, OPRO, Promptbreeder, GEPA | [2.2](#22-prompt-optimization) |
| L1 | **Context / memory** | ACE, Dynamic Cheatsheet, ReasoningBank | [2.1](#21-self-evolving-context--memory) |
| L2 | **Workflow / graph** | ADAS, AFlow, GPTSwarm, AgentSquare | [2.3](#23-automated-optimization-of-agentic-workflows) |
| L3 | **Harness / agent code** | STOP, Gödel Agent, DGM, SICA, Self-Harness | [2.4](#24-self-improving--self-modifying-harnesses) |
| L4 | **Optimizer / meta-harness code** | Meta-Harness, MCE, Meta Agent Search | [2.4](#24-self-improving--self-modifying-harnesses) |
| L5 | **Harness + model weights (jointly)** | SIA, SEAL | [2.6](#26-joint-optimization-with-model-weights) |
| — | *(boundary)* model weights **only** | *self-play, RLVR, synthetic data* | [§7](#7-adjacent-areas-out-of-focus) — out of focus |
| — | *(boundary)* the harness **design** itself | *ReAct, SWE-agent, AutoGen, MCP* | [§7](#7-adjacent-areas-out-of-focus) — out of focus |

*As the model becomes more capable, the field moves down this table: toward more complex self-improved targets and more general mechanisms. Below L5 lies pure model-weight self-improvement; outside the ladder entirely lies human-authored harness design. Both are treated as adjacent, not core.*

---

## Historical Lineage

| Year | Milestone | Significance |
| ---- | --------- | ------------ |
| 1965 | I. J. Good — "ultraintelligent machine" | First articulation of an intelligence explosion via self-design |
| 2008 | Yudkowsky — "Recursive Self-Improvement" | Names the RSI feedback loop |
| 2023 | Reflexion, Voyager, DSPy, Promptbreeder, STOP, FunSearch | Verbal self-improvement loops, self-growing skills, compiled pipelines, self-referential improvers |
| 2024 | ADAS, AFlow, GPTSwarm, Agent Symbolic Learning, TextGrad | Automated agent/workflow optimization; textual "backprop" over harness parts |
| 2025 | AlphaEvolve, ShinkaEvolve, DGM, SICA, ACE, GEPA, SEAL | Evolutionary coding agents; self-modifying agents; self-evolving context; self-adapting weights |
| 2026 | Meta-Harness, MCE, Self-Harness, AutoHarness, Hyperagents, SIA | Harnesses that improve harnesses; joint harness+weight loops |

---

## Paper List

### 1. Foundations & Position Pieces

- **Harness Engineering for Self-Improvement** — Lilian Weng. *Lil'Log* 2026. [[blog]](https://lilianweng.github.io/posts/2026-07-04-harness/) — The essay this list is built around; frames the harness as the near-term substrate for RSI.
- **Speculations Concerning the First Ultraintelligent Machine** — I. J. Good. *Advances in Computers* 1965. — Origin of the intelligence-explosion idea via self-design.
- **Recursive Self-Improvement** — E. Yudkowsky. *LessWrong* 2008. [[post]](https://www.lesswrong.com/posts/JBadX7rwdcRFzGuju/recursive-self-improvement) — Names and analyzes the RSI feedback loop.
- **A Survey of Self-Evolving Agents: What, When, How, and Where to Evolve** — Gao et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2507.21046) — Taxonomy of self-evolving agents across models, memory, tools, and architecture.
- **A Comprehensive Survey of Self-Evolving AI Agents** — Fang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2508.07407) — Bridges foundation models and lifelong agentic systems; proposes "Three Laws of Self-Evolving AI Agents".

### 2. Harness Optimization (the core)

*This is the heart of the list. Each subsection is a rung of the [optimization ladder](#the-optimization-ladder): a different part of the harness that the system learns to improve on its own.*

#### 2.1 Self-Evolving Context & Memory

*L1 of the ladder: the agent curates and grows its own context/memory from experience, improving without weight updates.*

- **Reflexion** — "Language Agents with Verbal Reinforcement Learning". Shinn et al. *NeurIPS* 2023. [[paper]](https://arxiv.org/abs/2303.11366) — Converts feedback into verbal self-reflections stored in episodic memory across trials — the archetypal in-harness self-improvement loop.
- **ExpeL** — "LLM Agents Are Experiential Learners". Zhao et al. *AAAI* 2024. [[paper]](https://arxiv.org/abs/2308.10144) — Gathers experiences, extracts NL insights into a growing context store, recalls at inference without fine-tuning.
- **Dynamic Cheatsheet** — "Test-Time Learning with Adaptive Memory". Suzgun et al. *EACL* 2026.† [[paper]](https://arxiv.org/abs/2504.07952) — Persistent self-curated memory of strategies/snippets at inference; a direct precursor to ACE.
- **ACE** — "Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models". Zhang et al. *ICLR* 2026. [[paper]](https://arxiv.org/abs/2510.04618) — Treats context as an evolving playbook via Generator/Reflector/Curator with incremental delta updates, avoiding context collapse.
- **MCE** — "Meta Context Engineering via Agentic Skill Evolution". Ye et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2601.21557) — Bi-level framework co-evolving context-management *skills* (meta) and *context artifacts* (base, as files/code) — separates the mechanism from the content.
- **ReasoningBank** — "Scaling Agent Self-Evolving with Reasoning Memory". Ouyang et al. *ICLR* 2026.† [[paper]](https://arxiv.org/abs/2509.25140) — Distills generalizable reasoning strategies from successes *and* failures; introduces memory-aware test-time scaling.
- **Agent Workflow Memory (AWM)** — Wang, Mao, Fried, Neubig. *ICML* 2025. [[paper]](https://arxiv.org/abs/2409.07429) — Induces reusable "workflows" as durable procedural memory that the agent grows and reuses with experience.
- **Memp** — "Exploring Agent Procedural Memory". Fang et al. *ACL Findings* 2026.† [[paper]](https://arxiv.org/abs/2508.06433) — Distills trajectories into script-like procedures with build/retrieve/update strategies the agent maintains over time.
- **MemAct** — "Memory as Action: Autonomous Context Curation for Long-Horizon Agentic Tasks". Zhang et al. *ACL Findings* 2026.† [[paper]](https://arxiv.org/abs/2510.12635) — Reframes working-memory management as learnable policy actions trained end-to-end.

#### 2.2 Prompt Optimization

*L0 of the ladder: the harness's instruction layer as the object that gets optimized.*

- **APE** — "Large Language Models Are Human-Level Prompt Engineers". Zhou et al. *ICLR* 2023. [[paper]](https://arxiv.org/abs/2211.01910) — Treats the instruction as a program, proposing/scoring candidates via search.
- **OPRO** — "Large Language Models as Optimizers". Yang et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2309.03409) — "Optimization by PROmpting": generate new solutions from a meta-prompt of prior (solution, score) pairs.
- **EvoPrompt** — "Connecting LLMs with Evolutionary Algorithms Yields Powerful Prompt Optimizers". Guo et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2309.08532) — Runs GA/DE over a population of discrete prompts with LLM mutation/crossover.
- **Promptbreeder** — "Self-Referential Self-Improvement via Prompt Evolution". Fernando et al. *arXiv* 2023.† [[paper]](https://arxiv.org/abs/2309.16797) — Evolves both task-prompts *and* the mutation-prompts that modify them — self-referential prompt improvement.
- **ProTeGi** — "Automatic Prompt Optimization with 'Gradient Descent' and Beam Search". Pryzant et al. *EMNLP* 2023. [[paper]](https://arxiv.org/abs/2305.03495) — Coined "textual gradients": LLM critiques as NL gradients editing prompts.
- **DSPy** — "Compiling Declarative Language Model Calls into Self-Improving Pipelines". Khattab et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.03714) — Programming model treating LM pipelines as optimizable text-transformation graphs.
- **MIPROv2** — "Optimizing Instructions and Demonstrations for Multi-Stage Language Model Programs". Opsahl-Ong et al. *EMNLP* 2024. [[paper]](https://arxiv.org/abs/2406.11695) — Jointly bootstraps few-shot examples and proposes instructions via Bayesian optimization.
- **TextGrad** — "Automatic 'Differentiation' via Text". Yuksekgonul et al. *Nature* 2025. [[paper]](https://arxiv.org/abs/2406.07496) — Backpropagates textual feedback through compound AI systems, PyTorch-style.
- **GEPA** — "Reflective Prompt Evolution Can Outperform Reinforcement Learning". Agrawal et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2507.19457) — Genetic-Pareto reflective optimizer reading full traces; beats RL with up to 35× fewer rollouts.

#### 2.3 Automated Optimization of Agentic Workflows

*L2 of the ladder: the agentic workflow/graph is searched and optimized automatically, rather than hand-designed.*

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
- **Alita** — "Generalist Agent Enabling Scalable Agentic Reasoning with Minimal Predefinition and Maximal Self-Evolution". Qiu et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.20286) — Self-evolves by autonomously generating and reusing its own MCP tools on the fly.

#### 2.4 Self-Improving & Self-Modifying Harnesses

*L3–L4 of the ladder: the harness/agent code — and the code that optimizes it — as the object of self-modification. **The most direct realization of the theme.***

- **STOP** — "Self-Taught Optimizer: Recursively Self-Improving Code Generation". Zelikman et al. *COLM* 2024. [[paper]](https://arxiv.org/abs/2310.02304) — A seed improver recursively improves its own scaffolding code (weights fixed); the improver, not the solution, is the target.
- **Gödel Agent** — "A Self-Referential Agent Framework for Recursive Self-Improvement". Yin et al. *ACL* 2025. [[paper]](https://arxiv.org/abs/2410.04444) — Uses monkey-patching to dynamically rewrite its own logic at runtime.
- **Darwin Gödel Machine (DGM)** — "Open-Ended Evolution of Self-Improving Agents". Zhang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.22954) — A coding agent rewrites its own codebase over an open-ended archive; SWE-bench 20%→50%.
- **SICA** — "A Self-Improving Coding Agent". Robeyns et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2504.15228) — Removes the meta/target distinction; the agent edits its own codebase for cost/speed/accuracy.
- **Meta-Harness** — "End-to-End Optimization of Model Harnesses". Lee et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2603.28052) — An agentic proposer searches over harness *code* via the file system; returns a Pareto frontier of harnesses. "A harness for optimizing harnesses."
- **Self-Harness** — "Harnesses That Improve Themselves". Zhang et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2606.09498) — A propose-evaluate-accept loop: weakness mining → bounded harness proposal → regression validation on held-in/held-out splits.
- **AutoHarness** — "Improving LLM Agents by Automatically Synthesizing a Code Harness". Lou et al. *arXiv* 2026.† — Uses iterative code refinement with environment feedback to auto-synthesize a code harness.
- **Hyperagents** — Zhang et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2603.19461) — A meta-agent controls how to modify task agents to create new ones.

#### 2.5 Evolutionary & Program Search

*The engine behind self-improving coding agents: an LLM proposes edits, an evaluator scores them, and the agent's own code/algorithms evolve over a population. These are the mechanisms that DGM, Meta-Harness, and Self-Harness build on.*

- **AlphaEvolve** — "A Coding Agent for Scientific and Algorithmic Discovery". Novikov et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2506.13131) — Evolutionary coding agent with LLM ensemble + evaluators; marked `EVOLVE-BLOCK` regions; discovered a 48-mult 4×4 matrix algorithm.
- **FunSearch** — "Mathematical Discoveries from Program Search with Large Language Models". Romera-Paredes et al. *Nature* 2023. [[paper]](https://www.nature.com/articles/s41586-023-06924-6) — LLM + evaluator in an evolutionary loop; the template that self-improving coding agents descend from.
- **ShinkaEvolve** — "Towards Open-Ended and Sample-Efficient Program Evolution". Lange et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2509.19349) — Parent sampling, novelty rejection sampling, and bandit LLM selection for sample-efficient evolution.
- **ThetaEvolve** — "Test-time Learning on Open Problems". Wang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2511.23473) — Combines evolutionary search with RL and in-context learning.
- **ELM** — "Evolution through Large Models". Lehman et al. *arXiv* 2022.† [[paper]](https://arxiv.org/abs/2206.08896) — LLM diff model as a mutation operator inside MAP-Elites; the earliest LLM-as-mutation program-evolution work.

#### 2.6 Joint Optimization with Model Weights

*L5 of the ladder: the boundary where harness improvement and weight updates happen in the same loop. This is the deepest in-focus rung; pure weight-only self-improvement is [out of focus](#7-adjacent-areas-out-of-focus).*

- **SIA** — "Self Improving AI with Harness & Weight Updates". Hebbar et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2605.27276) — A Feedback-Agent decides, per iteration, whether to update the harness or the model weights.
- **SEAL** — "Self-Adapting Language Models". Zweiger et al. *NeurIPS* 2025. [[paper]](https://arxiv.org/abs/2506.10943) — The model generates its own "self-edits" (finetuning data + directives), applied via SFT with an RL loop — the model editing its own update process.
- **Voyager** — "An Open-Ended Embodied Agent with Large Language Models". Wang et al. *TMLR* 2024. [[paper]](https://arxiv.org/abs/2305.16291) — Lifelong learning via automatic curriculum + a self-growing executable skill library (harness-side, no weight updates); the canonical skill-accumulation loop.
- **SkillWeaver** — "Web Agents can Self-Improve by Discovering and Honing Skills". Zheng et al. *COLM* 2025. [[paper]](https://arxiv.org/abs/2504.07079) — Agents synthesize reusable, debugged API skills into their harness; +31.8% on WebArena.

### 3. Auto-Research: A Self-Improvement Loop in Action

*Auto-research is included only where the system forms a **closed self-improvement loop** — it reviews, critiques, and builds on its own outputs — rather than a one-shot designed pipeline. It is where evaluation, memory, and self-correction must all come together.*

#### 3.1 Closed-Loop & Self-Improving Research Agents

- **The AI Scientist-v2** — "Workshop-Level Automated Scientific Discovery via Agentic Tree Search". Yamada et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2504.08066) — Template-free agentic tree search that iteratively refines its own experiments; produced the first fully-AI paper to pass workshop peer review.
- **CycleResearcher** — "Improving Automated Research via Automated Review". Weng et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2411.00816) — Policy model + reward model trained with iterative RL so the researcher improves from its own automated reviews.
- **AgentRxiv** — "Towards Collaborative Autonomous Research". Schmidgall & Moor. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2503.18102) — A shared preprint server letting agent labs build on each other's reports — collective self-improvement.
- **Dolphin** — "Moving Towards Closed-loop Auto-research through Thinking, Practice, and Feedback". Yuan et al. *ACL* 2025. [[paper]](https://arxiv.org/abs/2501.03916) — Closed-loop idea→experiment→feedback with traceback-guided debugging.
- **NovelSeek / InternAgent** — "When Agent Becomes the Scientist". InternAgent Team. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.16938) — Unified closed-loop multi-agent framework across 12 scientific tasks.
- **AI co-scientist** — "Towards an AI co-scientist". Gottweis et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.18864) — Gemini multi-agent system that tournament-evolves biomedical hypotheses through a generate-debate-rank loop.
- **AIDE** — "AI-Driven Exploration in the Space of Code". Jiang et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.13138) — Casts ML engineering as iterative code optimization via agentic tree search over its own solutions.

#### 3.2 Self-Improving Idea & Data Generation

- **Autodata** — "An Agentic Data Scientist to Create High Quality Synthetic Data". Kulikov et al. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2606.25996) — Challenger/weak-solver/strong-solver/judge roles produce "just right" difficulty data, with the challenger prompt updated iteratively from feedback.
- **ResearchAgent** — "Iterative Research Idea Generation over Scientific Literature". Baek et al. *NAACL* 2025. [[paper]](https://arxiv.org/abs/2404.07738) — Iteratively generates and refines its own problems, methods, and designs via a literature graph and reviewing agents.

#### 3.3 Reality Checks: Are Agents Scientists Yet?

- **Why LLMs Aren't Scientists Yet** — "Lessons from Four Autonomous Research Attempts". Trehan & Chopra. *arXiv* 2026.† [[paper]](https://arxiv.org/abs/2601.03315) — Documents six recurring failure modes of self-directed research loops (training-data defaults, implementation drift, memory degradation, over-optimism, insufficient domain intelligence, weak scientific taste).
- **Early Science Acceleration Experiments with GPT-5** — Bubeck et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2511.16072) — Notes "p-hacking and eureka-ing": self-improvement loops declaring victory on noise.
- **Evaluating Sakana's AI Scientist** — "Wishful Thinking or Emerging Reality?". *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2502.14297) — Independent eval: high experiment failure rate, hallucinated results.

### 4. Evaluators That Close the Loop

*A self-improvement loop is only as good as its evaluator. These benchmarks and verifiers are the signal a self-improving harness optimizes against — and their weaknesses are its blind spots.*

#### 4.1 AI Research & ML Engineering Benchmarks

- **PaperBench** — "Evaluating AI's Ability to Replicate AI Research". Starace et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2504.01848) — Replicate 20 ICML 2024 papers from scratch; 8,316 rubrics.
- **MLE-bench** — "Evaluating Machine Learning Agents on Machine Learning Engineering". Chan et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.07095) — 75 Kaggle competitions with human-leaderboard baselines.
- **RE-Bench** — "Evaluating Frontier AI R&D Capabilities Against Human Experts". Wijk et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2411.15114) — 7 open-ended ML R&D environments vs 61 human experts.
- **ScienceAgentBench** — "Toward Rigorous Assessment of Language Agents for Data-Driven Scientific Discovery". Chen et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.05080) — 102 tasks from 44 peer-reviewed papers.
- **CORE-Bench** — "Fostering the Credibility of Published Research...". Siegel et al. *TMLR* 2024. [[paper]](https://arxiv.org/abs/2409.11363) — 270 computational-reproducibility tasks from 90 papers.
- **EXP-Bench** — "Can AI Conduct AI Research Experiments?". Kon et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2505.24785) — 461 end-to-end tasks; best agents ~0.5% full success.
- **KernelBench** — "Can LLMs Write Efficient GPU Kernels?". Ouyang et al. *ICML* 2025. [[paper]](https://arxiv.org/abs/2502.10517) — 250 PyTorch workloads scored by fast_p — a fast, automatable verifier well-suited to evolutionary harnesses.

#### 4.2 Coding & Terminal Agent Benchmarks

- **SWE-bench** — "Can Language Models Resolve Real-World GitHub Issues?". Jimenez et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.06770) — 2,294 real issue→PR tasks; the standard target for coding-harness self-improvement (DGM, SICA).
- **Terminal-Bench** — "Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces". Merrill et al. *arXiv* 2026.† — Human-verified containerized terminal tasks; the eval used by Meta-Harness / Self-Harness.
- **HAL** — "Holistic Agent Leaderboard: The Missing Infrastructure for AI Agent Evaluation". Kapoor et al. *ICLR* 2026. [[paper]](https://arxiv.org/abs/2510.11977) — Standardized, cost-aware, third-party leaderboard across 9 benchmarks.

#### 4.3 Verification & Verifiers

- **Let's Verify Step by Step** — Lightman et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2305.20050) — Process supervision beats outcome supervision; releases PRM800K.
- **Generative Verifiers (GenRM)** — "Reward Modeling as Next-Token Prediction". Zhang et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2408.15240) — CoT verification via next-token prediction.
- **LLMs Cannot Self-Correct Reasoning Yet** — Huang et al. *ICLR* 2024. [[paper]](https://arxiv.org/abs/2310.01798) — Intrinsic self-correction often degrades without an external signal — motivates verifiers *outside* the self-improvement loop.

### 5. Challenges: When Self-Improvement Goes Wrong

*The failure modes that gate real RSI. The evaluator and permission control should sit **outside** the loop that evolves the harness.*

- **Misevolution** — "Your Agent May Misevolve: Emergent Risks in Self-evolving LLM Agents". Shao et al. *ICLR* 2026. [[paper]](https://arxiv.org/abs/2509.26354) — First systematic study of misevolution across model/memory/tool/workflow paths — directly about self-evolving harness risk.
- **Defining and Characterizing Reward Hacking** — Skalse et al. *NeurIPS* 2022. [[paper]](https://arxiv.org/abs/2209.13085) — First formal definition; "unhackability" is a strong condition.
- **Scaling Laws for Reward Model Overoptimization** — Gao et al. *ICML* 2023. [[paper]](https://arxiv.org/abs/2210.10760) — Functional forms for gold-reward degradation vs KL — the core risk of a loop optimizing against a proxy.
- **Specification Gaming: the Flip Side of AI Ingenuity** — Krakovna et al. *DeepMind blog* 2020. [[blog]](https://deepmind.google/discover/blog/specification-gaming-the-flip-side-of-ai-ingenuity/) — Catalog of agents exploiting objective loopholes.
- **Sycophancy to Subterfuge** — "Investigating Reward Tampering in Language Models". Denison et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2406.10162) — Generalization from sycophancy to reward-function tampering — a self-improvement loop editing its own reward.
- **Monitoring Reasoning Models for Misbehavior** — Baker et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2503.11926) — CoT monitoring detects hacking, but training against the monitor yields obfuscation.
- **AI Agents That Matter** — Kapoor et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2407.01502) — Agent benchmarks over-focus on accuracy vs cost with weak holdouts — a validity crisis for self-improvement signals.
- **Many SWE-bench-Passing PRs Would Not Be Merged into Main** — METR. *Report* 2026. — Benchmark-passing PRs have far lower human merge rates — an evaluation-validity warning for self-improving coding harnesses.
- **Leakage and the Reproducibility Crisis in ML-based Science** — Kapoor & Narayanan. *Patterns* 2023. [[paper]](https://arxiv.org/abs/2207.07048) — Data leakage across 17 fields / 329 papers; relevant to trusting auto-research results.
- **AgentHarm** — "A Benchmark for Measuring Harmfulness of LLM Agents". Andriushchenko et al. *ICLR* 2025. [[paper]](https://arxiv.org/abs/2410.09024) — Malicious agent tasks testing refusal + retained capability.

### 6. Related Surveys

- **A Survey on Self-Evolution of Large Language Models** — Tao et al. *arXiv* 2024.† [[paper]](https://arxiv.org/abs/2404.14387) — Self-evolution as cycles of experience acquisition → refinement → updating → evaluation.
- **A Survey of Context Engineering for Large Language Models** — Mei et al. *arXiv* 2025.† [[paper]](https://arxiv.org/abs/2507.13334) — 1400+ paper survey of retrieval/processing/management components that self-evolving context builds on.
- **Automated Design of Agentic Systems: A Survey** — Madžar & Mekterović. *Preprints.org* 2026.† — Surveys 33 ADAS methods along target/search/representation/feedback axes.
- **Agent Harness for Large Language Model Agents: A Survey** — Meng et al. *Preprints.org* 2026.† [[repo]](https://github.com/Gloriaameng/Awesome-Agent-Harness) — Formalizes the harness as H=(E,T,C,S,L,V); a complementary systems-oriented view of the substrate self-improvement acts on.

### 7. Adjacent Areas (Out of Focus)

*These areas are close to the theme but deliberately **not** its subject. They are listed only to mark the boundary; each group is intentionally short and non-exhaustive.*

**A. Harness design patterns — the editable substrate, not self-improvement.** These works define *how to build* a good harness. They are the surface the §2 methods act on, but they are hand-authored design, not systems that improve themselves.

- *Execution loop:* ReAct ([2210.03629](https://arxiv.org/abs/2210.03629)), Self-Refine ([2303.17651](https://arxiv.org/abs/2303.17651)), ReWOO ([2305.18323](https://arxiv.org/abs/2305.18323)).
- *Coding-agent harnesses:* SWE-agent ([2405.15793](https://arxiv.org/abs/2405.15793)), OpenHands ([2407.16741](https://arxiv.org/abs/2407.16741)), CodeAct ([2402.01030](https://arxiv.org/abs/2402.01030)), Agentless ([2407.01489](https://arxiv.org/abs/2407.01489)).
- *Multi-agent frameworks:* AutoGen ([2308.08155](https://arxiv.org/abs/2308.08155)), MetaGPT ([2308.00352](https://arxiv.org/abs/2308.00352)).
- *Memory/state & runtimes:* MemGPT ([2310.08560](https://arxiv.org/abs/2310.08560)), AIOS ([2403.16971](https://arxiv.org/abs/2403.16971)), MCP ([2503.23278](https://arxiv.org/abs/2503.23278)).
- *Practitioner reports:* [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) (Anthropic), [Effective Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) (Anthropic).

**B. Purely model-weight self-improvement — improves the model, not the harness.**

- *Self-play & self-training:* SPIN ([2401.01335](https://arxiv.org/abs/2401.01335)), Self-Rewarding LMs ([2401.10020](https://arxiv.org/abs/2401.10020)).
- *Reinforced self-improvement from zero data:* Absolute Zero / AZR ([2505.03335](https://arxiv.org/abs/2505.03335)), R-Zero ([2508.05004](https://arxiv.org/abs/2508.05004)), TTRL ([2504.16084](https://arxiv.org/abs/2504.16084)).
- *RL for reasoning (RLVR):* DeepSeek-R1 ([2501.12948](https://arxiv.org/abs/2501.12948)), DeepSeekMath / GRPO ([2402.03300](https://arxiv.org/abs/2402.03300)).
- *Bootstrapped synthetic data:* STaR ([2203.14465](https://arxiv.org/abs/2203.14465)), Self-Instruct ([2212.10560](https://arxiv.org/abs/2212.10560)), ReST^EM ([2312.06585](https://arxiv.org/abs/2312.06585)).

> When these are co-optimized with the harness in one loop, the relevant work lives in [§2.6](#26-joint-optimization-with-model-weights) instead.

---

## Future Directions

Open problems where current self-improving harnesses provide partial solutions but no general, production-grade infrastructure:

1. **Weak and fuzzy evaluators.** Self-improvement loops work best with measurable, objective metrics. Research taste, novelty, and long-term value are hard to verify.
2. **Context and memory lifecycle.** Memory grows with agent autonomy; context engineering should become a core part of intelligence rather than a software-layer afterthought.
3. **Negative results.** Literature is biased toward successes; a self-improving harness should make failed attempts easy to preserve and learn from.
4. **Diversity collapse.** Evolutionary and RL loops exploit known high-reward patterns; open-ended search needs mechanisms against population collapse.
5. **Reward hacking.** A self-improvement loop optimizes whatever signal it is given; evaluators and permission control should sit *outside* the loop that evolves the harness.
6. **Broken abstraction boundaries.** When a program can edit its own OS/harness, the editable surface, permission control, and security layers must live *outside* the self-improvement loop.
7. **Long-term success.** Sandbox RLVR rarely captures maintainability, ownership boundaries, migration cost, or future debugging burden.
8. **The role of humans.** Humans should move *up* the stack — providing oversight at the right abstraction level and the right time — not be removed from the loop.

---

## Contributing

**Pull requests are very welcome!** 🎉 This list is meant to be a living, community-maintained resource. If you know of a paper we missed, spot an outdated or broken link, or have a better one-line summary, please [open a pull request](https://github.com/leezythu/Awesome-Harness-Self-Improvement/pulls) or [start an issue](https://github.com/leezythu/Awesome-Harness-Self-Improvement/issues). Every contribution — big or small — is appreciated.

Please respect the [scope](#scope-please-read): the harness (context, prompts, workflow, tools, code) must be the object that **improves itself**, or a direct evaluator of such a loop. Harness *design* work and purely model-weight methods belong in [§7](#7-adjacent-areas-out-of-focus) at most.

Please open a PR that:

- Adds the paper under the most relevant subsection, keeping the format: `**Name** — "Title". Authors. Venue Year. [[paper]](link) — one-line description tying it to harness self-improvement.`
- Uses `†` for preprints or very recent postings.
- Prefers the canonical venue where a paper is published; otherwise the arXiv abstract page.

> **Accuracy note:** entries marked `†` include recent (2025–2026) preprints whose arXiv IDs, authorship, or venues may still change. Please verify links before citing in formal work.

## Citation

If you find this list useful, please consider citing this repository:

```bibtex
@misc{awesome_self_improving_harness,
  title  = {Awesome Harness Engineering for Self-Improvement},
  author = {leezythu},
  year   = {2026},
  howpublished = {\url{https://github.com/leezythu/Awesome-Harness-Self-Improvement}}
}
```
