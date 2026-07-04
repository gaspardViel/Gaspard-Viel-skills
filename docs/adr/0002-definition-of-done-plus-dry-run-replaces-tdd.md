# Definition-of-done checklist + dry-run preview replace TDD as the verification loop

The engineering skills loop on a binary, automatable signal: a test goes red, then green. Non-dev productivity work (file/folder management, business-process solutions with AI) has no test runner to loop on, so the skills need a different definition of "done" to build toward.

We considered a dry-run/preview alone (propose the plan of changes, human confirms, then execute — similar to reviewing a diff before a commit), but a preview with no prior criteria gives the human nothing objective to judge it against.

Decision: pair the two. Before acting, write a concrete, checkable Definition-of-Done checklist (the "red" state is any unchecked item). During execution, the agent proposes its plan against that checklist for human confirmation before it runs (the dry-run). "Green" means every checklist item is satisfied after execution.
