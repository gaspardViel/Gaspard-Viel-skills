# Separate skill bank for non-dev productivity, instead of parameterizing the engineering skills

The engineering skills (`grill-with-docs`, `to-prd`, `to-issues`, `implement`, `tdd`, `code-review`...) assume a precise contract: git, an issue tracker, a test runner, a PR. We considered parameterizing those same skills to also cover non-dev work (file/folder management, business-process solutions with AI), but that would widen their interface and dilute the depth that makes them useful — a skill trying to serve two contracts ends up serving neither well.

Decision: build a separate, sibling skill bank for non-dev productivity work. It can share the underlying philosophy (relentless grilling before building, ADRs for hard-to-reverse decisions, a project glossary) but not the code or the interface of the engineering skills.
