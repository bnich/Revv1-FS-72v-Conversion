# CLAUDE.md — Revv1 FS 72 V conversion

Parent guidance: `../CLAUDE.md`. It carries the repo map, the document conventions, the public-repo
hygiene rules and the cross-cutting safety rules. **Read it first; this file adds only what is
specific to this repo.**

## What this repo is

Not a software project. The umbrella for a physical build: procurement, execution steps, shop
instructions, the issues log and the safety rules. There is no build/test/lint tooling — the work is
reading, editing, and keeping the documents accurate and mutually consistent.

## The three documents that must stay in sync

`docs/build-sheet.md`, `docs/checklist.md` and `docs/work-order.md` describe the **same build** at
three audiences. A change to a shared fact usually needs all three.

- cost and rationale → build sheet
- ordered steps → checklist
- hand-to-the-shop summary → work order

⚠️ **The checklist is what the owner prints and follows step by step**, so it has a higher bar than
the others: no step may depend on a part that is not in its own consumables section, and no phase may
require power before the phase that supplies it.

## The issues log is a source of build facts

`docs/issues-log.md` is not a diary. **When an issue closes, push the resolution into the checklist
steps, the work-order BOM and sequence, and the build sheet's order tracker.** Open issues belong in
the checklist's blockers list, not only in the log.

## ⚠️ The public build sheet is deliberately redacted

`docs/build-sheet.md` differs from any older private copy **on purpose**: financing arrangements and a
personalised vendor discount code were removed. Prices stayed, because they are useful to anyone
costing the same build. **Do not "resync" those back in.**

## Totals

The build sheet's totals (Committed / Remaining / All-in) and its order-tracker table must be
**recomputed together** whenever an item's status or cost changes.

## Scope

Mounts, the ESP32 module, the `.heb` tooling and the display protocol work live in their own repos —
see the map in `../CLAUDE.md`. Link to them; do not copy their facts here.
