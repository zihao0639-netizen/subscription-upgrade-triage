---
name: subscription-upgrade-triage
description: 'Run a decision triage on whether a subscription is worth upgrading, and deliver a binary verdict with a machine-checkable trigger line. Trigger on "do I really need to upgrade X", "is this Pro worth it", "should I pay for the higher tier", "which of these plans should I buy". Core seven steps: check official sources only, never secondhand; separate quota buckets instead of mistaking window A for window B; verify empirically whether a feature actually returns at reset time; plan by peak not average; ask about real daily usage before judging sufficiency; hunt for a zero-dollar equivalent first; annualize the cost; then give a binary answer plus a trigger line. Conclusions must be revisable on new evidence, with errors stated plainly.'
license: MIT
compatibility: 'Needs no special tools; works with web access for checking official pricing pages. Works in Claude Code, Claude.ai, Codex, Cursor and any SKILL.md-compatible agent.'
metadata:
  author: zihao0639-netizen
  version: '1.0.0'
  tags: 'subscription,upgrade,decision,triage,quota'
---

# Subscription Upgrade Decision Triage

## One-sentence principle

> **The user does not want analysis — they want a "verdict".**
> So the conclusion must be **binary** (upgrade / do not upgrade), paired with a **machine-checkable trigger line** —
> not "depends how much you use it", but "hit it once more and we upgrade, no more deliberation".

---

## The seven steps

### 1. Check **official** sources first, never secondhand
- Official help center / official pricing page > press > third-party blogs.
- ⚠️ Secondhand material is often mixed with **deprecated** info (old model names, old quotas, old prices).
  Rule: **when three or more sources disagree on the same thing, trust the official page, and state the discrepancy honestly.**
- ⚠️ Third-party "review sites" often lag one or two major versions — **when you see a specific number, first suspect it is stale**.

### 2. 🔴 Separate 「quota buckets」— the easiest step to get wrong
The same product usually has **several unrelated quotas**, each with its own window and unit. Classic traps:

| Symptom | Truth |
|---|---|
| The dashboard shows both a "5-hour window" and a "weekly window" | most likely **model/agent quota**, not the feature you think |
| A message shows "feature X + a reset time" | **does not mean** it is talking about feature X — the copy is usually generic |
| The same model has two names in two interfaces | usually **two tiers of the same model**, not two models |

### 3. ⭐ The most reliable qualitative method: **at reset time, try it — does the feature actually come back?**
- Reading copy goes wrong; **"does the feature return after the reset" is a hard fact.**
- Measured example: the system says "5-hour window, resets at 15:12", but **after 15:12 the target feature still does not work**
  ⇒ **rules out in one shot** the assumption that "that window governs this feature".
- Bonus: it also characterizes "behavior after exhaustion": **trickle-back per minute** vs **block refresh** are two entirely different mechanisms.

### 4. Record in **groups**, never as one average
- Typical groups: **weekend / weekday**, **peak / off-peak**, **self / family shared**.
- Rationale: **"average usage is a weak decision metric"** — a quiet average hides the peak that actually saturates.
- ⇒ **plan around the peak that actually matters.**

### 4.5 🔴 Clarify **real daily usage** before judging "enough" — do not assume for the user

**"Structurally insufficient" is a very strong claim; it must rest on "daily usage", not on "one observed peak".**

- **This skill has stepped on it**: seeing "last night's session + this morning's session stacked up and maxed the quota",
  it wrote "his usage **necessarily** spans all day ⇒ structurally cannot fit ⇒ must upgrade".
  The user then clarified: "**I never use it that long daily; it is mostly the evening stretch after I get home**"
  ⇒ **a fixed daily block ⇒ yesterday's usage already slid out of the rolling window ⇒ never collides. The conclusion was void on the spot.**
- ⇒ ✅ **correct order: first ask "when do you usually use it, roughly how long" → then judge "enough".**
  A single peak means "might collide", **not** "collides every day".
- ⇒ ✅ **peaks only set the "upper-bound risk"; the daily pattern decides "whether to spend money".**

### 4.6 ⭐ Hunt for a **zero-dollar equivalent** before talking about paying more

**Before adding money, ask one question: is there a "no-cost swap" for this bottleneck?**

- Example (real): the bottleneck is quota in a certain pool ⇒ **dropping one tier within the same pool often changes quota by 10x or more, and is free**.
  The cost is only "one tier weaker model".
