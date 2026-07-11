---
name: cadrage
description: Cadre un nouveau Projet — fixe l'objectif, énumère les Éléments requis, écrit la Definition-of-Done, et crée le Manifeste + Classement.
disable-model-invocation: true
---

# Cadrage

Opens a **Projet**: fixes what "done" is, enumerates the **Éléments requis** that make it done, and lays down the **Manifeste** + **Classement** that hold its state (ADR-0003, ADR-0004). Run once per Projet, at the start. After it, the operator loops on `/traiter` and `/suivi`.

## Steps

### 1. Fix the objectif and the deadline

State the goal in one line and the Projet's end (a real external deadline if a **Livrable** has one, else the operator's target). These become the **Manifeste** frontmatter.

**Completion criterion:** objectif and deadline written, both concrete enough to check against later.

### 2. Enumerate the Éléments requis

List every item the Projet needs before it can complete. Classify each by the one asymmetry that splits the subtypes: *is it judged by a third party, or only consulted by the operator?*

- **Livrable** — submitted to a jury / administration / client / publisher. Can be sent back (`à corriger`); carries its own external deadline. Name its **judge** — who receives and can reject it, always present. If someone other than the operator has to *supply* it (a recommender writing a letter, an office issuing a certificate), also name that **source** — it's who gets chased while the element is `manquant`; the judge is who `à corriger` resubmission always goes back to (ADR-0008). Most Livrables have no separate source — the operator is producing it themselves — in which case there's nothing extra to capture.
- **Ressource** — only consulted to frame the work. Never `à corriger`, never its own deadline.

Every element starts at **Statut** `manquant`.

A Livrable's judge is assumed capable of a *correctable* rejection (`à corriger`, resubmit to the same judge). If a Livrable instead has only one shot — a terminal "no" with no second attempt — that's not a Statut this bank tracks: a terminal rejection means re-running `cadrage` to drop the Livrable or open a new one against a different judge, not something `suivi`/`traiter` handle.

**Completion criterion:** every Élément requis is listed and tagged **Livrable** or **Ressource** — no item left untyped; every Livrable has a named judge, and a source where one is distinct from the operator.

### 3. Write the Definition-of-Done

The **Definition-of-Done** *is* the required set satisfied: one checkable line per Élément requis reaching `obtenu`. This is the checklist every later Dry-run judges against (ADR-0002).

**Completion criterion:** one checkable criterion exists for each Élément requis.

### 4. Dry-run the Classement + Manifeste

Present the proposed folder tree (**Classement**) and the **Manifeste** file against the Definition-of-Done, for the operator to confirm before anything is written.

**Completion criterion:** the operator has confirmed the Dry-run.

### 5. Create the Classement and the Manifeste

On confirm, create the **Classement** tree and write the **Manifeste** — one flat Markdown file, the single source of truth:

- frontmatter: `objectif`, `deadline`, and the **default Relance cadence** — the graduated ticks every Livrable inherits (default `T-14 / T-7 / T-2` before its deadline), so `/suivi` reads the policy from here (ADR-0006).
- a table of Éléments requis: `Élément | Type | Statut | Deadline | Judge | Source`, with an **optional cadence override** on a Livrable's row when it needs a wider window than the default. `Source` is left blank unless it's someone other than the operator and different from `Judge` (ADR-0008); Ressource rows leave `Deadline`, `Judge`, and `Source` blank.
- a per-element **Relance** log (empty for now).

`à corriger`, the Relance log, and the cadence can't be inferred from a file's presence — they live here explicitly (ADR-0004).

**Completion criterion:** the Manifeste exists in the Classement with a default cadence in its header, every Élément requis is a row with Type + `manquant` Statut (every Livrable's row also names its Judge), and the Classement tree is on disk. Point the operator at `/traiter` and `/suivi`.
