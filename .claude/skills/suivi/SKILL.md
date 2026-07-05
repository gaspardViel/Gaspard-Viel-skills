---
name: suivi
description: Calcule le Suivi de complétude du Projet et propose les Relances dues, gated par un Dry-run.
disable-model-invocation: true
---

# Suivi

Reports where the **Projet** stands and proposes the **Relances** that are due. The reporting half of the loop; `/traiter` is the acting half. Reads — and, for Relance logs, appends to — the **Manifeste**.

## Steps

### 1. Read the Manifeste

Load the Projet's **Manifeste** — its Éléments requis, Statuts, deadlines, and Relance history.

**Completion criterion:** every Élément requis and its current Statut is in view.

### 2. Record any reported rejections

`obtenu` on a Livrable is optimistic — submitted, presumed fine, never a third-party
verdict (ADR-0007) — so a rejection is news the operator brings, not something the
Manifeste already shows. Ask: has any currently-`obtenu` Livrable come back `à corriger`
since the last run?

For each one reported:

- Capture the rejection **motif**.
- Capture the **resubmission deadline** — mandatory. If the third party gave none, ask
  the operator to set a target date themselves (the same fallback `cadrage` uses for the
  Projet's own deadline). Do not record the rejection without one.
- Set the element's Statut `obtenu → à corriger` and its Deadline to the resubmission
  date.
- Append a timestamped entry to the element's Relance log with the motif and new
  deadline. This entry *is* the fresh-cycle boundary step 4 relies on — no Dry-run gate
  here, since this only transcribes a verdict the third party already delivered, it sends
  nothing outward (ADR-0007).

**Completion criterion:** every `obtenu` Livrable has been checked; every reported
rejection has a motif, a resubmission deadline, an updated Statut/Deadline, and a
timestamped log entry.

### 3. Compute the Suivi de complétude

Aggregate **live** over each Élément requis's **Statut** — never read a stored total, which could have drifted (ADR-0004). This aggregate *is* the Definition-of-Done checklist instantiated over the Projet: the Projet is "ready to submit" only when its outstanding elements clear.

**Completion criterion:** the count of `obtenu` / `manquant` / `à corriger` is derived from the rows, and the outstanding elements are named.

### 4. Compute which Relances are due

Due-ness is evaluated **now**, at this invocation — never scheduled (ADR-0006). Walk each outstanding Élément requis and apply its type:

- **Livrable with a deadline** — read the cadence: the default ticks in the **Manifeste** header (default `T-14 / T-7 / T-2` before the deadline), unless the element's row carries an override. A tick is **due only if** the deadline has crossed its threshold *and* no Relance has been logged since that threshold was crossed — the per-element Relance log is the sole state, so re-running this step the same day never re-proposes an already-sent tick.
- **Missed ticks collapse.** If several ticks elapsed since the last logged Relance, mark only the **most recent** crossed one as due — never the backlog.
- **Livrable with no deadline** — not due. **Flag it**: "no deadline, no Relance until one is set." A missing deadline is almost always an oversight.
- **`à corriger` Livrable** — its deadline is the **resubmission date**, and the cadence runs a **fresh cycle** against it: prior log entries don't block the new ticks, because the rejection's own logged entry (step 2) marks where the fresh cycle starts.
- **Ressource** — never dated-due. It appears only in the step-3 completeness report, never as a scheduled Relance (ADR-0006).

**Completion criterion:** every outstanding element is marked due or not-due; each Livrable's decision cites the crossed tick (or the reason: no deadline / already logged / not yet reached); its Relance log was consulted before deciding.

### 5. Draft each due Relance, targeting by state

The target — and whether a Dry-run gate applies — depends on direction. **Outward** goes to a third party and is irreversible; **inward** just surfaces to the operator (ADR-0006):

- **`manquant` Livrable → outward.** Draft a message chasing the **third party** for the missing item. This is the only case that leaves the desk, so it is **Dry-run gated**: present the draft, send only on the operator's confirmation. Nothing outward fires unattended in v1.
- **`à corriger` Livrable → inward.** Surface a nudge to the **operator** to fix the element before the resubmission date (the fix is `/traiter` work); the third party is not re-contacted until resubmission. Carry the rejection motif so it reads as a correction, not a first ask. No outward send, so no gate — it points the operator at `/traiter`.
- **Ressource → inward.** Its nudge *is* the step-3 completeness line ("still missing X"); no separate message, no gate.

**Completion criterion:** every due Relance is drafted and correctly targeted; every **outward** (third-party) Relance has passed the operator's Dry-run before sending.

### 6. Log the Relances

Append every Relance that fired a dated tick to that element's log in the **Manifeste** — both the **outward** send and the **inward** `à corriger` nudge. This is what makes step 4 idempotent: an unlogged tick would re-propose itself on the next run. A Ressource line in the report is not a dated tick and is not logged.

**Completion criterion:** every dated tick acted on this run has a log entry, so re-running `/suivi` immediately proposes nothing new.
