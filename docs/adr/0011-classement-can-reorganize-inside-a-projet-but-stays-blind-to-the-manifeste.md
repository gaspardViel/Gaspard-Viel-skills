# classement can reorganize files inside an existing Projet, but stays blind to every Manifeste

`classement` was scoped as an out-of-Projet, one-off filing job (ADR-0005): if the files are a
Projet's own Éléments requis, use `cadrage` instead. But an operator mid-Projet sometimes just
wants to reorganize what's already `obtenu` — rename files, move them under a cleaner tree —
without changing anything about their Statut. And separately: could a stock being filed
happen to match some Projet's currently-`manquant` Élément requis, and should `classement`
notice?

**Can it run inside a Projet's Classement?** We considered keeping `classement` strictly
out-of-Projet and leaving in-Projet reorganization as an unnamed, ad-hoc action. Rejected: the
procedure — inventory, target tree, naming scheme, Dry-run, bulk move (`classement` steps
1–4) — is identical whether the files sit inside a Projet's Classement or not. Branching on
*where the files live* rather than *what the operation is* violates the same principle
ADR-0001/ADR-0005 already settled (branch on task shape, not subject matter).

Decision: `classement` may run inside an existing Projet's Classement to reorganize
already-filed Éléments requis (rename, relocate), as long as it never writes to that Projet's
Manifeste. Statut lives only in the Manifeste (ADR-0004); `classement`'s job is physical
placement, never state — an element's Statut cannot change as a side effect of being moved or
renamed.

**Should it cross-reference Manifestes?** We considered having `classement` flag, at the end
of a run, that a filed file resembles some Projet's `manquant` Élément requis. Rejected: that
requires `classement` to know about every open Projet's Manifeste, which is a matching job
outside a "survey a stock, file it, done" skill's contract — and it blurs the line with what
`/suivi` already owns (tracking completeness against the Manifeste).

Decision: `classement` never reads, writes, or cross-references any Manifeste, including the
one belonging to the Projet it may be reorganizing inside of. It has no opinion on whether a
file corresponds to some Élément requis; noticing that and acting on it (via `/traiter`) is
the operator's call, not `classement`'s.
