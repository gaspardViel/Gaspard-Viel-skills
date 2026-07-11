---
name: classement
description: Range un stock de fichiers existant dans l'arborescence du Classement selon un schéma de nommage, gated par un Dry-run.
disable-model-invocation: true
---

# Classement

Files an existing **stock of files** into a **Classement** tree. A one-off with its own procedure — target tree + naming scheme + bulk filing — which is why it stands alone rather than running inline in the on-ramp (ADR-0005). It does **not** open a **Manifeste**; if the files are Éléments requis of a Projet that accretes over time, use `/cadrage` instead (and if that only becomes clear mid-run, stop and redirect there — ADR-0010).

`classement` can also run **inside** an already-`cadrage`'d Projet, to reorganize Éléments requis already `obtenu` (rename, relocate) — but it never touches that Projet's **Manifeste**. Its job is physical placement, never Statut; it does not read, write, or cross-reference any Manifeste, even to notice that a filed file looks like some Projet's outstanding Élément requis (ADR-0011). That noticing, if it happens, is the operator's call — pointing it out is `/suivi`'s and `/traiter`'s job, not this skill's.

## Steps

### 1. Survey the stock

Inventory the files to be filed — count, kinds, and any existing partial structure.

**Completion criterion:** every file in the stock is accounted for in the inventory.

### 2. Decide the target tree and naming scheme

Settle the **Classement** shape and a naming scheme that every file will follow.

**Completion criterion:** a target tree and a naming rule are written, and every surveyed file maps to a destination.

### 3. Dry-run the filing plan

Present the full plan — each file's source → destination and final name — against a **Definition-of-Done** ("every file placed, none left over, naming scheme applied, no name collides") for the operator to confirm (ADR-0002).

Before presenting, check the planned destinations for **naming collisions** — two different source files that would land on the same final name. A collision is a **blocking condition**: the Dry-run cannot be confirmed as-is, because confirming it would silently overwrite one file with another, which is exactly the silent-loss the Dry-run gate exists to prevent (ADR-0002). Surface every collision and resolve it (adjust the naming rule, add a disambiguating suffix, or merge) before asking for confirmation.

**Completion criterion:** the operator has confirmed the Dry-run, and the confirmed plan contains zero naming collisions.

### 4. Bulk-file

On confirm, move every file to its destination under the naming scheme.

**Completion criterion:** every file from the survey sits at its planned destination, the naming scheme is applied uniformly, and no file is left unfiled.
