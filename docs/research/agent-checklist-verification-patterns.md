# Research: does the evidence support the ADR-0002 verification loop?

This document validates (or challenges) the decision recorded in
[`docs/adr/0002-definition-of-done-plus-dry-run-replaces-tdd.md`](../adr/0002-definition-of-done-plus-dry-run-replaces-tdd.md):
a three-part loop of (1) write a checklist before acting, (2) propose a plan against
that checklist and get human confirmation before executing, (3) verify every checklist
item afterward. It does not re-derive the ADR's argument; it checks each piece against
primary sources — official Anthropic/OpenAI agent-building guidance, Claude Code's own
documented permission model, and peer-reviewed/preprint papers on agent planning and
verification.

Note on method: some canonical hosts (arxiv.org, openai.com, most of anthropic.com/claude.com)
were unreachable through this session's fetch tooling, which is scoped to a small set of
allowed hosts. Where a page couldn't be fetched directly, findings below rely on search-engine
snippets of the primary page/paper itself (not third-party paraphrase sites), and every claim
is still attributed to the primary URL/arXiv ID so it can be checked directly.

## 1. Is "plan → human confirmation → post-hoc checklist verification" an established, named single pattern?

The pieces are well established individually; the specific three-part combination is not
found as one named pattern anywhere in the primary literature.

