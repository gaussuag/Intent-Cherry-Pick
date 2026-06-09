# Intent Cherry-Pick

`intent-cherry-pick` is a Codex skill for source-fidelity commit porting.

It is intended for cases where a normal `git cherry-pick` is too mechanical: branches may have diverged, conflicts may need human-style resolution, and the correct result must preserve both the source commit facts and the target branch facts.

The skill does not redesign the change. It ports the source commit as faithfully as possible and uses intent only to resolve conflicts.

## What It Does

- Reads the source commit or commit range and target branch code.
- Extracts the source commit intent and implementation facts.
- Identifies target branch facts that must remain protected.
- Classifies each change by fidelity level:
  - `L0`: exact apply, automatic
  - `L1`: context-adjusted apply, automatic
  - `L2`: conflict fusion, requires human confirmation
  - `L3`: semantic substitute, requires human confirmation
- Runs a precheck review before code changes.
- Applies the approved source-fidelity fusion on a local temporary branch.
- Performs code-review-level intent tests without running project builds or tests.
- Runs a final check review on the actual diff.
- Creates an audit record under `docs/intent-cherry-pick/`.
- Creates a commit message that preserves the original source commit message and appends a short fixed `Intent-Cherry-Pick` section.
- Does not push temporary branches to remotes.
- Deletes the local temporary branch after successful merge-back when it is safe to do so.

## What It Does Not Do

- It does not run build, unit test, integration test, packaging, or custom validation commands by default.
- It does not optimize, refactor, rename, clean up, or broadly reformat code.
- It does not expand the requested scope.
- It does not push local work branches to a remote.
- It does not force-delete branches.
- It does not delete remote branches.

## Typical Prompt

```text
Use $intent-cherry-pick to port commit f437cc373 from pc_dev_main to pc_dev_taptap.
```

With scope constraints:

```text
Use $intent-cherry-pick to port commit f437cc373 from pc_dev_main to pc_dev_taptap.
Only include CommonData and GiantBaseSDK core changes. Do not include Steam or Rail dependencies.
```

In Chinese:

```text
使用 $intent-cherry-pick，把 pc_dev_main 上的 f437cc373 合入 pc_dev_taptap。
只保留 CommonData 和 GiantBaseSDK 的核心改动，不要合入 Steam/Rail 依赖。
```

## Workflow Summary

1. The user provides source branch, source commit/range, and target branch.
2. The agent inspects git history, source diff, target code, call chains, and related symbols.
3. The agent produces an intent plan:
   - Scope Contract
   - Intent Report
   - Source Fact Lock
   - Target Fact Lock
   - Fidelity Classification
   - Fusion Plan
   - Intent Test Plan
4. The human confirms the intent and any `L2` or `L3` adaptation.
5. Precheck reviews the plan.
6. The agent creates a local temporary branch from target and applies the approved fusion.
7. The agent performs code-review-level intent tests.
8. Check reviews the actual diff.
9. The agent creates a workflow record and commit.
10. If requested, the agent merges back to target and safely cleans up the local temporary branch.

## Audit Record

Records are written to:

```text
docs/intent-cherry-pick/YYYY/YYYY-MM-DD-<source-short-hash>-to-<target-branch>.md
```

The record contains:

- Source commit
- Scope
- Intent
- Source Fact Lock
- Target Fact Lock
- Evidence reviewed
- Fidelity classification
- Fusion plan
- Precheck result
- Human decisions
- Implementation summary
- Intent tests
- Check result
- Cleanup status
- Remaining risks

## Commit Message Format

The generated commit message keeps the source commit message first:

```text
<source commit title>

<source commit body, if any>

(cherry picked from commit <full source hash>)

Intent-Cherry-Pick:
Source-Branch: <source branch>
Target-Branch: <target branch>
Record: docs/intent-cherry-pick/YYYY/YYYY-MM-DD-<source-short-hash>-to-<target>.md
Precheck: PASS
Check: PASS
```

The appendix intentionally has no free-text `Note` field. Workflow details belong in the audit record.

## Language Rule

The skill detects the dominant language of the triggering prompt.

- If the prompt is mainly Chinese, workflow record body text is written in Chinese.
- If the prompt is mainly English, workflow record body text is written in English.
- Record titles and section headings remain English.
- Source commit messages are preserved exactly and are not translated.

## Files

The installable skill is in:

```text
intent-cherry-pick/
```

This outer `Intent Cherry-Pick/` directory is only a distribution package for sharing through git.
