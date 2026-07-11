---
name: traiter
description: Fais avancer un Élément requis d'un cran — Definition-of-Done + Dry-run embarqués — et mets à jour son Statut dans le Manifeste.
disable-model-invocation: true
---

# Traiter

Advances Éléments requis to `obtenu`. The acting half of the loop; `/suivi` is the reporting half. Runs against the **Manifeste** written by `/cadrage`.

Handle one **Livrable** per run — its completion criterion is judged by a real third party, so batching hides premature completion. A **Ressource** carries none of that risk (never judged, never `à corriger`, never sent anywhere), so several may be advanced together in one run (ADR-0009).

`traiter` does not check dependencies between Éléments requis (e.g. one Livrable needing another already `obtenu`) — the operator, working a human-sized dossier, is expected to know that themselves. Deliberately out of scope, not an oversight.

## Steps

### 1. Pick the Élément requis

Read the **Manifeste**. Choose either one **Livrable** that is `manquant` or `à corriger`, or one or more **Ressources** that are `manquant`, to advance this run.

**Completion criterion:** exactly one Livrable, or one or more Ressources, is selected, and each selected element's current **Statut** is known.

### 2. Write its Definition-of-Done

Write or refresh the checkable criteria for each selected element reaching `obtenu`. For an `à corriger` **Livrable**, fold in the rejection motif that `/suivi` recorded in the element's Relance log when the rejection came in (ADR-0007) — what the judge sent it back for.

**Completion criterion:** a checklist exists per selected element that, when green, means it is `obtenu`.

### 3. Do the work into the Classement

Produce or obtain each selected element and place it in the **Classement** under the Projet's naming scheme.

**Completion criterion:** each element's file exists in the Classement.

### 4. Confirm against the Definition-of-Done

For a **Livrable**, present the proposed change against its Definition-of-Done for the operator to confirm before it is committed — a formal **Dry-run** (ADR-0002), since it's headed to a real third party. For a **Ressource**, an informal confirmation that the criteria are met is enough — no formal Dry-run, since nothing leaves the desk (ADR-0009).

**Completion criterion:** the operator has confirmed each selected element (Dry-run for a Livrable, informal confirmation for a Ressource).

### 5. Update the Statut in the Manifeste

On confirm, set each selected element's **Statut** in the Manifeste: `manquant → obtenu`, or `à corriger → obtenu`. Do **not** touch any completeness total — **Suivi de complétude** is computed live by `/suivi`, never stored (ADR-0004).

**Completion criterion:** each selected element's row in the Manifeste reads `obtenu` and its Definition-of-Done is green.