- **Plan-then-execute is a named, established pattern**, distinct from reactive loops like ReAct.
  [Architecting Resilient LLM Agents: A Guide to Secure Plan-then-Execute Implementations](https://arxiv.org/abs/2509.08646)
  (Del Rosario, Krawiecka, Schroeder de Witt, 2025) defines it explicitly: an LLM first
  produces a complete multi-step plan, then a separate executor carries it out step by step —
  contrasted with ReAct's per-step reasoning/acting/observing interleaving.
- **Human approval of the plan before execution is documented empirically**, not just proposed.
  [Plan-Then-Execute: An Empirical Study of User Trust and Team Performance When Using LLM Agents As a Daily Assistant](https://arxiv.org/abs/2502.01390)
  (He, Demartini, Gadiraju, CHI 2025) studies exactly this: the agent drafts a plan, the user
  edits/approves it, then a step-wise execution follows — with human involvement in execution
  scaling with task risk. This is the closest primary-source match to ADR-0002's "dry-run"
  step, and it is framed around trust and risk, not around checklist-grounded criteria.
- **Plan-and-Solve is a related but different pattern**: [Plan-and-Solve Prompting](https://arxiv.org/abs/2305.04091)
  (Wang et al., ACL 2023) uses "devise a plan, then carry it out" purely as a prompting
  technique to reduce missing-step reasoning errors — no human confirmation gate, no
  post-hoc checklist. It establishes "plan-first" as a reasoning-quality lever, not a
  human-oversight lever.
- **Checklist-style, pre-committed criteria for verifying agent work exist as a named idea**,
  most directly in Anthropic's own engineering writeup on long-running coding agents:
  [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
  describes a "Default-FAIL contract" where every completion criterion starts false and can't
  flip to passing without the agent opening evidence for it — i.e., a checklist fixed in
  advance, checked post-hoc, with a bias toward "not done" by default. This is architecturally
  very close to ADR-0002's own model, but it is a build-verification harness for coding agents,
  not a plan-then-human-confirm workflow — the pre-execution human confirmation step is absent.
- **Conclusion**: pre-committed criteria (Anthropic harness work), pre-execution human
  confirmation of a plan (the CHI 2025 study, the plan-then-execute architecture paper), and
  post-hoc checklist grading all appear as separate, independently attested pieces in primary
  sources. No single primary source combines all three the way ADR-0002 does.

## 2. Failure modes of judging success only after acting, with no criteria fixed in advance

There is direct primary evidence that acting-then-judging (without pre-fixed criteria) is
worse than the alternative, from Anthropic's own operational experience, not just theory.

- [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)
  reports a concrete, named failure mode: "models reliably skew positive when they grade their
  own work. Asked 'are you done?' they answer 'yes' more often than they should," producing
  agents that "ship at 30% complete with full confidence." Anthropic's fix was structurally
  identical to ADR-0002's fix: a separate "fresh-context evaluator" agent that never saw the
  build, scored against pre-set criteria, replacing self-judgment made after the fact.
- [Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366)
  (Shinn et al., NeurIPS 2023) evaluates task success three ways: binary environment feedback,
  hand-written heuristics, and LLM self-evaluation — and treats all three as evaluator
  *choices* whose reliability varies. Its own limitations note that self-evaluation and
  heuristic evaluators can produce variable-quality feedback, and that the iterative
  self-reflection loop has **no formal convergence guarantee** toward correct outcomes when
  the evaluator itself is fallible. This is evidence, from the paper that popularized purely
  verbal/post-hoc self-critique, that the technique's benefit is conditional on evaluator
  quality — it is not a free substitute for externally fixed, checkable criteria.
- Neither Reflexion nor the ReAct-based baselines it builds on gate on human-approved,
  pre-committed criteria — Reflexion's self-reflection happens entirely after an attempt,
  compared against the agent's own or a heuristic judgment of the outcome. The Anthropic
  harness post is the stronger and more directly applicable evidence here because it names
  the specific failure (hindsight-biased self-grading) as something they hit in production
  and had to engineer around, rather than a hypothetical.

## 3. When primary sources say human confirmation gates belong, and how Claude Code implements this

- **Anthropic's own agent-building guidance ties gate placement to risk and reversibility**,
  not to task type. Per Anthropic's engineering blog: "build checkpoints where agents pause for
  human review… particularly important before they carry out irreversible actions, like
  approving financial transactions or deleting data" ([Building Effective Agents](https://www.anthropic.com/research/building-effective-agents),
  content confirmed via direct search of the page). The companion piece [Lessons from Anthropic
  on building effective human-agent teams](https://claude.com/blog/building-effective-human-agent-teams)
  describes graduating from "humans review every decision" to "agents surface only the
  decisions with hard tradeoffs" as trust is established — i.e., the gate is not fixed, it
  narrows over time.
- **Claude Code's documented permission model is concrete and operational**, at
  [Configure permissions](https://code.claude.com/docs/en/permissions): read-only tools
  (file reads, grep) require no approval; Bash commands and file modifications require
  approval by default ("ask"), with allow/ask/deny rules evaluated in that precedence order
  and a documented fail-closed default ("unmatched commands default to requiring manual
  approval" is the deny-first, ask-next, allow-last ordering the docs specify). Even in the
  most permissive `bypassPermissions` mode, root/home-directory deletions "still prompt as a
  circuit breaker against model error" — i.e., irreversibility is treated as a hard floor for
  human confirmation regardless of configured trust level.
- **OpenAI's guidance frames this the same way**: "[Human intervention] is a critical
  safeguard enabling you to improve an agent's real-world performance without compromising
  user experience," implemented as an explicit handoff mechanism when the agent can't or
  shouldn't proceed alone, with "guardrails critical at every stage, from input filtering and
  tool use to human-in-the-loop intervention" ([A practical guide to building agents](https://cdn.openai.com/business-guides-and-resources/a-practical-guide-to-building-agents.pdf), OpenAI, 2025).
- **Mapping to checklist-style verification**: none of these sources tie the human-confirmation
  gate specifically to a *checklist* — they tie it to risk/irreversibility. ADR-0002's move of
  attaching the gate to "confirm the plan matches a pre-written checklist" is a reasonable and
  compatible specialization (the checklist gives the human something concrete to check the
  plan against, addressing the "preview alone gives no objective standard" problem the ADR
  itself raises) but it is an extension beyond what these sources describe, not something
  they independently prescribe.

## 4. Established patterns for tracking a checklist of required items and detecting gaps

This is the weakest-covered question. No primary source describes an agent pattern that
matches ADR-0002's actual use case — confirming that a fixed, known-in-advance set of
required items (e.g., all documents a VAE certification file needs) are present and valid —
closely enough to call it an established pattern.

- **Closest partial analogs**, each with a real mismatch:
  - *Checklist-grounded LLM-judge evaluation*: [RubricEval](https://arxiv.org/abs/2603.25133)
    and [From Rubrics to Reliable Scores (Rulers)](https://arxiv.org/abs/2601.08654) both use
    checklists as a scoring mechanism for judging free-text LLM *output quality*, not for
    tracking whether a required set of real-world artifacts exists. Notably, RubricEval finds
    rubric-level evaluation outperforms plain checklist-level evaluation, suggesting even
    within this literature "checklist" is treated as the weaker of two verification designs.
  - *Behavioral testing*: [CheckList: Beyond Accuracy](https://aclanthology.org/2020.acl-main.442/)
    (Ribeiro et al., ACL 2020, Best Paper) established "checklist" as a named methodology for
    systematically covering test cases against an NLP model — an important precedent for
    "checklist" as a rigorous verification vocabulary, but it verifies model behavior, not
    task/document completeness for an end-user deliverable.
  - *Requirements traceability / completeness tooling*: papers like
    [TVR](https://arxiv.org/abs/2504.15427) and [R2Code](https://arxiv.org/abs/2604.22432) use
    LLMs/RAG to detect missing or inconsistent links between requirements and
    code/specifications. This is the closest domain match conceptually (detecting gaps
    against a required set) but the required set there is software requirements, not a
    fixed real-world document/task list, and none of it is framed as a human-confirmed,
    pre-execution checklist.
  - *Slot-filling dialogue systems* fill a fixed, predefined set of slots per turn, which is
    structurally similar to "confirm N required items are present," but the literature here
    is about extracting values from conversational text, not verifying assembled real-world
    deliverables, and it doesn't involve a pre-execution human confirmation step at all.
- **Plain statement**: nothing in primary agent-design literature or documentation directly
  addresses "an agent that manages a fixed checklist of required real-world items/documents
  and confirms none are missing before declaring the task done." ADR-0002's checklist concept
  for this repo's domain (VAE files, aid applications, business registration) is a novel
  application of general checklist/plan-then-execute ideas to a domain none of the surveyed
  sources cover — this should be treated as our own extrapolation, not as literature-backed.

## Implications for our design

**Supported by primary sources, solidly:**
- Gating human confirmation on risk/irreversibility (Anthropic's agent guidance, OpenAI's
  guidance, Claude Code's own permission model) — directly on point for the "dry-run" half
  of ADR-0002.
- Fixing verification criteria in advance rather than judging success only after the fact —
  Anthropic's own long-running-agent harness work independently converged on the same fix for
  the same failure mode ADR-0002 is trying to avoid (self-graded, hindsight-biased "done").
- Plan-then-execute (plan first, execute second) as a distinct, named, studied pattern, with
  at least one CHI paper empirically linking human review of the plan to trust and outcomes.

**Partially supported / an extension beyond the literature:**
- Tying the pre-execution human-confirmation gate specifically to a pre-written checklist
  (rather than to risk level generically) is compatible with, but not directly prescribed by,
  any source found. It's a reasonable synthesis of two separately-attested ideas (checklist
  criteria + human confirmation gate), not a documented combination.

**Not supported / no coverage found — genuinely novel for our use case:**
- The specific application to tracking a fixed set of required real-world items (documents,
  administrative steps) has no close primary-source match. Slot-filling, requirements
  traceability, and checklist-grounded LLM evaluation are the nearest neighbors, and each
  differs in an important way (they verify text/model output or software artifacts, not
  human-facing administrative deliverables assembled by a non-technical user).

**Net verdict**: the evidence supports ADR-0002's two general moves (fix criteria before
acting; gate irreversible/consequential actions on human confirmation) as individually
well-established in primary agent-design sources, including a very close structural match in
Anthropic's own "Default-FAIL" harness work. It does not confirm the specific three-part
combination, or the application to non-software checklist domains, as an established pattern
— those are reasonable, literature-consistent extrapolations that this repo is making on its
own.
