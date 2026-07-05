---
name: classement
description: Range un stock de fichiers existant dans l'arborescence du Classement selon un schéma de nommage, gated par un Dry-run.
disable-model-invocation: true
---

# Classement

Files an existing **stock of files** into a **Classement** tree. A one-off with its own procedure — target tree + naming scheme + bulk filing — which is why it stands alone rather than running inline in the on-ramp (ADR-0005). It does **not** open a **Manifeste**; if the files are Éléments requis of a Projet that accretes over time, use `/cadrage` instead.

## Steps

### 1. Survey the stock

Inventory the files to be filed — count, kinds, and any existing partial structure.

**Completion criterion:** every file in the stock is accounted for in the inventory.

### 2. Decide the target tree and naming scheme

Settle the **Classement** shape and a naming scheme that every file will follow.

**Completion criterion:** a target tree and a naming rule are written, and every surveyed file maps to a destination.

### 3. Dry-run the filing plan

Present the full plan — each file's source → destination and final name — against a **Definition-of-Done** ("every file placed, none left over, naming scheme applied") for the operator to confirm (ADR-0002).

**Completion criterion:** the operator has confirmed the Dry-run.

### 4. Bulk-file

On confirm, move every file to its destination under the naming scheme.

**Completion criterion:** every file from the survey sits at its planned destination, the naming scheme is applied uniformly, and no file is left unfiled.
