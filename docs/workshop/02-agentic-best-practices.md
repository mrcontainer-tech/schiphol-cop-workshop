# Agentic engineering - the nine habits

The [participant guide](./01-participant-guide.md) gives you the loop; this guide gives you the habits behind it. They separate people who *ship* with agents from people who fight them. None of this is theory: every practice below is wired into this repo somewhere, and today you'll trip over each one at least once.

## 1. Human plans, agent executes

**You own the plan. The agent owns the keystrokes.**

Planning is where your engineering judgment has the most leverage: a wrong plan costs you a sentence to fix, a wrong implementation costs you a 400-line diff review and a revert. Read every plan critically and push back on at least one thing, every time. If you can't find anything to push back on, you haven't read it as its reviewer yet.

*Today:* step 3 of the loop. The guided exercise showed the host pushing back on a fine plan on purpose; copy that reflex.

## 2. Start fresh, and start with a plan

**One ticket, one clean session.**

Long chat histories are context pollution: half-abandoned approaches, stale file contents, and old ticket details all stay in the window and quietly steer new work. A fresh session that starts from the ticket key gets the current state of the repo and nothing else.

*Today:* when you pick up Exercise 3, close the Exercise 2 chat. Yes, really. The jira skill makes rehydrating context a one-liner, so a fresh start costs you nothing.

## 3. Two strikes, then reset

**If the agent fails twice at the same thing, stop prompting harder. Start again.**

A failing thread is sunk cost. "No, try again", "that's still wrong", "please just..." teaches the agent nothing and burns your context window on a record of failure, which makes the next attempt *worse*, not better. After two misses: kill the session, and restart with a tighter plan that encodes what you learned. Name the file. Forbid the approach that failed. Shrink the task. What you learned survives the reset; the flailing doesn't.

*Today:* the moment you notice you're negotiating with the chat instead of steering it, that's the signal. Reset. It feels like losing progress; it's the fastest move available.

## 4. Small, scoped tasks

**Agents are strongest inside a well-bounded working set.**

An agent juggling one module, one behavior, and one acceptance list is precise. An agent asked to "refactor the app and also fix the stats" invents scope, and invented scope is where hallucinated requirements live. One ticket = one branch = one PR. If the plan comes back with more than about five steps, the task is too big: split the ticket, not the difference.

*Today:* the exercises are pre-scoped to 20-45 minutes each. Notice how rarely the agent wanders when the ticket is tight. That's not the model being good; that's the ticket being good.

## 5. Harness engineering

**Invest in the repo, not in heroic prompts.**

Anything you find yourself typing into chat twice belongs in a file: collaboration policy in `.github/copilot-instructions.md`, per-path style in `.github/instructions/`, tool permissions and the change ritual in `AGENTS.md`, recurring procedures in `.github/skills/`, past decisions in `docs/adr/`, and exact repro commands in tickets. This repo is the worked example: everything the agent does well today, it does because a file taught it, not because anyone prompted well.

*Today:* watch the layers fire. Say "pick up the ticket" and the jira skill loads. Touch a `.py` file and the style rules apply. That behavior is a PR away from any repo you own.

## 6. Fast, deterministic feedback loops

**Agents iterate at the speed of your test suite.**

The reproduce-fix-verify loop only works when verification is instant and repeatable. Here, pytest runs in well under a second, the seed data never changes, and every bug ticket states the exact command and exact wrong output. Give an agent a flaky suite or a "run the staging pipeline and wait" loop and it starts guessing, because guessing is cheaper than waiting.

*Today:* this is why every ticket has a one-line repro. Feel free to be spoiled by it, then go build the same thing at work.

## 7. Verify like your name is on the commit (it is)

**Green is not the same as correct.**

This codebase ships with a green test suite and three live bugs. Let that sink in before you trust any "all tests pass" from an agent. Run the ticket's repro yourself. Demand the regression test and make the agent prove it fails on the old code. Read every hunk of `git diff`; anything you don't understand, you ask about before the PR, because the reviewer of record is you, not the model.

*Today:* steps 5 and 6 of the loop, and the entire point of Exercises 1-3.

## 8. Context is a budget

**Give the agent handles, not walls of text.**

Pasting three files and the whole ticket history into chat doesn't make the agent smarter; it dilutes the signal and crowds out the working space. Hand over a ticket key and let the jira skill fetch exactly what's needed. Let path instructions and skills load on demand instead of front-loading every rule. Relevant beats voluminous, every time.

*Today:* "Pick up OPS-3" is the whole prompt that starts an exercise. Watch how much arrives without you pasting anything.

## 9. Decisions stay human

**Agents implement decisions. They don't get to make them silently.**

The expensive failure mode is not wrong code; it's plausible code that quietly commits your team to an undecided thing: a data format, a dependency, an API shape. Any plan that contains an unmade decision stops until a human makes it, and the decision gets written down where the next session will find it. In this repo that artifact is the ADR.

*Today:* Exercise 5 refuses to let you code before ADR-0008 exists. That friction is the feature.

## Cheat sheet

| Trigger | Habit |
|---------|-------|
| New ticket | Fresh session, plan first, push back once (#1, #2) |
| Plan has an unmade decision in it | Stop; decide; write the ADR (#9) |
| Plan has more than ~5 steps | Split the ticket (#4) |
| Agent failed twice at the same thing | Reset with a tighter plan; stop prompting (#3) |
| You typed the same instruction twice this week | Move it into the harness: instructions, skill, or AGENTS.md (#5) |
| Agent is guessing instead of checking | Your feedback loop is too slow or flaky; fix that first (#6) |
| "All tests pass" | Run the repro, read the diff, demand the failing-then-passing regression test (#7) |
| About to paste a wall of code into chat | Hand over the key/path and let the tooling fetch it (#8) |

## Monday morning

You can't take the exercises home, but the harness travels: copy `copilot-instructions.md`, one path-scoped instructions file, `AGENTS.md`, and one skill into a repo you own, adapted to your team's rituals. Then enforce two habits for a week: plans before code, and two-strikes-then-reset. Those two alone change how the tool feels. The other seven follow from the first time they save you an afternoon.
