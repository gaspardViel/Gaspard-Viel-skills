# The bank's shape follows the three jointures, not the five-phase linear sequence

The lifecycle research (`docs/research/common-project-lifecycle-across-disciplines.md`)
was read for a "common route" across project-management corpora, case-management/checklist
science, and single-actor productivity systems. Its verdict is sharp: the five-phase
linear sequence (Initiating → Planning → Executing → Monitoring&Controlling → Closing) is
**one lineage's convention** (PMI, echoed by ISO 21502 and PRINCE2), **not** a universal
invariant — CMMN is built for activities performed in "unpredictable order," PMBOK 7
replaces the ordered phases with concurrent performance domains, and even in PMI's own
model Monitoring "occurs in parallel to Execution," not after it. What *is* independently
attested by every family is a set of **jointures**: (a) fix the required set in advance,
(b) track completeness as a *derived* value, (d) define "done." (Relance, (c), is
first-class nowhere and lives inside monitoring.)

We considered mirroring the five phases as one skill each (literal reading of "toutes les
étapes communes"). Rejected: it would build the very artifact the research dismantles, and
impose an order the research calls a convention.

Decision: the bank's skills follow the jointures. `cadrage` fixes the required set (a) and
the "done" criterion (d) together — the "done" *is* that set satisfied. `traiter` and
`suivi` run as a **concurrent loop**, not "phase 4 after phase 3": `suivi` computes
completeness live and surfaces gaps while elements are still being produced. Relance is
not a first-class skill; it lives inside `suivi` (ADR-0006), matching the research's
finding that gap-chasing is absorbed into monitoring.
