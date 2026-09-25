# Universal Agent Configuration & Custom Commands

This repository defines the **AI Agent Hackathon Evaluator Skill**. All agents operating in this workspace (Antigravity, Claude Code, GitHub Copilot, OpenCode, Cursor, etc.) must follow these instructions.

---

## Slash Commands

### `/hack-eval [problem_statement]` (or `/eval-agent`)

**Intent:** Execute a complete, read-only hackathon evaluation of this project against the given problem statement using the `ai-agent-hackathon-evaluator` skill.

**Execution Protocol:**
1. **Extract Problem Statement:**
   - If argument `[problem_statement]` is provided in the slash command (e.g., `/hack-eval Build a multi-agent triage system...`), use it as the ground truth.
   - If no problem statement is provided in the command, check if [.agents/skills/ai-agent-hackathon-evaluator/SKILL.md](file:///f:/globalai/eval_skill/.agents/skills/ai-agent-hackathon-evaluator/SKILL.md) has had its `[PROBLEM_STATEMENT]` placeholder replaced.
   - If still missing, **HALT** and prompt the user: *"Please provide the hackathon problem statement to begin the AI agent evaluation."*
2. **Execute Evaluation Skill:**
   - Refer to [.agents/skills/ai-agent-hackathon-evaluator/SKILL.md](file:///f:/globalai/eval_skill/.agents/skills/ai-agent-hackathon-evaluator/SKILL.md).
   - Perform a complete, read-only inspection of the codebase harness.
   - Score the 5 parameters totaling **450 marks** (Alignment 100, Code Quality 100, Innovation 100, Security 100, Grounding & Evals 50).
   - Write the comprehensive judging report to `hack_evaluation.md`.
3. **Commit:**
   - Selectively stage `hack_evaluation.md` and commit with message: `chore: add AI agent hackathon evaluation`.
4. **Chat Response:**
   - Respond in chat with strictly: `File created`.
