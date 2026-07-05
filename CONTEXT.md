# Non-Dev Productivity Skill Bank

A sibling skill bank for non-developer productivity work: helping a single person or
household drive a personal administrative or business project (a VAE file, an aid
application, a business registration) to completion by tracking the required elements
and chasing the missing ones. The vocabulary below is the project's ubiquitous
language — prefer these terms (French, as the real-world domain uses them) over their
English or platform-specific synonyms.

## Language

### The unit of work

**Projet**:
One person's or household's undertaking with a goal to reach (a VAE validation, an aid
obtained, a business created), requiring a defined set of Éléments requis to be assembled
before it can complete. Single-actor: a jury or administration is an external target of a
Relance, never a participant we model states for.
_Avoid_: dossier, solution métier, case, task, project (English).

### Required elements

**Élément requis**:
One item the Projet needs before it can be considered complete. Carries an explicit
status of its own; completeness is never inferred from mere file existence. Has two
subtypes below.
_Avoid_: requirement, item, pièce (that is one subtype, not the general term).

**Pièce justificative**:
A subtype of Élément requis that is *submitted to a third party* (a jury, an
administration). Because it is judged externally, it can be rejected and sent back —
so it alone can carry the `à corriger` status.
_Avoid_: document, attachment, justificatif.

**Ressource de cadrage**:
A subtype of Élément requis that is *only consulted by the user* to frame or guide the
work (guidance, a template, reference material) — never submitted, never judged by a
third party, so it is never `à corriger`.
_Avoid_: resource, reference, framing document.

### Status

**Statut** (of an Élément requis):
Where a single Élément requis stands. The flat set is **obtenu** (in hand and valid),
**manquant** (not yet obtained), and — for a Pièce justificative only — **à corriger**
(submitted but sent back as wrong/incomplete). We deliberately keep this flat, not a
branched submitter→reviewer→approver machine.
_Avoid_: present/absent, complete/incomplete, en construction/en instruction (those are a platform's states, not ours).

**Suivi de complétude**:
The Projet's completeness, always *computed live* as an aggregate over each Élément
requis's Statut — never stored as an independent number that could drift. For a Projet,
this *is* the Definition-of-Done checklist (ADR-0002) instantiated over its Éléments
requis. Gates downstream steps: a Projet is "ready to submit" only when its outstanding
Éléments requis clear.
_Avoid_: progress, completion percentage (as a stored value), advancement.

### Follow-up

**Relance**:
An action taken to close a gap on an Élément requis — either chasing a third party (for
a Pièce justificative) or nudging the user (for a Ressource de cadrage). Recorded in a
per-Projet history so what was already asked is visible before asking again. Automatic,
deadline-scheduled Relances are reserved for Éléments requis pinned to a real date;
open-ended gaps stay user/agent-triggered.
_Avoid_: reminder, notification, nudge, follow-up (English).

### Storage

**Classement**:
The file/folder tree that stores a Projet's Éléments requis (its Pièces justificatives
and Ressources de cadrage). The on-disk home of a Projet, distinct from the computed
Suivi de complétude that reports on it.
_Avoid_: gestion de fichier, arborescence, file structure.

### Method

**Definition-of-Done checklist**:
The concrete, checkable criteria written *before* the agent acts. Any unchecked item is
the "red" state; "green" is every item satisfied after execution. Replaces the test
runner that engineering skills loop on (see ADR-0002).
_Avoid_: acceptance criteria, spec, done list.

**Dry-run**:
The agent's proposed plan of changes, presented against the Definition-of-Done checklist
for human confirmation *before* it executes. The checklist is what gives the human an
objective standard to judge the preview against.
_Avoid_: preview, diff, plan (unqualified).
