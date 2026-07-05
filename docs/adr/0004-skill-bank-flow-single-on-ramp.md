# Non-dev skill bank flow: a single on-ramp routes one-off vs Projet

The bank mirrors the engineering flow (grill → build → verify) but adapts it to non-dev
work. We considered separate entry points per task type; rejected, following ADR-0001's
principle of branching on task complexity, not on subject matter.

Decision: a single **on-ramp** skill grills the intent and routes on one question —
*"does this require assembling a set of Éléments requis over time?"*
- **Yes → Projet path**: a `cadrage-projet` skill fixes the objectif and enumerates the
  Éléments requis, creating the manifest + Classement; then the `traiter` / `suivi-complétude`
  / `relance` loop runs.
- **No → one-off path**: Definition-of-Done → dry-run → execute → verify, with no manifest
  (e.g. "rename and file these 40 invoices by month").

Notable shapes recorded so they aren't "fixed" later by mistake:
- **No `to-issues` equivalent** — the Éléments requis *are* the issues.
- **Verification is an embedded discipline** (the ADR-0002 Definition-of-Done + dry-run
  loop lives inside every acting skill), not a standalone skill.
- **`classement` is its own skill** — filing/organizing an existing stock of documents into
  the tree is a common one-off, distinct from advancing a Projet, so it stands alone rather
  than being folded into `traiter`.

(Skill names above are working names, not yet finalized.)
