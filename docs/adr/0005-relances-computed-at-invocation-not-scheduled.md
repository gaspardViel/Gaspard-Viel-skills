# Relances are computed at invocation and gated by dry-run, not background-scheduled (v1)

The completeness research (`docs/research/existing-completeness-tracking-tools.md`) favored
automatic, deadline-based, graduated reminders (DocuSign's cadence). But a *truly* automatic
Relance requires a background scheduler that sends outward-facing, irreversible emails with
no human in the loop — which contradicts ADR-0002's dry-run gate, whose whole point is that
consequential/irreversible actions get human confirmation first.

Decision (v1): the agent computes which Relances are *due* at invocation time — from each
element's deadline plus a graduated cadence policy stored in the manifest — then drafts them
and requires human confirmation before sending (the dry-run). Split by subtype:
- a **Pièce justificative** has a real external deadline → graduated proposals as the date
  approaches (e.g. T-14 / T-7 / T-2), not a single T-1 warning;
- a **Ressource de cadrage** usually has no deadline → surfaced only on demand ("what am I
  still missing?"), never pushed on a schedule (the research's explicit anti-pattern).

A Relance on an `à corriger` element carries the rejection motif, distinguishing it from an
initial ask. A background scheduler (via the `schedule` / `loop` skills) that fires unattended
relances is a possible **v2 evolution**, not the v1 default.
