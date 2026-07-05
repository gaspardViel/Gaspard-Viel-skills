# Handoff — File-Deliverable Skill Bank

Last updated: 2026-07-05. Pick up the design of the non-dev, file-deliverable skill bank.

## Where we are

The bank's **domain model is settled** ([CONTEXT.md](CONTEXT.md) + [docs/adr/](docs/adr/) 0001–0006) and the **five skills are scaffolded** in `.claude/skills/`: `on-ramp`, `cadrage`, `traiter`, `suivi`, `classement`. Each has a description, steps, and completion criteria, using the fixed leading words (skill names + the CONTEXT.md glossary).

The last working session **grilled the `suivi` Relance cadence** and pushed the decisions all the way through:

- [ADR-0006](docs/adr/0006-relances-computed-at-invocation-not-scheduled.md) rewritten with the full tick semantics (idempotent via the Manifeste log; missed ticks collapse to the most recent; cadence lives in the Manifeste header, overridable per-Livrable; `à corriger` restarts the cadence and nudges the operator; v2 = léger scheduled nudge to the operator, outward send always Dry-run gated).
- [CONTEXT.md](CONTEXT.md) **Relance** definition corrected — target depends on state (`manquant` Livrable → third party; Ressource / `à corriger` → operator).
- [suivi](.claude/skills/suivi/SKILL.md) steps 3–5 and [cadrage](.claude/skills/cadrage/SKILL.md) step 5 filled to match.

Don't re-derive any of the above — read the ADR and the glossary; they are the source of truth.

## Open items (in priority order)

1. **`à corriger` transition is uncovered.** `suivi` knows how to *read* an `à corriger` Livrable and restart its cadence, but nothing specifies *how* a Livrable becomes `à corriger` or *who* records the resubmission deadline. Likely belongs in [traiter](.claude/skills/traiter/SKILL.md) or a Manifeste edit. Grill this next.
2. **`cadrage` and `traiter` not yet grilled.** Same depth pass `suivi` got. For `cadrage`: does the Livrable/Ressource split hold on real dossiers? For `traiter`: is "one element per run" right?
3. **`on-ramp` and `classement` are still thin scaffolds** — steps exist, bodies not deepened.
4. **No concrete Manifeste template yet.** The exact frontmatter fields, table format, cadence syntax, per-Livrable override field, and Relance log format are described in prose but not pinned to a real Markdown example. Worth pinning once, since `cadrage` writes it and `suivi`/`traiter` read it.
5. **Default cadence `T-14 / T-7 / T-2` is provisional** — confirm the shipped numbers.
6. **v2 (not v1):** léger scheduled nudge to the operator via the `schedule` skill. Design already traced in ADR-0006; do not build in v1.

## Suggested skills for the next session

- `/grilling` — for open items 1–2 (grill `traiter`/`cadrage` before writing bodies).
- `/domain-modeling` — to record any decision that comes out of grilling into an ADR or CONTEXT.md **before** editing skill bodies (keep the ADR the source of truth, not the scaffold).
- Reference `writing-great-skills` (`.claude/skills/writing-great-skills/SKILL.md`) while filling bodies — leading words, completion criteria, progressive disclosure.

## Notes / caveats

- **Placement:** the five bank skills live in `.claude/skills/` alongside the installed Matt-Pocock dev skills. If a separate source location is wanted, they can move.
- **Language:** English prose with French domain terms in bold, matching CONTEXT.md. Descriptions of user-invoked skills are in French.
- **Invocation modes** (ADR-0005): `on-ramp` is model-invoked (has triggers); the other four are `disable-model-invocation: true`, user-typed.