- ⇒ ✅ **so the decision becomes a true binary**:
  **A. stay on the current tier + drop a tier / change usage ($0, cost: discounted capability or experience)**
  **B. upgrade (costs money, capability intact)**
  **Rule in one sentence**: **"can that work still get done one tier lower?"**
- 💡 **Recommended phrasing: try A for 2–3 days first (zero-cost trial), then use measured results to decide whether to spend the money.**
  ⇒ this turns "should I upgrade" from a guess into something measured, and fits cautious users best.
- ⚠️ but state clearly "when you may skip the trial and upgrade directly": **the user explicitly knows they need the top-tier capability**, or **cannot be bothered and is happy to pay for convenience**.

#### 🔴 4.6.1 "drop a tier to save quota" — the free path most likely to point the wrong way

**"Swap to a lower-tier model, quota grows 10x" sounds like the perfect free fix — but verify one premise first:**
**is the lower tier weaker than what the user 【already has】?**

- **This skill has stepped on it**: it offered "drop to a lightweight model, quota grows from 5–45 to 250–2,000 messages" as a zero-cost plan.
  A later check found: **on the same execution-class benchmark, that lightweight model scores lower than a local model the user already runs (by 7 points)**.
  ⇒ **the lightweight model is weaker than the free solution he already has** ⇒ dropping a tier = switching to a worse model, **while still consuming platform quota**.
  ⇒ the correct plan is 「**move that work back to what he already has (the local model), consuming zero platform quota**」, not "drop a tier within the platform".
- ⇒ ✅ **conclusion: before offering a "drop a tier" plan, ask — does the user already have a stronger free alternative?**
  If yes, **the correct fix is "switch channels", not "drop a tier"**.
- ⇒ ⚠️ **another mandatory check**: does the user use a platform feature **because "the model is stronger" or because "the feature exists nowhere else"**?
  - **just the model** ⇒ "switch channels" suffices, saves the full amount
  - **those features** ⇒ only "upgrade" remains, because what you buy is not the model, it is the platform
  **this difference decides whether the whole spend is justified; ask first.**

### 5. Annualize the cost before presenting it
- Saying "+$X/month" registers nothing; saying **"+$X×12 ≈ ¥Y/year"** registers.
- At the same time spell out **what you get for it** (ideally more than one thing):
  example: the upgrade does not just buy A, it also multiplies B, C, D by N ⇒ that is the complete "worth it" argument.
- ⚠️ **also check "can it still be upgraded further"** — the higher tier may be off-sale, turning a three-way choice into a **binary** one.

### 6. Conclusion: **binary + trigger line only**
```
Current judgment: 🟡 leaning 🔴 (toward upgrade), but discipline says wait 1–2 more days

Trigger line:
🔴 one more day of "getting capped"        ⇒ upgrade directly, do not wait for N days
🟡 only 1 capped day in N days and daily avg ≤ threshold ⇒ do not upgrade yet, switch to usage pattern XX
🟢 zero capped days across N days           ⇒ definitively do not upgrade
```
- 🔴 **never ask the user "what do you think"** — they came to you precisely because they do not want to compute it themselves.
- 🔴 **never leave the judgment hanging** — either give a conclusion, or give a trigger line **you yourself can execute**.

### 7. On new evidence, **revise immediately**, and say plainly "I was wrong last round"
- This skill got it wrong **4 times** in live use: misattributing a window to the wrong bucket,
  treating a setting's label as behavior, **assuming the user's "daily usage"**, and **discarding a reason on a false premise**.
  Every time because **causality was inferred from one line of copy / one name / one assumption**.
- ⇒ **before attributing cause, first ask or verify "what are the actual facts, how many buckets, what is his daily pattern".**
- ⇒ when revising, **state clearly "where I was wrong and what I changed it to"** — do not gloss over it; the user will trust your later conclusions more for it.
- ⇒ ⚠️ **do not become afraid to conclude because "I have admitted error many times"** — what the user wants is a **verdict**.
  The right move: **nail down the premises first, then give a firm conclusion + trigger line.**

---

## Deliverable shape

| Element | Requirement |
|---|---|
| Structure | **conclusion first** → gap/evidence → trigger line → annualized cost → action list |
| Goal | after reading, the user **only has to do one thing** (usually "tell me next time it happens") |
| Avoid | sending the user to a webpage to fill forms, compute numbers, or make the call themselves; no repeated questions |
| Ledger | if continuous tracking is needed, **the AI maintains one file itself**, the user dictates one sentence |
