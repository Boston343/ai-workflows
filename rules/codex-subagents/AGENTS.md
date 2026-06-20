## Reviewer Agents

Reviewer agents are timeboxed. A partial review with clear limits is useful; a silent timeout is
not.

When spawning a reviewer, provide a review packet instead of asking it to rediscover the repo:

- task goal and explicit review focus
- changed file list and `git diff --stat`
- relevant `git diff` hunks or branch/commit range
- commands already run and their results
- known risk areas and acceptance criteria
- relevant docs already identified by the implementer

Use focused reviewer scopes such as behavior/API, tests, and accessibility. Avoid broad prompts like "review this whole working tree" unless the change is tiny.

Reviewer prompts must include this budget contract:

```text
You are a timeboxed reviewer. You have 10 minutes total.

Spend at most 90 seconds orienting.
Use at most 12 search/read tool calls.
Read at most 4 local pattern files outside the diff.
Do not run installs, full builds, broad test suites, generation, or smoke checks unless explicitly asked.
Do not edit files.

Stop investigating by minute 8 and write the review.
If incomplete, return partial findings plus unchecked risk areas.
```

Required reviewer output:

```text
Verdict: Block / Follow-up / Looks reasonable in scoped review

Findings:
- Max 5, ordered by severity.
- Each finding must include file path, line or symbol, concrete risk, and suggested acceptance check.

Missing Tests:
- Max 3, only tests that would catch realistic regressions.

Unchecked Risks:
- Areas not reviewed because of the time/tool budget.
```

If the reviewer finds no issues, it must still return the verdict and unchecked risks. Do not
discard a reviewer response merely because it is incomplete; discard only responses that ignore the
output contract.
