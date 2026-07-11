# on-ramp re-routes whenever the true shape of the work reveals itself, in either direction

on-ramp asks its one routing question early (ADR-0005), but the honest answer isn't always
visible on the first pass. An "inline one-off" can turn out, once the grill goes deeper, to
need a set of Éléments requis assembled over time — it should have gone to `cadrage`. Just as
often, a "stock of files to file" turns out to *be* the Éléments requis of an ongoing Projet,
not a bare filing job — it should have gone to `cadrage` too, not `classement`.

We considered letting on-ramp finish whatever it already started inline (or already handed to
`classement`) and only pointing at `cadrage` for what comes *next*. Rejected: that lets a real
Projet get partway driven with no Manifeste — exactly the ungoverned state ADR-0003 and
ADR-0004 exist to prevent. A Projet without a Manifeste has no source of truth for its
Éléments requis, no matter how it got started.

We also considered adding no rule at all, on the grounds that on-ramp's one question, asked
well, should rarely be wrong. Rejected: the fix costs one sentence to write down, and the
Dry-run gate (ADR-0002) already guarantees nothing has touched disk before confirmation — so
there is no real cost to catching the misroute, only to not writing the rule.

Decision: whenever the true shape of the work reveals itself mid-grill — in either
direction — on-ramp stops and redirects to `/cadrage` rather than continuing on the wrong
path. This holds symmetrically: an inline one-off that turns out to need accretion over time
redirects to `cadrage`, and a "stock to file" that turns out to be a Projet's own Éléments
requis redirects to `cadrage` instead of `classement`. Nothing is lost either way — the
Dry-run gate means nothing has been written yet, so redirecting is free.
