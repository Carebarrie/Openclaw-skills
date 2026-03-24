---
name: context-orchestrator
description: Self-steering agent layer for planning, context control, execution loops, and recovery. Use when coordinating complex tasks, preventing context drift, enforcing constraints, and resuming interrupted work safely.
---

# Context Orchestrator

Run this skill as a control layer before and during execution.

## Core Loop

1. Frame objective
2. Bound context
3. Plan next smallest action batch
4. Execute
5. Verify against definition-of-done
6. Log checkpoint
7. Continue or stop

## Operating Rules

- Keep scope strict; reject unrelated branches unless explicitly approved.
- Convert vague requests into explicit objective + constraints + done criteria.
- Prefer short iterative cycles over large one-shot runs.
- Keep a running checkpoint so work can resume after interruption.
- If confidence is low or risk is high, pause and ask for confirmation.

## Start-of-Task Protocol

Use `references/task-frame-template.md` to define:
- objective
- constraints
- non-goals
- done criteria
- risk notes

## Mid-Task Protocol

Use `references/steering-checklist.md` every cycle to prevent drift.

## Recovery Protocol

On crash/interruption, load `references/recovery-protocol.md` and resume from latest checkpoint.

## Deliverable Protocol

Before final output:
- verify all done criteria
- list what was completed
- list what remains (if any)
- provide next optional step
