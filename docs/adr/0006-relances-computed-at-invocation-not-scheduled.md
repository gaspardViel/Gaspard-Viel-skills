# Relances are computed at invocation and gated by Dry-run, not background-scheduled (v1)

The completeness research (`docs/research/existing-completeness-tracking-tools.md`) favored
automatic, deadline-based, graduated reminders (DocuSign's cadence). But a *truly*
automatic Relance requires a background scheduler that sends outward-facing, irreversible
emails with no human in the loop — which contradicts ADR-0002's Dry-run gate, whose whole
point is that consequential/irreversible actions get human confirmation first.

Decision (v1): `suivi` computes which Relances are *due* at invocation time — never a
background daemon — then drafts them and requires human confirmation before sending (the
Dry-run).

## Cadence policy

A **graduated cadence** of dated ticks relative to a **Livrable**'s own deadline (default
T-14 / T-7 / T-2). The default policy lives in the **Manifeste** header, written by
`cadrage`, and is **overridable per-Livrable** via an optional field on the element's row —
so the common dossier sets it once, and a slow-administration Livrable can widen its own
window without forcing every element to be configured.

## Tick semantics

Because invocation is manual, a tick is not a scheduled event that fires at its date — it
is a condition evaluated whenever `suivi` runs:

- **At most once.** A tick is due only if the deadline has crossed its threshold *and* no
  Relance has been logged since that threshold was crossed. The per-element Relance log in
  the Manifeste is the sole state, making the computation idempotent: re-running `suivi`
  the same day does not re-propose an already-sent tick.
- **Missed ticks collapse.** If the operator skipped `suivi` and several ticks elapsed,
  only the **most recent** crossed tick is proposed — never the full backlog. Relancing a
  third party twice in a day because *the operator* was late is noise that works against
  them.

## Subtype split

- A **Livrable** has a real external deadline → the dated cadence above.
- A **Ressource** usually has no deadline → surfaced only in the completeness report on
  demand ("what am I still missing?"), **never** nudged on a schedule (the research's
  explicit anti-pattern).
- A **Livrable with no deadline set** is almost always an oversight, not a choice, so
  `suivi` **flags** it ("no deadline, no Relance will be proposed until one is set") rather
  than silently letting it slip past the point where anyone could be chased.

## `à corriger`

When a Livrable is sent back `à corriger`, the rejection carries a **resubmission
deadline** that replaces the element's original (now-past) deadline, and the cadence
**restarts** against it (a fresh cycle; prior log entries don't block the new ticks).
Crucially, the resulting Relance **targets the operator** — a nudge to fix the element
before resubmission (the fix itself is `traiter` work) — **not** the third party, who is
not re-contacted until the corrected Livrable is resubmitted. The rejection motif colors
that nudge and is cited in the message that accompanies the eventual resubmission.

## v2 evolution

A background scheduler remains a v2 evolution — but not the auto-relance-to-third-party
that this ADR originally imagined. The v2 path is a **léger scheduled nudge to the
operator** (via the `schedule` skill): it wakes on the cadence, computes due Relances, and
delivers the drafts to the operator's inbox. The outward send to the third party stays
**Dry-run gated** in every version. v1 ships the due-ness computation (the brain); v2 only
adds the delivery channel (the postman).
