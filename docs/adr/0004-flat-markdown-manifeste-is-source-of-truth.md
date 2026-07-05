# A per-Projet flat Markdown Manifeste is the source of truth; views are pluggable projections

A Projet's state — each Élément requis's Statut, its deadline, and its Relance history —
must be stored explicitly: `à corriger` (a Livrable submitted then rejected) and the
follow-up history cannot be inferred from a file's mere presence in the Classement. The
research's jointure (b) is equally firm the other way: completeness itself must **never**
be stored, only computed live (CMMN models a Milestone as "evaluation of progress" with
"no work directly associated" — a derived value, never a stored one).

We considered an external database as master — Notion linked databases with cross-views
were the concrete example. Rejected as master because it splits truth between the on-disk
documents and a SaaS, requires the connector to function at all, and can drift out of sync
with the files it describes.

Decision: one flat Markdown **Manifeste** per Projet lives in that Projet's Classement as
the single source of truth — a frontmatter header (objectif, deadline) plus a table of
Éléments requis (`Élément | Type | Statut | Deadline`) and a per-element Relance log (each
relance timestamped with its motif). `Suivi de complétude` is always computed live from
this Manifeste, never stored as an independent number. Any visualization — Notion, an HTML
artifact, a rendered table — is a one-way downstream projection, interchangeable and
optional, never the master.
