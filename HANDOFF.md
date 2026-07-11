# Handoff — File-Deliverable Skill Bank

Last updated: 2026-07-11. Pick up the design of the non-dev, file-deliverable skill bank.

## Where we are

The bank's **domain model is settled** ([CONTEXT.md](CONTEXT.md) + [docs/adr/](docs/adr/) 0001–0009) and the **five skills are scaffolded** in `.claude/skills/`: `on-ramp`, `cadrage`, `traiter`, `suivi`, `classement`. Each has a description, steps, and completion criteria, using the fixed leading words (skill names + the CONTEXT.md glossary).

The last working session **grilled the `suivi` Relance cadence** and pushed the decisions all the way through:

- [ADR-0006](docs/adr/0006-relances-computed-at-invocation-not-scheduled.md) rewritten with the full tick semantics (idempotent via the Manifeste log; missed ticks collapse to the most recent; cadence lives in the Manifeste header, overridable per-Livrable; `à corriger` restarts the cadence and nudges the operator; v2 = léger scheduled nudge to the operator, outward send always Dry-run gated).
- [CONTEXT.md](CONTEXT.md) **Relance** definition corrected — target depends on state (`manquant` Livrable → third party; Ressource / `à corriger` → operator).
- [suivi](.claude/skills/suivi/SKILL.md) steps 3–5 and [cadrage](.claude/skills/cadrage/SKILL.md) step 5 filled to match.

This session **grilled and closed open item 1** — the `à corriger` transition:

- [ADR-0007](docs/adr/0007-suivi-records-the-a-corriger-transition.md): `obtenu` on a Livrable is optimistic (submitted, not a third-party verdict); `suivi` (not `traiter`) records the rejection at the start of its run, ungated (it transcribes a fact, sends nothing outward); the rejection is its own timestamped Relance-log entry, marking the fresh-cycle boundary ADR-0006 needs; the resubmission deadline is mandatory (operator sets a target if the third party gave none).
- [CONTEXT.md](CONTEXT.md) **Statut** definition sharpened — `obtenu` means something different per subtype (Ressource: in hand and valid; Livrable: submitted and presumed fine until contradicted).
- [suivi](.claude/skills/suivi/SKILL.md) gained a new step 2 ("Record any reported rejections"); steps renumbered 3–6 accordingly, with cross-references fixed.
- [traiter](.claude/skills/traiter/SKILL.md) step 2 now points at where the rejection motif lives (the Relance log entry `suivi` writes).

This session **grilled and closed open item 1** — `cadrage` and `traiter`, to the same depth `suivi` got:

- [ADR-0008](docs/adr/0008-livrable-judge-and-source-are-separate-fields.md): a Livrable's **judge** (who receives and can reject it) and its **source** (whoever the operator is waiting on to supply it, when that's not the operator) are separate — a `manquant` Livrable is relanced at its source if one is set, else its judge; `à corriger` resubmission always targets the judge.
- [ADR-0009](docs/adr/0009-traiter-batches-ressources-not-livrables.md): `traiter` keeps its strict one-Livrable-per-run, Dry-run-gated discipline for Livrables, but allows batching several Ressources per run with only an informal confirmation — a Ressource is never judged, `à corriger`, or sent anywhere, so the Dry-run's reason for existing (ADR-0002) doesn't apply.
- Two scope boundaries recorded **without an ADR** (clarifications, not trade-offs): a **terminal** rejection (no possible resubmission) is out of `suivi`/`traiter`'s hands — it invalidates the Projet's plan, so it sends the operator back to `cadrage`, not into a new Statut ([CONTEXT.md](CONTEXT.md) **Livrable**, [cadrage](.claude/skills/cadrage/SKILL.md) step 2); and `traiter` deliberately does **not** track dependencies between Éléments requis — the operator is expected to know their own human-sized dossier ([traiter](.claude/skills/traiter/SKILL.md) intro).
- [CONTEXT.md](CONTEXT.md) **Livrable** and **Relance** definitions updated for judge/source; [cadrage](.claude/skills/cadrage/SKILL.md) steps 2 and 5 capture judge/source per Livrable; [traiter](.claude/skills/traiter/SKILL.md) intro and steps 1–5 updated for Livrable-vs-Ressource batching; [suivi](.claude/skills/suivi/SKILL.md) step 5 relances the source (not always "the third party").

Don't re-derive any of the above — read the ADRs and the glossary; they are the source of truth.

## Open items (in priority order)

1. **`on-ramp` and `classement` are still thin scaffolds** — steps exist, bodies not deepened.
2. **No concrete Manifeste template yet.** The exact frontmatter fields, table format, cadence syntax, per-Livrable override field, judge/source columns (ADR-0008), and Relance log format are described in prose but not pinned to a real Markdown example. Worth pinning once, since `cadrage` writes it and `suivi`/`traiter` read it — including how the step-2 rejection log entry (ADR-0007) actually looks in Markdown.
3. **Default cadence `T-14 / T-7 / T-2` is provisional** — confirm the shipped numbers.
4. **v2 (not v1):** léger scheduled nudge to the operator via the `schedule` skill. Design already traced in ADR-0006; do not build in v1.

## Suggested skills for the next session

- `/grilling` — for open item 1 (grill `on-ramp`/`classement` before deepening bodies), or to pin down open item 2's concrete Manifeste template.
- `/domain-modeling` — to record any decision that comes out of grilling into an ADR or CONTEXT.md **before** editing skill bodies (keep the ADR the source of truth, not the scaffold).
- Reference `writing-great-skills` (`.claude/skills/writing-great-skills/SKILL.md`) while filling bodies — leading words, completion criteria, progressive disclosure.

## Notes / caveats

- **Placement:** the five bank skills live in `.claude/skills/` alongside the installed Matt-Pocock dev skills. If a separate source location is wanted, they can move.
- **Language:** English prose with French domain terms in bold, matching CONTEXT.md. Descriptions of user-invoked skills are in French.
- **Invocation modes** (ADR-0005): `on-ramp` is model-invoked (has triggers); the other four are `disable-model-invocation: true`, user-typed.
