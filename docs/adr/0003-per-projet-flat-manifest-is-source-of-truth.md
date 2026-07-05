# A per-Projet flat Markdown manifest is the source of truth; views are pluggable projections

A Projet's state — each Élément requis's Statut, its deadline, and its Relance history —
must be stored explicitly: `à corriger` (a Pièce justificative submitted then rejected) and
the follow-up history cannot be inferred from a file's mere presence in the Classement.

We considered making an external database the master — Notion linked databases with
cross-views were the concrete example, and the completeness research (`docs/research/existing-completeness-tracking-tools.md`)
drew its "compute, don't store" pattern from Airtable/Notion rollups. Rejected as the master
because it splits truth between the on-disk documents and a SaaS, requires the connector to
function at all, and can drift out of sync with the files it describes.

Decision: one flat Markdown manifest per Projet lives in that Projet's Classement as the
single source of truth — a frontmatter header (objectif, deadline) plus a table of Éléments
requis (`Élément | Type | Statut | Deadline`) and a dedicated per-element Relance log
(each relance timestamped with its motif). `Suivi de complétude` is always computed live
from this manifest, never stored as an independent number. Any visualization — Notion, an
HTML artifact, a rendered table — is a one-way downstream projection, interchangeable and
optional, never the master. The goal a view serves is only "see the statuses easily"; the
target is not fixed.
