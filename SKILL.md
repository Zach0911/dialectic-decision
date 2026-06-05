---
name: dialectic-decision
description: Use when the user explicitly asks for dialectic analysis, pro/con debate, adversarial review, 20-round contention, two-agent contest, 正反方博弈, 对抗分析, 深度推演, or similar decision pressure-testing.
---

# Dialectic Decision

## Overview

Use this skill to pressure-test an important question, plan, or decision through an internal pro/con contest, then return a concise recommendation. Do not use it implicitly for ordinary questions or simple execution tasks.

## Trigger Rule

Use only when the user explicitly requests this style of reasoning, such as:

- "调用正反方博弈 Skill"
- "用博弈方式分析"
- "正反方辩论一下"
- "让两个 agent 较量"
- "做 20 轮推演"
- "用反方质疑后给最佳方案"
- "Use dialectic-decision"

If the user does not explicitly request adversarial or dialectic analysis, do not invoke this skill.

## Step 1: Confirm Boundary

Before starting the contest, check whether the task has enough information:

- Background: why this matters
- Goal: what decision or output the user needs
- Constraints: scope, time, resources, rules, non-goals
- Success standard: what a good answer must achieve

If any of these are missing and material to the decision, ask concise clarification questions first. Do not invent key business facts or silently expand scope.

## Step 2: Set Roles

Use three roles:

| Role | Responsibility |
|---|---|
| Pro agent | Defend the strongest feasible solution, value, benefits, execution path, and reasons to proceed |
| Con agent | Attack assumptions, risks, hidden costs, failure modes, omissions, and reasons to pause or change direction |
| Arbiter | Synthesize the contest into the best practical recommendation |

When subagent tools are available and the user explicitly requested agents, dispatch separate pro and con subagents with the minimum necessary context. If real subagents are unavailable or not allowed, simulate the roles internally and state briefly that the contest was simulated.

For real subagents, keep prompts role-specific:

- Pro agent: defend the strongest version of the proposal, pressure-test likely objections internally, and return only the strongest case, survived objections, limits, and recommendation.
- Con agent: attack the strongest version of the proposal, pressure-test likely defenses internally, and return only the strongest risks, survived defenses, concessions, and recommendation.
- Arbiter: compare both outputs and produce the final user-facing answer.

## Step 3: Run the Contest

Run 20 internal rounds by default.

Rules:

- Keep every round anchored to the same user question.
- Maintain a private 20-item round ledger before answering. Each item should capture only the round focus and what changed: risk found, assumption revised, tradeoff clarified, or solution improved.
- Make each round add a new risk, clarification, tradeoff, correction, or stronger version of the solution.
- Make the pro side respond to the strongest con-side objections.
- Make the con side attack the improved proposal, not a stale version.
- Stop early only if the user requested a lighter version, the decision is clearly too small for 20 rounds, or missing information makes meaningful contention impossible. If the user explicitly asks for 20 rounds, do not stop early unless the task is blocked by missing information.
- Do not expose full hidden reasoning or a full transcript unless the user explicitly asks for a visible round-by-round summary.
- Do not claim that 20 rounds were completed unless the private ledger was actually completed or both role-specific subagents returned adequate pressure-tested outputs.

Use this internal checklist while arbitrating:

- strongest positive case
- strongest negative case
- assumptions that survived attack
- assumptions that failed
- practical constraints
- reversible vs. irreversible risks
- simplest useful next action

## Step 4: Output Concisely

Default to Chinese and Markdown unless the user asks otherwise.

Use this format:

```markdown
## 最佳方案

[直接给出推荐方案]

## 核心理由

- [理由 1]
- [理由 2]
- [理由 3]

## 主要风险

- [风险 1]
- [风险 2]

## 修正建议

- [建议 1]
- [建议 2]

## 最终判断

[一句话结论：建议做 / 不建议做 / 建议调整后做]
```

Keep the answer decision-oriented. The final recommendation must be executable, not only a balanced list of opinions.

## Completion Check

Before finalizing, verify:

- The user explicitly triggered dialectic or adversarial analysis.
- Missing background, goal, constraints, or success standard were either supplied or clarified.
- Pro and con roles were actually used, either through subagents or internal simulation.
- A 20-round private ledger was completed, or early stop was explicitly justified.
- The final answer does not expose the full hidden transcript by default.
- The final answer includes a concrete recommendation, core reasons, main risks, suggested fixes, and final judgment.

## Common Mistakes

- Do not use this skill for ordinary quick answers.
- Do not show all 20 rounds by default.
- Do not create fake certainty when the input lacks key facts.
- Do not let the con side make generic objections; require concrete risks.
- Do not let the pro side repeat benefits without answering objections.
- Do not end with "both options are fine"; choose a direction or name the exact missing decision input.
