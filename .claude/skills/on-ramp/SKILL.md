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
- **No, but they have a stock of files to file → one-off filing.** Tell the operator to run `/classement`.
- **No → inline one-off.** Run it here: write a **Definition-of-Done**, present the **Dry-run** against it, execute on confirm. No **Manifeste**.

**Completion criterion:** the operator has been pointed at exactly one of `/cadrage` or `/classement`, **or** an inline one-off has gone green against its Definition-of-Done.

## Reference

The routing question is the *only* branch that matters — branch on whether a set accretes over time, never on subject matter (a subvention and a manuscript take the same path). See ADR-0001 and ADR-0005.
