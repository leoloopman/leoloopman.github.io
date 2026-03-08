---
layout: post
title: "The agent harness (what it is, and why it matters)"
date: 2026-03-08 00:00:00 +0000
---

I keep seeing the same mistake in agent projects: we argue about the model, then we ship a demo, then it falls apart the moment the task takes longer than an hour.

The bottleneck usually isn't "agent intelligence". It's the harness.

By "harness" I mean everything around the model that makes work reliable over hours or days: the environment, the artifacts it writes, the loop it follows, the checks that prevent it from lying to itself, and the handoffs that let a new run pick up where the last one stopped.

A practical definition:

An agent harness is "instructions + tools + workflow + state".

If you squint, it's what turns a model into a worker that can:
- gather context,
- plan,
- execute with tools,
- verify results,
- persist progress,
- recover from failures,
- and hand off across sessions without losing the plot.

## Why agents fail on real tasks

The core failure mode is boring: long tasks don't fit in one context window.

When a run ends mid-task, a naive system starts the next run like a new engineer walking onto a shift with zero memory. You can try summarization and compaction, but summaries are not the same thing as a durable record of what changed, what's broken, and what still needs to happen.

In practice you see the same few problems over and over:

- One-shotting: the agent tries to do too much in one go, runs out of context halfway through, and leaves the repo in a weird in-between state.
- Broken handoffs: the next run has no clear "here's what happened" and no trustworthy "here's what's next".
- Premature victory: the agent sees progress and declares done (often because the local feedback loop is weak).
- False-done features: tasks get marked complete without any end-to-end check that a user journey works.

If you've ever watched an agent confidently tell you it "fixed the issue" without running the tests, you know the vibe.

This is why the harness matters. It's not motivational. It's operational.

## The harness gives you external memory and a feedback loop

A good harness does two things.

First, it creates external memory the agent can read and trust. Not "the agent remembers", but "the repo remembers".

Second, it forces a feedback loop where the agent has to look at reality (tests, logs, UI behavior) instead of vibes.

That sounds abstract, so here's the simplest mental model that I've found useful:

- The model generates proposals.
- The harness turns proposals into work.
- The harness also tells the model when it's wrong.

## The emerging pattern: initializer + incremental loops

There's a pattern showing up across teams that run long-lived coding agents:

1. An "initializer" run that sets the repo into a known-good shape and writes down what matters.
2. A loop that does one small chunk of work, verifies it, commits it, and leaves a clear trail for the next session.

Anthropic describes a two-agent variant of this for long-running tasks. The key detail isn't that it's "two agents". It's the structure: the first run creates the scaffolding, later runs behave like a disciplined engineer with a ritual.

What the initializer typically does:
- creates an `init.sh` (or equivalent) to boot the environment and run basic checks
- writes a progress log file (append-only)
- expands the vague prompt into a concrete feature list with acceptance criteria
- makes an initial git commit so there's a clean baseline

What the loop typically does on each run:
- reads progress + git history
- runs the smallest useful smoke test
- picks one feature
- implements it
- verifies it
- commits with a descriptive message
- updates the progress log so the next run doesn't guess

None of this is fancy. It's just the stuff humans do when they're trying not to get lost. The difference is that agents won't do it unless the harness makes it cheap and automatic.

## What "best practices" actually look like today

If you're building an agent that works on a repo, these are the parts I'd treat as non-negotiable:

- Persistent state:
  - a progress log (append-only)
  - git discipline (small commits, good messages)
  - requirements that can't be casually rewritten

- Work decomposition:
  - one feature per run or per PR
  - explicit acceptance criteria (steps + expected outcome)

- Verification-first:
  - run tests, always
  - include at least one end-to-end journey for anything user-facing
  - use browser automation when the UI is the truth

- Guardrails:
  - enforce invariants mechanically (CI gates) instead of hoping the prompt sticks
  - sandbox risky operations

- Context management:
  - keep a short map doc (I like `AGENTS.md`) that points to canonical sources
  - avoid giant manuals that rot and waste context

This is the stuff that makes agents boring in the best way. Less magic. More shipping.

## A quick note on evaluation harnesses

Execution harnesses make the agent run.

Evaluation harnesses make the output good.

If you don't have some kind of scoring or gate, you'll keep arguing with your agent about whether it's "done". That's not a fun way to live. It's also a sign your harness is underbuilt.

I'll write about evaluation harnesses separately because it deserves its own post.

## Closing thought

When an agent fails, the easy story is "the model isn't good enough".

Sometimes that's true. Most of the time it's not.

Most of the time, the agent failed because it didn't have a memory it could trust, a loop that forced verification, and guardrails that made it hard to do the wrong thing fast.

That's harness work. It's not glamorous, but it's the difference between a demo and a system.

---

Sources:
- Anthropic: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents
- OpenAI: https://openai.com/index/harness-engineering/
- Cursor: https://cursor.com/blog/agent-best-practices
- Paddo.dev: https://paddo.dev/blog/agent-harnesses-from-diy-to-product/
- DEV: https://dev.to/n_asuy/the-secret-isnt-the-model-its-the-harness-587a
