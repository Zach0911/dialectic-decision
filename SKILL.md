---
name: dialectic-decision
description: Use when the user explicitly asks for dialectic analysis, pro/con debate, adversarial review, 20-round contention, two-agent contest, 正反方博弈, 对抗分析, 深度推演, 投资判断, 买卖决策, 仓位风控, or similar decision review.
---

# Dialectic Decision

## Overview

Use this skill to pressure-test an important question, plan, or decision through an internal pro/con contest, then return a concise recommendation. Do not use it implicitly for ordinary questions or simple execution tasks.

For financial investment decisions, use the financial investment mode below before running the contest.

## Trigger Rule

Use only when the user explicitly requests this style of reasoning, such as:

- "调用正反方博弈 Skill"
- "用博弈方式分析"
- "正反方辩论一下"
- "让两个 agent 较量"
- "做 20 轮推演"
- "用反方质疑后给最佳方案"
- "Use dialectic-decision"
- "用正反方博弈判断这个投资"
- "帮我压测是否买入/卖出/加仓/减仓"

If the user does not explicitly request adversarial or dialectic analysis, do not invoke this skill.

## Step 1: Confirm Boundary

Before starting the contest, check whether the task has enough information:

- Background: why this matters
- Goal: what decision or output the user needs
- Constraints: scope, time, resources, rules, non-goals
- Success standard: what a good answer must achieve

If any of these are missing and material to the decision, ask concise clarification questions first. Do not invent key business facts or silently expand scope.

## Financial Investment Mode

Use this mode when the explicit dialectic request concerns stocks, bonds, funds, ETFs, crypto, options, futures, IPOs, portfolios, allocation, buy/sell/hold decisions, adding/reducing positions, valuation, timing, or other investment choices.

### Required Boundary Check

Before analysis, confirm or infer only from user-provided facts:

- Instrument: ticker/name, market, asset class, and whether leverage or derivatives are involved.
- Decision: buy, sell, hold, add, reduce, subscribe, avoid, hedge, or compare.
- Time horizon: intraday, short-term, medium-term, long-term, or unknown.
- User context: risk tolerance, existing position, portfolio concentration, capital at risk, liquidity needs, and max acceptable loss.
- Data basis: latest price/action, valuation, financials, news, filings, macro/sector context, and source time.

If key fields are missing, ask this concise template before giving an actionable investment view:

```markdown
请先补充 4 个信息：

1. 标的：代码 / 市场 / 资产类型
2. 周期：短线 / 中线 / 长线
3. 仓位：无仓 / 轻仓 / 重仓 / 占总资产比例
4. 风险：最大可接受亏损比例或金额
```

### Current Data Rule

If the investment decision depends on current price, valuation, filings, news, policy, rates, earnings, flows, or market regime, verify up-to-date sources before the contest. Cite or name the data time basis in the answer.

If current data cannot be checked, do not give an actionable conclusion. Use `信息不足` or provide a framework-only analysis, and state exactly which data is missing.

### Asset-Specific Checks

Use the relevant checks during the contest:

| Asset | Must Check |
|---|---|
| Stock | business quality, valuation, earnings, guidance, balance sheet, governance, liquidity, event risk |
| ETF/Fund | index/strategy exposure, holdings concentration, fees, tracking error, liquidity, premium/discount, drawdown |
| Bond | credit risk, duration, yield, rate sensitivity, issuer quality, liquidity, default/reinvestment risk |
| Option/Future | leverage, margin, expiry, implied volatility, liquidity, path dependency, liquidation risk |
| IPO | valuation, cornerstone/lockup, allocation, subscription heat, fundamentals, listing volatility |
| Crypto | custody, liquidity, volatility, protocol/exchange/regulatory risk, leverage, weekend gap risk |

### Investment Contest Focus

Make the pro side argue the strongest investment thesis:

- upside drivers
- valuation or expected return logic
- catalyst path
- risk/reward asymmetry
- position sizing rationale
- conditions that would strengthen the thesis

Make the con side argue the strongest risk thesis:

- downside scenarios
- valuation overextension
- liquidity, leverage, volatility, and drawdown risk
- financial, regulatory, governance, macro, sector, and event risks
- behavioral risks such as FOMO, anchoring, concentration, and averaging down
- conditions that would invalidate the thesis

### Compliance and Safety Boundaries

- Do not promise returns, certainty, insider knowledge, or guaranteed outcomes.
- Do not present the answer as personalized financial advice unless enough user-specific suitability information is available and the user explicitly requests that framing.
- Prefer "analysis conclusion" language over direct commands.
- Do not output `买入` or `卖出` as a standalone conclusion. Always attach premise, position size, invalidation condition, and review/exit condition.
- Use decision labels such as: `信息不足`, `不建议参与`, `谨慎观察`, `低仓位试错`, `分批参与`, `持有但设退出条件`, `减仓控制风险`.
- Always include risk, position sizing, invalidation conditions, and exit/reevaluation triggers when giving an actionable investment view.
- If the user asks for a trade and suitability data is missing, ask for the missing risk and portfolio context or provide only a general framework.

### Financial Output Format

For investment decisions, use this format instead of the generic format:

```markdown
## 投资判断

[信息不足 / 不建议参与 / 谨慎观察 / 低仓位试错 / 分批参与 / 持有但设退出条件 / 减仓控制风险]

## 关键依据

- [依据 1：数据、估值、基本面、催化或风险收益]
- [依据 2]
- [依据 3]

## 反方最强质疑

- [核心反方观点 1]
- [核心反方观点 2]

## 主要风险

- [风险 1]
- [风险 2]

## 仓位与风控

- 仓位：[建议区间或信息不足]
- 失效条件：[什么发生时原判断失效]
- 退出/复盘条件：[止损、止盈、事件、财报、价格区间或时间点]

## 最终结论

[一句话：在什么前提下，可以/不可以/谨慎参与]
```

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
- Structure the private ledger in four phases:
  - Rounds 1-5: define the strongest thesis, goal, assumptions, and success criteria.
  - Rounds 6-10: attack risks, hidden costs, failure modes, and missing information.
  - Rounds 11-15: revise the thesis with constraints, mitigations, sizing, and fallback paths.
  - Rounds 16-20: arbitrate, choose the best label, define conditions, and prepare the final answer.
- Make each round add a new risk, clarification, tradeoff, correction, or stronger version of the solution.
- Make the pro side respond to the strongest con-side objections.
- Make the con side attack the improved proposal, not a stale version.
- Stop early only if the user requested a lighter version, the decision is clearly too small for 20 rounds, or missing information makes meaningful contention impossible. If the user explicitly asks for 20 rounds, do not stop early unless the task is blocked by missing information.
- Do not expose full hidden reasoning or a full transcript unless the user explicitly asks for a visible round-by-round summary.
- Do not claim that 20 rounds were completed unless the private ledger was actually completed or both role-specific subagents returned adequate pressure-tested outputs.

If the user asks to see the process, provide only a visible summary:

- 4 phase summaries, not all hidden reasoning.
- strongest pro point
- strongest con point
- key assumption that changed
- final arbitration reason

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
- For investment decisions, current data needs, suitability context, risk limits, and compliance boundaries were handled.
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
- Do not give investment conclusions from stale or missing data without clearly labeling the limitation.
- Do not omit position sizing, invalidation conditions, and exit/reevaluation triggers for actionable investment views.
