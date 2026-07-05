# The bank serves file-deliverable projects for a single operator, separate from the engineering skills

The engineering skills (`grill-with-docs`, `to-prd`, `to-issues`, `implement`, `tdd`,
`code-review`…) assume a precise contract: git, an issue tracker, a test runner, a PR.
Parameterizing them to also cover non-code work would widen their interface and dilute the
depth that makes them useful — a skill serving two contracts serves neither well.

We considered scoping this bank narrowly to personal/admin productivity (VAE files, aid
applications), and considered the opposite — unifying dev and non-dev under one bank.

Decision: a separate, sibling skill bank scoped to **any project whose deliverables are
files in a tree**, driven by a **single operator** — administrative dossiers, grant and
tender responses, manuscripts, research corpora, creative work. It excludes code that
needs a test runner / CI, because its verification loop is a Definition-of-Done checklist
plus a Dry-run (ADR-0002), not a test runner. It shares the underlying philosophy
(relentless grilling before building, ADRs for hard-to-reverse decisions, a project
glossary) but not the code or the interface of the engineering skills.
