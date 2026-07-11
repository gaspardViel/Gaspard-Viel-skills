# File-Deliverable Project Skill Bank

A skill bank that helps a **single operator** drive any project whose deliverables are
**files in a tree** — an administrative dossier, a grant application, a tender response,
a manuscript, a research corpus — to completion, by fixing the required set of elements
up front, advancing them one by one, tracking completeness as a derived value, and
chasing the gaps until a "done" criterion is met. It excludes code that needs a test
runner / CI (engineering skills already serve that contract, ADR-0001); here the
verification loop is a Definition-of-Done checklist plus a dry-run (ADR-0002).

The bank's shape follows the **three load-bearing jointures** that the lifecycle research
(`docs/research/common-project-lifecycle-across-disciplines.md`) found to be the real,
independently-attested invariants — (a) fix the required set in advance, (b) track
completeness as a *derived* value, (d) define "done" — **not** the five-phase linear
sequence, which that same research shows is one lineage's convention, not a universal law
(ADR-0003).

The vocabulary below is the project's ubiquitous language. Prefer these terms (French,
as the real-world domain uses them) over their English or platform-specific synonyms.

## Language

### The unit of work

**Projet**:
One operator's undertaking with a goal to reach and an end, requiring a defined set of
Éléments requis to be assembled before it can complete. Single-actor: a jury, an
administration, a client, or a publisher is an external *target* of a Relance, never a
participant we model states for.
_Avoid_: dossier, case, task, project (English).

### Required elements

**Élément requis**:
One item the Projet needs before it can be considered complete. Carries an explicit
Statut of its own; completeness is never inferred from mere file existence. Has two
subtypes below, split by one asymmetry: *is it judged by a third party or only consulted
by the operator?*
_Avoid_: requirement, item.

**Livrable**:
A subtype of Élément requis that is *submitted to a third party* (a jury, an
administration, a client, a publisher). Because it is judged externally, it can be
rejected and sent back — so it alone can carry the `à corriger` status, and it alone
carries a real external deadline that drives a graduated Relance cadence. Carries a
**judge** (who receives and can reject it — always present) and, optionally, a distinct
**source** (whoever the operator is actually waiting on to supply it, when that isn't the
operator themselves — e.g. a recommendation letter's recommender is not the jury judging
the assembled dossier) (ADR-0008). `à corriger` assumes a rejection that can be corrected
and resubmitted *to the same judge*; a **terminal** rejection (no second attempt
possible) invalidates the Projet's plan itself, not the element's Statut — it sends the
operator back to `cadrage` to drop the Livrable or open a new one against a different
judge, not to a status `suivi`/`traiter` need to know about.
_Avoid_: deliverable (English), document, pièce justificative (too admin-specific).

**Ressource**:
A subtype of Élément requis that is *only consulted by the operator* to frame or guide
the work (guidance, a template, reference material) — never submitted, never judged by a
third party, so it is never `à corriger` and never carries an external deadline.
_Avoid_: resource (English), reference, ressource de cadrage.

### Status

