# `traiter` keeps one-Livrable-per-Dry-run but allows batching Ressources

`traiter` was scoped to one Élément requis per run so that "the deep completion criterion
is per-element, not per-Projet" — batching hides premature completion. That reasoning
holds for a Livrable, where getting it wrong means a real rejection from a real third
party. It over-applies to a Ressource: a Ressource is never judged, never `à corriger`,
never sent anywhere (CONTEXT.md) — the entire premise of the Dry-run gate, guarding a
consequential or irreversible action (ADR-0002), doesn't hold for "I read the funder's
guidelines" the way it holds for "I'm submitting this financial statement." Forcing three
separate Dry-run-gated runs to check off three trivial Ressources is friction with no
corresponding risk reduction — the same over-classification ADR-0007 already corrected
for recording a rejection in `suivi`.

Decision: `traiter` keeps its strict one-element-per-run, Dry-run-gated discipline for
Livrables. For Ressources, it may mark several `obtenu` in a single run, with only an
informal confirmation — no formal Dry-run — mirroring the ungated, fact-transcribing
write ADR-0007 introduced for rejections. The risk ADR-0002 exists to prevent (an
irreversible action going out unconfirmed) simply isn't present for a Ressource.
