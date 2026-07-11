---
name: on-ramp
description: Route a file-deliverable undertaking — grill the intent, then send it down the Projet path or run it as a one-off. Use when the user says they're assembling or building a dossier, grant application (dossier de subvention), tender response (réponse à appel d'offres), manuscript, or research corpus; wants to reach a "done" over a set of files; or mentions tracking what is still manquant.
---

# On-ramp

The single entry point to the file-deliverable skill bank. It reads the intent and routes on **one question**, so the operator never has to know the bank's map (ADR-0005).

This skill only *routes* and, for simple one-offs, *acts inline*. The heavy paths live in `cadrage`, `traiter`, `suivi`, and `classement` — name them, don't re-implement them.

## Scope

This bank drives a **Projet** whose deliverables are **files in a tree** to completion. It is **not** for code that needs a test runner / CI — that is the engineering skills' contract (ADR-0001). If the work compiles and tests, hand it back.

## Steps

### 1. Grill the intent

Ask enough to answer the routing question below — no more. Surface what the goal is, what has to exist for it to be "done", and whether files already exist that just need filing.

**Completion criterion:** you can state, in one sentence, what "done" means for this undertaking.

### 2. Route on one question

*Does this require assembling a set of **Éléments requis** over time?*

- **Yes → Projet path.** Tell the operator to run `/cadrage`. Stop here — do not start fixing the objectif yourself; `cadrage` owns that and the **Manifeste** it creates.
- **No.** One clarifying follow-up, not a second routing question: *do they already have a stock of files sitting around to file?*
  - **Yes → one-off filing.** Tell the operator to run `/classement`.
  - **No → inline one-off.** Run it here: write a **Definition-of-Done**, present the **Dry-run** against it, execute on confirm. No **Manifeste**.

**Completion criterion:** the operator has been pointed at exactly one of `/cadrage` or `/classement`, **or** an inline one-off has gone green against its Definition-of-Done.

### 3. Re-route if the true shape reveals itself

The routing question is asked early, before the work is understood in depth. If, once inline work or a `classement` filing is already underway, it becomes clear the honest answer was actually "yes" — the work needs a set of Éléments requis assembled over time — **stop and redirect to `/cadrage`**, in either direction:

- An inline one-off that turns out to need accretion over time → stop, tell the operator to run `/cadrage`.
- A stock handed to `classement` that turns out to be a Projet's own Éléments requis, not a bare filing job → stop, tell the operator to run `/cadrage` instead.

Nothing is lost by redirecting: the Dry-run gate means nothing has touched disk yet (ADR-0010). Don't try to salvage or continue the wrong path "for what's already started" — a Projet driven without a Manifeste has no source of truth for its state.

**Completion criterion:** if a misroute is caught, the operator is redirected to `/cadrage` before anything irreversible happens; if no misroute occurs, this step is a no-op.

## Reference

The routing question is the *only* branch that matters at the top level — branch on whether a set accretes over time, never on subject matter (a subvention and a manuscript take the same path). The classement-vs-inline choice is a cheap clarifying follow-up on the "No" leaf, not a peer routing decision. See ADR-0001, ADR-0005, and ADR-0010 (re-routing).
