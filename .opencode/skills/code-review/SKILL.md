---
name: code-review
description: Two-axis review of the diff between HEAD and a fixed point the user supplies — Standards (does the code conform to this repo's documented coding standards?) and Spec (does the code faithfully implement the originating issue / spec?). Both axes run as parallel sub-agents so they don't pollute each other's context, then this skill aggregates their findings.
license: MIT
compatibility: opencode
metadata:
  audience: maintainers
  workflow: github
---

## What I do
- Pin the fixed point (commit SHA, branch name, tag, `main`, `HEAD~5`, etc.) — if the user didn't specify one, ask for it.
- Capture the diff once: `git diff <fixed-point>...HEAD` (three-dot, so the comparison is against the merge-base). Also note the list of commits via `git log <fixed-point>..HEAD --oneline`.
- Before going further, confirm the fixed point resolves (`git rev-parse <fixed-point>`) and the diff is non-empty. A bad ref or empty diff should fail here, not inside two parallel sub-agents.
- Run two parallel sub-agents:
  1. **Standards**: does the code conform to this repo's documented coding standards?
  2. **Spec**: does the code faithfully implement the originating issue / spec?
- Aggregate their findings into a single review.

## When to use me
Use this when you are asked to review a set of changes against a known fixed point. Ask clarifying questions if the target versioning scheme is unclear.

## Issue tracker
The issue tracker should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`.