**Statut** (of an Élément requis):
Where a single Élément requis stands. The flat set is **obtenu**, **manquant** (not yet
obtained), and — for a Livrable only — **à corriger** (submitted but sent back as
wrong/incomplete). **obtenu** means different things per subtype: for a Ressource, in
hand and valid (nothing left to check); for a Livrable, submitted and presumed fine —
optimistic, not a third-party verdict, since the system has no way to know the verdict
until told (ADR-0007). A rejection is exactly the event that contradicts that optimism
and moves a Livrable to `à corriger`. Deliberately flat, not a branched
submitter→reviewer→approver machine — there is no separate "submitted, awaiting
judgment" status.
_Avoid_: present/absent, complete/incomplete, en construction/en instruction (a
platform's states, not ours).

**Suivi de complétude**:
The Projet's completeness, always *computed live* as an aggregate over each Élément
requis's Statut — never stored as an independent number that could drift. For a Projet,
this *is* the Definition-of-Done checklist (ADR-0002) instantiated over its Éléments
requis. Gates downstream steps: a Projet is "ready to submit" only when its outstanding
Éléments requis clear.
_Avoid_: progress, completion percentage (as a stored value), advancement.

### Follow-up

**Relance**:
An action taken to close a gap on an Élément requis. Its *target* depends on the element's
state: a `manquant` **Livrable** chases its **source** if one is set, else its **judge**
(ADR-0008); a **Ressource** nudges the operator; and an `à corriger` **Livrable** *also*
nudges the operator — to fix the element before resubmission — **not** the judge, who is
not re-contacted until the corrected Livrable is resubmitted. Computed as *due* at invocation time from the element's deadline
and a graduated cadence, then drafted and gated by a Dry-run before it is sent (ADR-0006) —
never fired unattended by a background scheduler in v1. On an `à corriger` Livrable it
carries the rejection motif, which colors the nudge and is cited at resubmission. Recorded
in a per-element history so what was already asked is visible before asking again.
_Avoid_: reminder, notification, nudge, follow-up (English).

### Storage

**Manifeste**:
One flat Markdown file per Projet, living in that Projet's Classement, that is the
**single source of truth** for its state: a frontmatter header (objectif, deadline) plus
a table of Éléments requis (`Élément | Type | Statut | Deadline`) and a per-element
Relance log. `à corriger` and the Relance history cannot be inferred from a file's mere
presence, so they must be stored here explicitly (ADR-0004). Any visualization — Notion,
an HTML artifact, a rendered table — is a one-way downstream projection, interchangeable
and optional, never the master.
_Avoid_: database, source of truth (English), state file.

**Classement**:
The file/folder tree that stores a Projet's Éléments requis (its Livrables and
Ressources) and its Manifeste. The on-disk home of a Projet, distinct from the computed
Suivi de complétude that reports on it.
_Avoid_: gestion de fichier, arborescence, file structure.

### Method

**Definition-of-Done checklist**:
The concrete, checkable criteria written *before* the agent acts. Any unchecked item is
the "red" state; "green" is every item satisfied after execution. Replaces the test
runner that engineering skills loop on (ADR-0002).
_Avoid_: acceptance criteria, spec, done list.

**Dry-run**:
The agent's proposed plan of changes, presented against the Definition-of-Done checklist
for human confirmation *before* it executes. The checklist is what gives the human an
objective standard to judge the preview against. Embedded in every acting skill, not a
standalone skill.
_Avoid_: preview, diff, plan (unqualified).

## The bank's skills

A single **on-ramp** (model-invoked, so the agent can reach it when it detects a project
to drive) grills the intent and routes on one question — *does this require assembling a
set of Éléments requis over time?* (ADR-0005):

- **Yes → Projet path**: `cadrage` fixes the objectif, enumerates the Éléments requis, and
  writes the Definition-of-Done (the "done" *is* that required set satisfied), creating the
  Manifeste + Classement. Then the concurrent loop runs: `traiter` (advance one Élément
  requis, with the DoD + Dry-run embedded) and `suivi` (compute Suivi de complétude over
  all elements and propose the due Relances).
- **No → one-off path**: Definition-of-Done → Dry-run → execute, no Manifeste. `classement`
  (filing an existing stock of files into the tree) is its own named skill; other one-offs
  run inline in the on-ramp.

`cadrage`, `traiter`, `suivi`, and `classement` are user-invoked (zero context load; the
on-ramp tells the operator which to type). There is **no `to-issues` equivalent** — the
Éléments requis *are* the issues.

If the true shape of the work only reveals itself once one of these paths is already
underway — an inline one-off that turns out to need accretion over time, or a stock handed
to `classement` that turns out to be a Projet's own Éléments requis — on-ramp stops and
redirects to `cadrage` rather than continuing on the wrong path; nothing is lost, since the
Dry-run gate means nothing has touched disk yet (ADR-0010). `classement` may also run
**inside** an already-`cadrage`'d Projet, to reorganize Éléments requis already `obtenu`
(rename, relocate) — but it never writes to, reads, or cross-references that Projet's
Manifeste; Statut lives only there, and `classement`'s job is physical placement, never state
(ADR-0011).
