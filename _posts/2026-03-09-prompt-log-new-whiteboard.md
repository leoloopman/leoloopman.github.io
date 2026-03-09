---
layout: post
title: "The prompt log is the new whiteboard"
date: 2026-03-09 00:00:00 +0000
---

The old technical interview had one sacred artifact: the whiteboard.

Not because whiteboards were great, but because they forced something useful. You had to show your thinking. You couldn’t hide behind “it works on my machine.” The interviewer could see how you framed the problem, where you hesitated, and what you did when you hit a wall.

AI tools break that artifact.

When a candidate can use a coding agent, the final code stops being the interesting part of the story. They can generate plausible code in seconds. They can also generate plausible nonsense in seconds. Output is cheap now.

So what replaces the whiteboard?

I think it’s the prompt log.

Not “the prompt” as a party trick. The whole trail: what you asked for, what the agent suggested, what you accepted, what you rejected, what you changed, what you verified, and where you stopped and re-scoped because the original plan was wrong.

That trail is the new evidence of engineering judgment.

## Why the old interview breaks

Most of us already knew the whiteboard interview was a proxy. It tested some real things (communication, basic problem solving) and a lot of noise (nerves, memorized patterns, working without your normal tools).

But it did have one advantage: it made reasoning visible.

With agents in the loop, that visibility disappears unless you deliberately bring it back. People paste, run, and move on. And you get a weird new failure mode: the code works, but the candidate can’t explain the first line.

That’s not a “kids these days” rant. It’s a measurement problem. If you only grade the final diff, you’re not interviewing the engineer. You’re interviewing the tool.

In 2026, “can you get to a solution” is table stakes.

The actual question is: “Would I trust you to ship with this toolchain?”

## The prompt log makes judgment legible

A good prompt log is basically a decision record. It shows things you actually care about on a team:

- Did you clarify the requirement, or did you guess?
- Did you break the work into pieces, or did you dump everything into one mega-prompt?
- Did you verify the output, or did you trust it because it sounded confident?
- When the agent was wrong, did you notice quickly?
- Can you explain the result in plain English?

This is the stuff good engineers already do. The prompt log just makes it visible again.

## “AI allowed” is not the point. “AI auditable” is.

Letting candidates use AI and then scoring only the final output is a recipe for confusion. Some people will look amazing because the tool carried them. Some people will look slow because they insisted on doing everything manually. Neither tells you much.

A better setup is:
- AI is allowed
- the interaction trail is captured
- verification steps are visible
- decisions are discussable

That’s what turns it back into an interview.

## What an AI-native interview looks like (for engineers)

Forget puzzles. Use a small repo.

The task should force real engineering behaviors:
- reading existing code
- making a change without breaking everything
- adding or adjusting tests
- explaining tradeoffs

A structure that works well:

1) Give a multi-file mini-project (service, CLI, tiny app)
2) Provide an agent in the environment
3) Require two artifacts at the end:
   - the code change
   - the prompt log
4) Use the live time to talk through the loop: framing, delegation, verification, integration

Speed still matters, but you can’t be sloppy and call it “productive.”

## Make “AI power” explicit

One trick I like is to make the agent’s power level obvious. You can do it in phases:

- Phase 1: the agent can answer questions but cannot edit code
- Phase 2: the agent can propose edits (diffs), but the candidate applies them
- Phase 3: the agent can edit files directly

This isn’t about controlling the candidate. It’s about turning the interview into a controlled experiment.

Some people are careful when the tool is read-only, then get reckless the moment it can write files. That’s useful signal.

## What to score (a rubric that matches real work)

You don’t need a 20-row spreadsheet. You need signals that map to real incidents.

### 1) Framing
- Did they restate the problem clearly?
- Did they identify constraints and failure modes?
- Did they define “done” in testable terms?

### 2) Delegation
- Did they break work into chunks the agent can actually execute?
- Did they provide the right context?
- Did they avoid asking the agent to “decide the architecture” without guardrails?

### 3) Verification
- Did they run tests or build a minimal smoke check?
- Did they add a test for the edge case that matters?
- Did they verify behavior instead of just reading code and nodding?

### 4) Ownership
- Can they explain why the change works?
- Did they simplify or clean up agent-generated code?
- Did they catch bugs that look fine at a glance?

### 5) Prompt log quality

This is not “beautiful prompting.” It’s decision quality.
- Are there decision points?
- Are re-scopes explained?
- Are rejections explained?
- Are checks and verifications recorded?

If the log reads like “asked agent, accepted, shipped,” that’s a red flag.

## A PROMPTLOG.md template you can copy

I like boring formats because they’re searchable and hard to game.

```md
# Prompt log

## Task
- Goal:
- Constraints:
- Definition of done:
- Risks to watch:

## Plan (before touching code)
- Step 1:
- Step 2:
- Step 3:

## Agent interactions (chronological)

### 2026-03-09 15:10
Prompt:
- ...

Agent output (summary):
- ...

Decision:
- Accepted:
- Rejected:
- Edited:
Reason:
- ...

Verification:
- Commands run / checks performed:
- Results:
- Follow-up:
```

You can keep this as rough notes during the interview and clean it up afterward. The point is simple: make your thinking visible.

## The punchline

Whiteboard interviews were never really about whiteboards.

They were about exposing reasoning.

Now that agents can generate code on demand, reasoning is the scarce resource in the loop. The prompt log is the new whiteboard, because it’s the best evidence we have that someone can steer the tools instead of being dragged around by them.
