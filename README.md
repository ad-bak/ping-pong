# TWO AI ONE DEV

A Claude Code skill that runs two genuinely separate AI instances on your coding task — one that implements, one that reviews. They iterate until the work is actually done.

```
/ping-pong
> What's your task?
> fix the leaderboard to track points from all game modes

SUPERVISOR: Here's the plan:
1. Change setDoc to increment() in src/data/leaderboard/index.ts
2. Add submitGlobalScore to saga branch in ResultsScreen.tsx
3. Add submitDailyScore to classic branch in ResultsScreen.tsx
TASKS_READY

[Task 1/3]
DEV: Changed setDoc to increment(). increment() was already imported. DONE

SUPERVISOR: Correct. Pattern matches the global leaderboard. NEXT

[Task 2/3]
DEV: Added submitGlobalScore and submitDailyScore to saga branch. DONE

SUPERVISOR: Saga branch looks right. Check ResultsScreen.tsx line 71 — you're
passing totalScore but the daily schema expects sessionScore. Fix that. REVISION

DEV: Fixed. Was using the wrong variable. DONE

SUPERVISOR: Correct. NEXT
...
APPROVED

✓ All tests passed.
```

---

## Why two agents?

One brain in two modes will rationalize its own decisions. A fresh instance with a different system prompt has no memory of *why* the code was written that way — only what it sees. That's what makes the disagreement real.

The Supervisor re-reads the actual files after every step. It doesn't trust the Developer's report.

---

## Installation

From your project root:

```bash
mkdir -p .claude/skills && git clone https://github.com/ad-bak/ping-pong.git /tmp/ping-pong && cp -r /tmp/ping-pong/ping-pong .claude/skills/
```

Requires [Claude Code](https://claude.ai/code).

---

## Usage

```
/ping-pong
```

Then describe your task. Claude will ask if anything is unclear before starting.

**Dry run** (plan only, no changes):

```
/ping-pong --dry
```

---

## How it works

1. **Supervisor plans** — reads the codebase, produces a numbered task list, flags anything uncertain before a single file is touched
2. **Developer executes** — one task at a time, scoped, no wandering
3. **Supervisor reviews** — re-reads changed files, catches regressions, points to exact lines
4. **Loop** until all tasks pass review
5. **Tests run once** at the end, not after every step
6. **You get pulled in** only when a real decision is needed

Max 8 agent turns. If the loop stalls, it surfaces to you.

---

## Unexpected bugs

If the Developer finds a pre-existing bug:
- **Blocks the task** → fixes minimally, flags it
- **Doesn't block** → flags it, doesn't touch it
- **Supervisor decides** whether to fix now, defer, or ask you

---

## License

MIT
