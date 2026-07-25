---
description: Run the plan → implement → review loop for a feature.
---
We're building: $ARGUMENTS

1. Delegate to the architect subagent to write the plan with steps and
   acceptance criteria. Show me the plan and wait for my "go".
2. After I approve, delegate to the engineer subagent to implement it on a
   new branch with tests passing.
3. Delegate to the architect subagent to review the diff against the plan.
4. If there are blockers, send them back to the engineer and re-review.
   Otherwise summarize and stop so I can merge.