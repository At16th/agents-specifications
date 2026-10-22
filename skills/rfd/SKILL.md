---
name: rfd
description: Write, update or change the state of an RFD (Request for Discussion) in the repo's rfds/ directory, following Oxide's RFD process. Use whenever a task is actionable and needs a plan, when the user asks for an RFD, or when an RFD's state changes (discussion, published, committed, abandoned).
---

# RFD skill

Every actionable change in this repo is planned as an RFD before it is
implemented. The process follows Oxide's RFD 1
(https://oxide.computer/blog/rfd-1-requests-for-discussion). An RFD is a
short, honest design document: why, what, how, and what is still unknown.

## When to use

- The user asks for an RFD, a plan, or a design for something.
- You are about to make a non-trivial change (infra, security, cost, data
  handling, tooling, process) and no RFD covers it yet.
- An existing RFD needs updating or its state changes.

Trivial fixes (typos, formatting, broken links) do not need an RFD.

## Hard rules

1. **Plan before acting.** Write the RFD and stop. Do not implement until the
   user has approved it (state `published`, or explicit approval in chat).
2. **No secrets.** Never put credentials, tokens, account IDs, ARNs with
   account IDs, or personal data in an RFD. Use placeholders like
   `<MGMT_ID>`.
3. **Never bypass hooks.** Don't commit with `--no-verify`. If a hook such as
   gitleaks fails, stop and report the finding.
4. **Open questions are explicit.** Anything undecided goes under "Open
   questions". Don't present guesses as decisions.
5. **One topic per RFD.** If the scope splits, write two RFDs and link them.

## Creating a new RFD

1. **Find the next number.** List `rfds/`, take the highest leading number,
   add 1. Filenames are zero-padded to 4 digits:
   `rfds/0003-short-kebab-slug.md`. In the title and in references, use the
   plain number ("RFD 3").
2. **Authors.** Use `git config user.name` and `git config user.email`:
   `authors: Name <email>`.
3. **State.** New RFDs start as `prediscussion`, or `ideation` if it's only a
   placeholder idea with no proposal yet.
4. **Write it from `template.md`** (next to this file). Keep the section
   order. Delete optional sections that genuinely don't apply; don't leave
   them empty.
5. **Update the index.** Add a row to `rfds/README.md` (create the file if
   missing, using the format below).
6. **Branch (if the user works with PRs):** `rfd/0003-short-kebab-slug`.
   When a PR is opened, set the state to `discussion` and put the PR URL in
   the `discussion:` field.
7. **Report back** with the file path, a 2–3 line summary, and the open
   questions, then wait.

## Writing guidance

- **Summary:** 2–4 sentences. What changes and why, readable on its own.
- **Background:** the problem and the current state. Link earlier RFDs
  rather than repeating them.
- **Proposal:** concrete design. Include config and code snippets where they
  remove ambiguity. Pin versions; say how they get updated.
- **Alternatives considered:** at least one, with the reason it lost. "Do
  nothing" counts.
- **Implementation:** numbered, verifiable steps. Mark the steps the user
  must do themselves (sudo, console clicks, approvals).
- **Cost:** required for anything that touches AWS or another paid service.
  Rough monthly estimate, what scales it, and which guard (budget, SCP)
  covers it.
- **Security and compliance:** required. Threats, what is enforced and what
  is only advisory, and any handling of personal data (retention, deletion,
  access).
- **Open questions:** numbered, so the discussion can reference them.
- Write plainly. Prefer short sentences and tables over long prose.

## States

| State | Meaning |
|---|---|
| `prediscussion` | Draft, not ready for review |
| `ideation` | Placeholder for an idea, no real proposal yet |
| `discussion` | Ready for review; PR open |
| `published` | Agreed and merged; implementation may start |
| `committed` | Implemented and in place |
| `abandoned` | Not pursued. Keep the file and record why at the top |

Allowed moves: `ideation` → `prediscussion` → `discussion` → `published` →
`committed`, and any state → `abandoned`. When changing the state, update both
the frontmatter and the index row. When moving to `committed`, add a short
"Outcome" note at the end saying what was actually done and where it differs
from the plan.

## Updating an existing RFD

- Change content only while the state is `prediscussion` or `discussion`.
- For `published` or `committed` RFDs, don't rewrite history: add an
  "Amendments" section with the date and the change, or write a new RFD that
  references and supersedes it.

## Index format (`rfds/README.md`)

```markdown
# RFDs

| RFD | Title | State | Discussion |
|---|---|---|---|
| [1](0001-aws-infrastructure-setup.md) | AWS infrastructure setup | discussion | |
| [2](0002-gitleaks-pre-commit-hook.md) | Gitleaks pre-commit hook | discussion | |
```

Keep rows sorted by number.
