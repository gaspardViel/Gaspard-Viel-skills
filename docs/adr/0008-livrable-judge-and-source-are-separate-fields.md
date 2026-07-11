# A Livrable's judge and its source are separate — cadrage captures both

CONTEXT.md's Relance definition says a `manquant` Livrable "chases the third party,"
implicitly assuming one third party per Livrable. That assumption breaks for a Livrable
that is *supplied by* someone other than the one who will *judge* the assembled dossier —
a recommendation letter, say: the recommender must produce it, but the funder or jury
judges the final submission, not the letter in isolation. Chasing "the third party" is
ambiguous the moment these two are different people.

We considered a new Élément requis subtype for third-party-sourced items. Rejected: the
Livrable/Ressource split stays on the one asymmetry that matters for the Statut machine —
is it judged, can it be sent back `à corriger`? A recommendation letter is judged (it's
part of what the funder can reject the dossier over), so it's a Livrable like any other;
splitting further would multiply subtypes for a distinction that only affects *who gets
chased*, not the Statut lifecycle.

Decision: `cadrage`, when enumerating a Livrable, captures two potentially-distinct
targets — the **judge** (who receives and can reject the assembled submission; always
present) and, optionally, a **source** (whoever the operator is actually waiting on to
supply the element, when that's not the operator themselves). When a Livrable is
`manquant`, `suivi` relances the source if one is set, else the judge. The judge is who
`à corriger`'s resubmission (ADR-0007) always goes back to — a source, once it has
supplied its part, drops out of the loop.
