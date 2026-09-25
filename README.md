# AI Agent Hackathon Evaluation Skill

A reusable evaluation skill designed for coding agents to rigorously judge hackathon submissions of AI agents and their complete harnesses against a defined problem statement.

---

## ⚡ Quick Setup: Add This Skill to Any Project

To evaluate any hackathon submission using your preferred coding agent, copy the skill configuration into the root of the project you want to judge:

### 1. Copy Skill Configuration to Target Repository

**PowerShell (Windows):**
```powershell
# From the target project root, copy the skill files:
Copy-Item -Recurse -Force "path\to\eval_skill\.agents", "path\to\eval_skill\.claude", "path\to\eval_skill\.github", "path\to\eval_skill\.opencode", "path\to\eval_skill\AGENTS.md" -Destination .
```

**Bash / Terminal (macOS / Linux):**
```bash
# From the target project root, copy the skill files:
cp -r path/to/eval_skill/{.agents,.claude,.github,.opencode,AGENTS.md} .
```

*(Minimal universal setup: simply copy the `.agents/` folder and `AGENTS.md` file.)*

---

### 2. Run the Evaluation in Your Coding Agent

Open the target project in your coding assistant and type:

| Coding Agent | Slash Command / Prompt | How to Run |
|---|---|---|
| **Antigravity (AGY)** | `/hack-eval <problem_statement>` | Native model rule detects command automatically |
| **Claude Code** | `/hack-eval <problem_statement>` | Native command loaded from `.claude/commands/hack-eval.md` |
| **GitHub Copilot** | `/hack-eval <problem_statement>` | Select `/hack-eval` in Copilot Chat prompt picker |
| **OpenCode** | `/hack-eval <problem_statement>` | Native command loaded from `.opencode/commands/hack-eval.md` |
| **Cursor / Windsurf / Others** | Type in chat: *"Execute /hack-eval using AGENTS.md"* | Guided by standard `AGENTS.md` |

> 💡 **Tip:** You can supply the hackathon problem statement directly in the command (e.g. `/hack-eval "Build a multi-agent triage system..."`). If omitted, the agent will pause and prompt you to enter it before running.

---

## Overview

This repository provides the **AI Agent Hackathon Evaluation Skill** (`hack-eval`). The skill instructs a coding agent to objectively inspect a project codebase, analyze all components of the AI agent harness, assign evidence-based scores across five evaluation parameters, generate a standardized Markdown evaluation report (`hack_evaluation.md`), commit the report to Git, and return a strict chat response.

### Skill Locations
The skill is packaged according to Antigravity's workspace customization standards and is available in:
- `.agents/skills/hack-eval/SKILL.md` (Native Antigravity Customization Root)
- `skills/hack-eval/SKILL.md` (Standard Skills Directory)
- `.agents/skills.json` (Customization manifest)

---

## Directory Structure

```text
eval_skill/
├── .agents/
│   ├── rules/
│   │   └── hack-eval-command.md                  # Antigravity /hack-eval rule
│   ├── skills.json                               # Skills configuration manifest
│   └── skills/
│       └── hack-eval/
│           ├── SKILL.md                          # Master executable evaluation skill
│           ├── references/
│           │   ├── scoring-rubric.md             # Detailed scoring bands and criteria
│           │   ├── security-checklist.md         # 5-dimension security audit checklist
│           │   └── harness-inspection-guide.md   # Step-by-step harness discovery guide
│           └── resources/
│               └── report-template.md            # Structural template for hack_evaluation.md
├── .claude/
│   └── commands/
│       └── hack-eval.md                          # Claude Code slash command
├── .github/
│   ├── copilot-instructions.md                   # GitHub Copilot agent instructions
│   └── prompts/
│       └── hack-eval.prompt.md                   # GitHub Copilot slash prompt
├── .opencode/
│   └── commands/
│       └── hack-eval.md                          # OpenCode slash command
├── AGENTS.md                                     # Universal agent configuration
├── skills/
│   └── hack-eval/                                # Mirrored skill package
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

The evaluation is scored out of **450 total marks** (primary scores are never normalized to 100):

| Parameter | Maximum Marks | Description |
|---|---|---|
| **1. Problem Statement Alignment** | 100 | Functional completeness, core use cases, edge case handling, output schema compliance. |
| **2. Code Quality** | 100 | Clean architecture, modularity, type safety, error handling, 12-factor config, tests, observability. |
| **3. Innovation** | 100 | Non-trivial agent architecture, orchestration (LangGraph, custom DAG), tools, memory, RAG innovations. |
| **4. Security** | 100 | Prompt injection defense, tool permissions/sandboxing, secret hygiene, RAG security, DoS/loop limits. |
| **5. Grounding and Evals** | 50 | **Grounding (25 pts):** Factual fidelity, citations, hallucination guards. <br>**Evals (25 pts):** Deterministic tests, trajectory evals, benchmark datasets. |
| **Total Score** | **450** | Total sum of all parameters. |


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

---

## Universal Slash Command (`/hack-eval`)

This skill includes native configuration for leading AI coding assistants:

| Assistant | Slash Command | Config Location | How to Invoke |
|---|---|---|---|
| **Antigravity (AGY)** | `/hack-eval` or `/skill` | `.agents/rules/hack-eval-command.md` | Type `/hack-eval <problem_statement>` |
| **Claude Code** | `/hack-eval` | `.claude/commands/hack-eval.md` | Type `/hack-eval <problem_statement>` |
| **GitHub Copilot** | `/hack-eval` | `.github/prompts/hack-eval.prompt.md` | Select or type `/hack-eval` in Copilot Chat |
| **OpenCode** | `/hack-eval` | `.opencode/commands/hack-eval.md` | Type `/hack-eval <problem_statement>` |
| **Any Agent** | Universal | `AGENTS.md` | Follows standard `AGENTS.md` instruction file |

### Usage Example:
```text
/hack-eval Build a multi-agent customer support triage system that classifies incoming tickets, runs RAG on product docs, and drafts replies.
```
If you omit the problem statement, the agent will pause and prompt you for it before running the evaluation.

