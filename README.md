# subscription-upgrade-triage

Decide whether a subscription is worth upgrading — and get a **binary verdict with a
machine-checkable trigger line**, not an essay.

A skill for Claude Code, WorkBuddy, Codex and any SKILL.md-compatible agent.

## Install

```bash
npx skills add zihao0639-netizen/subscription-upgrade-triage
```

or via SkillHQ:

```bash
skillhq install subscription-upgrade-triage
```

## What it does

"Should I upgrade to Pro?" looks like a math problem. It is actually a **facts problem** —
most wrong answers come from mistaking one quota window for another, or assuming the
user's daily usage from a single observed peak.

This skill enforces a seven-step discipline:

1. Check **official** sources only — never secondhand
2. Separate **quota buckets** — window A is usually not window B
3. Verify empirically: **at reset time, does the feature actually come back?**
4. Plan by **peak, not average** — and clarify real daily usage first
5. Hunt for a **zero-dollar equivalent** before adding money (and check the user does not
   already own a stronger free alternative)
6. **Annualize** the cost, then give a binary answer + trigger line
7. Revise immediately on new evidence — and say plainly where the last round was wrong

## Why it exists

Every step exists because it went wrong in live use — including four wrong conclusions in
one session, all from inferring causality out of a line of copy or an assumption. The
skill bakes those failures in as checks, so the same traps catch the next agent instead
of the user.

The core principle: **the user does not want analysis, they want a verdict** — binary,
with a trigger line they never have to think about again.

## Contents

```
skills/subscription-upgrade-triage/
└── SKILL.md    # the full methodology (single self-contained file)
```

## License

MIT
