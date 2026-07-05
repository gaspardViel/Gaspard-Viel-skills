---
name: traiter
description: Fais avancer un Élément requis d'un cran — Definition-of-Done + Dry-run embarqués — et mets à jour son Statut dans le Manifeste.
disable-model-invocation: true
---

# Traiter

Advances **one Élément requis** to `obtenu`. The acting half of the loop; `/suivi` is the reporting half. Runs against the **Manifeste** written by `/cadrage`.

Handle a single element per run — the deep completion criterion is per-element, not per-Projet, so batching hides premature completion.

## Steps

### 1. Pick the Élément requis

Read the **Manifeste**. Choose one element that is `manquant` or `à corriger` to advance this run.

**Completion criterion:** exactly one Élément requis is selected, and its current **Statut** is known.

### 2. Write its Definition-of-Done

Write or refresh the checkable criteria for *this* element reaching `obtenu`. For an `à corriger` **Livrable**, fold in what the third party sent it back for.

**Completion criterion:** a checklist exists that, when green, means this element is `obtenu`.

### 3. Do the work into the Classement

Produce or obtain the element and place it in the **Classement** under the Projet's naming scheme.

**Completion criterion:** the file exists in the Classement.

### 4. Dry-run against the Definition-of-Done

Present the proposed change against the element's Definition-of-Done for the operator to confirm before it is committed (ADR-0002).

**Completion criterion:** the operator has confirmed the Dry-run.

### 5. Update the Statut in the Manifeste

On confirm, set the element's **Statut** in the Manifeste: `manquant → obtenu`, or `à corriger → obtenu`. Do **not** touch any completeness total — **Suivi de complétude** is computed live by `/suivi`, never stored (ADR-0004).

**Completion criterion:** the element's row in the Manifeste reads `obtenu` and its Definition-of-Done is green.
