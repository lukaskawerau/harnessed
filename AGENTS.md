# AGENTS.md

## What This Repo Is

`harnessed` is a library of skills for improving agent effectiveness in existing repos.

Core framing:
- "harness ready" is a heuristic, not an absolute truth
- what matters depends on the project, stack, team, runtime, and risk profile
- these skills should surface useful questions and tradeoffs, not issue commandments

## How To Use These Skills

When you audit a repo:
- treat scores as rough signals, not objective truth
- explain why a recommendation matters in this specific repo
- call out when a recommendation does not fit the local context
- prefer "have you considered X?" over "you must do X"

When you apply a fix:
- adapt templates to the target repo
- preserve existing conventions when they are reasonable
- avoid adding process or tooling without a clear payoff
- say what is opinionated vs what is required by the repo's actual constraints

## Preferred Language

Prefer:
- "suggested"
- "consider"
- "likely helpful when"
- "not needed here because"
- "tradeoff"

Avoid:
- "absolute"
- "required by harnessed"
- "the only correct setup"
- "this repo is not ready" without context

## Repo Notes

Human readers and agent readers should come away with the same message:
- this repo is guidance
- recommendations are contextual
- the goal is better judgment, not checklist theater
