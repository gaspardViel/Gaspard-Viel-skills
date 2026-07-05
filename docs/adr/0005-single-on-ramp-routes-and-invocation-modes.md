# A single on-ramp routes one-off vs Projet; on-ramp is model-invoked, the rest user-invoked

The bank needs an entry point, and its skills need invocation modes. Two decisions,
coupled, so recorded together.

**Routing.** We considered separate entry points per subject matter. Rejected, following
ADR-0001's principle of branching on task complexity, not on subject matter.

Decision: a single **on-ramp** grills the intent and routes on one question — *does this
require assembling a set of Éléments requis over time?*
- **Yes → Projet path**: `cadrage` (creates Manifeste + Classement), then the concurrent
  `traiter` / `suivi` loop (ADR-0003).
- **No → one-off path**: Definition-of-Done → Dry-run → execute, no Manifeste. `classement`
  (filing an existing stock of files into the tree) has a distinct leading word and its own
  procedure — target tree + naming scheme + bulk filing — so it stands alone as a named
  skill; other one-offs run inline in the on-ramp.

**Invocation** (writing-great-skills: pay context load for model-invocation only when the
agent or another skill must reach a skill on its own). The operator returns to an existing
Projet and wants to type `/suivi` or `/traiter` directly, and the Dry-run already puts a
human in the loop at every acting step, so the agent needs no autonomous chaining between
skills.

Decision: only the **on-ramp is model-invoked** — one context-load cost, discoverable so
"I'm building my X dossier" can trigger the bank. `cadrage`, `traiter`, `suivi`, and
`classement` are **user-invoked** (zero context load), typed by hand; the on-ramp names
which one to reach for next. There is **no `to-issues` equivalent** — the Éléments requis
*are* the issues.

(Skill names above are working names.)
