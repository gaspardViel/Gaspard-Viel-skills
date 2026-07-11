# `obtenu` is optimistic; `suivi`, not `traiter`, records a Livrable's rejection into `à corriger`

`à corriger` has been in the flat Statut set since ADR-0004, and `suivi` already knows how
to *read* one and restart its cadence (ADR-0006) — but nothing specified how a Livrable
*gets there*, or who writes the resubmission deadline. This ADR closes that gap.

## `obtenu` is optimistic, not a verdict

For a Livrable, `traiter` sets `obtenu` at the moment the operator submits it (traiter
step 3–5) — before any third party has judged it. So `obtenu` means "sent, presumed fine
until told otherwise," not "accepted." We considered adding a fourth status for
"submitted, awaiting judgment," and rejected it: the Statut set is deliberately flat
(ADR-0004), not a submitter→reviewer→approver machine, and a rejection is exactly the
event that *contradicts* an optimistic `obtenu` — that's what `à corriger` is for.

## `suivi` records the transition, not `traiter`

Decision: `suivi`, at the start of its run (before computing the Suivi de complétude),
asks the operator whether any `obtenu` Livrable has come back rejected, and if so writes
`obtenu → à corriger` with the motif and resubmission deadline. `traiter` is left
untouched — it stays purely unidirectional (advance an element toward `obtenu`).

We considered putting this in `traiter` instead, since it already owns Statut writes
(traiter step 5). Rejected: `traiter`'s completion criterion is "this element reaches
`obtenu`" — a regression doesn't fit that shape, and folding it in would make `traiter`
check two directions instead of one. `suivi` is the natural home for a different reason
too: an `obtenu` Livrable is invisible to `suivi`'s own due-ness pass (ADR-0006 step 3
only walks outstanding elements), so `suivi` is the only place that can "wake" it back up
when a rejection is reported.

## No Dry-run gate on the recording

Decision: recording a rejection is **not** Dry-run gated. ADR-0006 already draws this
line for outward vs. inward: a Dry-run guards a consequential action *before it leaves the
desk* (traiter's write, suivi's outward Relance send). Recording a rejection sends
nothing — it transcribes something that already happened, reported by the operator. The
only check is that the agent confirms it understood the motif and deadline before
writing, not a formal preview/confirm cycle.

## The rejection is its own log entry

Decision: recording a rejection appends a **timestamped entry** to the element's Relance
log (motif + new resubmission deadline) — not just a Statut/Deadline update on the row.
ADR-0006 says the fresh cadence cycle after a rejection ignores prior log entries ("prior
log entries don't block the new ticks"), but that line is only mechanically checkable if
something marks where the fresh cycle starts. This entry is that marker: `suivi`'s
due-ness pass (ADR-0006 step 3) treats it as the boundary and disregards anything logged
before it for that element.

## The resubmission deadline is mandatory

Decision: `suivi` requires a resubmission deadline before it will record the rejection.
If the third party didn't give a firm one, the operator sets a target date themselves —
the same fallback `cadrage` already uses for the Projet's own deadline ("a real external
deadline if a Livrable has one, else the operator's target"). We considered reusing the
existing "no deadline" flag that a newly-enumerated Livrable gets (ADR-0006: flagged, not
blocked). Rejected for this case: a Livrable that is *already* `à corriger` is already
late against the third party, so letting it sit with no deadline recreates exactly the
blind spot the graduated cadence exists to prevent — worse than the neutral case of a
brand-new element that hasn't missed anything yet.
