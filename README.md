# demo-auto-bug-fix

A small .NET console app used to demo automated issue triage and bug fixing with
Claude Code GitHub Actions.

## Automated issue handling

When an issue is opened, `.github/workflows/claude-issue-triage.yml` runs an agent
that works through four phases:

1. **Understand** - reads the issue and locates the relevant code.
2. **Verify** - builds and runs the project to reproduce the reported behaviour,
   then reaches one of: `CONFIRMED`, `NOT REPRODUCED`, `WORKS AS DESIGNED`,
   `NEEDS INFO`, or `NOT A BUG`.
3. **Triage** - comments on the issue with the verdict, the evidence, and the root
   cause, then applies labels.
4. **Fix** - if the bug is confirmed, well understood, small, and needs no design
   decision, it opens a PR with a minimal fix and a verified before/after.
   Otherwise it labels the issue `needs-human` and explains what is blocking.

The agent never pushes to `main` and never merges. Every fix arrives as a PR for
human review.

### Re-running triage

- Add the `claude-fix` label to an existing issue, or
- Run the **Claude Issue Triage** workflow manually with an issue number.

### Labels

| Label | Meaning |
| --- | --- |
| `triaged` | Verified and triaged by the agent |
| `bug` | Confirmed defect |
| `needs-info` | Not reproducible without more detail |
| `not-a-bug` | Question, feature request, or working as designed |
| `needs-human` | Agent could not safely fix it |
| `claude-fixed` | Agent opened a candidate fix PR |
| `claude-fix` | Add to re-trigger triage |

## Other workflows

- `claude.yml` - responds to `@claude` mentions in issues and PR comments.
- `claude-code-review.yml` - reviews pull requests.
