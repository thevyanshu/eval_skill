# AI Agent Hackathon Evaluation Skill

A reusable evaluation skill designed for coding agents to rigorously judge hackathon submissions of AI agents and their complete harnesses against a defined problem statement.

---

## Overview

This repository provides the **AI Agent Hackathon Evaluation Skill** (`ai-agent-hackathon-evaluator`). The skill instructs a coding agent to objectively inspect a project codebase, analyze all components of the AI agent harness, assign evidence-based scores across five evaluation parameters, generate a standardized Markdown evaluation report (`hack_evaluation.md`), commit the report to Git, and return a strict chat response.

### Skill Locations
The skill is packaged according to Antigravity's workspace customization standards and is available in:
- `.agents/skills/ai-agent-hackathon-evaluator/SKILL.md` (Native Antigravity Customization Root)
- `skills/ai-agent-hackathon-evaluator/SKILL.md` (Standard Skills Directory)
- `.agents/skills.json` (Customization manifest)

---

## Directory Structure

```text
eval_skill/
├── .agents/
│   ├── skills.json                               # Skills configuration manifest
│   └── skills/
│       └── ai-agent-hackathon-evaluator/
│           ├── SKILL.md                          # Master executable evaluation skill
│           ├── references/
│           │   ├── scoring-rubric.md             # Detailed scoring bands and criteria
│           │   ├── security-checklist.md         # 5-dimension security audit checklist
│           │   └── harness-inspection-guide.md   # Step-by-step harness discovery guide
│           └── resources/
│               └── report-template.md            # Structural template for hack_evaluation.md
├── skills/
│   └── ai-agent-hackathon-evaluator/             # Mirrored skill package
│       ├── SKILL.md
│       ├── references/
│       │   ├── scoring-rubric.md
│       │   ├── security-checklist.md
│       │   └── harness-inspection-guide.md
│       └── resources/
│           └── report-template.md
└── README.md                                     # Repository documentation
```

---

## Evaluation Parameters & Scoring System

The evaluation is scored out of **405 total marks** (primary scores are never normalized to 100):

| Parameter | Maximum Marks | Description |
|---|---|---|
| **1. Problem Statement Alignment** | 100 | Functional completeness, core use cases, edge case handling, output schema compliance. |
| **2. Code Quality** | 100 | Clean architecture, modularity, type safety, error handling, 12-factor config, tests, observability. |
| **3. Innovation** | 100 | Non-trivial agent architecture, orchestration (LangGraph, custom DAG), tools, memory, RAG innovations. |
| **4. Security** | 100 | Prompt injection defense, tool permissions/sandboxing, secret hygiene, RAG security, DoS/loop limits. |
| **5. Grounding and Evals** | 5 | **Grounding (2.5 pts):** Factual fidelity, citations, hallucination guards. <br>**Evals (2.5 pts):** Deterministic tests, trajectory evals, benchmark datasets. |
| **Total Score** | **405** | Total sum of all parameters. |

---

## Core Operational Protocols

### 1. Problem Statement Handling
The skill contains a prominent `[PROBLEM_STATEMENT]` placeholder.
- **Before evaluation:** The user or orchestrator replaces `[PROBLEM_STATEMENT]` with the hackathon problem statement.
- **Validation check:** If `[PROBLEM_STATEMENT]` is unreplaced, empty, or whitespace, the evaluator immediately halts and asks the user for the problem statement in chat. It does not inspect the code or create any files until provided.

### 2. Read-Only Safety Guardrails
The evaluator functions purely as a read-only auditor:
- May **NOT** modify project source code, refactor, or fix bugs.
- May **NOT** alter configuration or install dependencies.
- May **NOT** create evaluation scripts inside the target project.
- **ONLY `hack_evaluation.md` may be created or modified.**

### 3. Immediate Git Commit Protocol
Once `hack_evaluation.md` is generated:
1. Git state is checked (initialized if absent).
2. Existing user changes are preserved; **ONLY** `hack_evaluation.md` is staged.
3. Committed with exact message:
   ```bash
   git commit -m "chore: add AI agent hackathon evaluation"
   ```
4. No user confirmation prompt is requested before committing.

### 4. Strict Chat Output
Upon successful creation and commit of `hack_evaluation.md`, the evaluator agent outputs **EXACTLY**:
```text
File created
```
No scores, summaries, or explanatory text are displayed in the chat.
