<!-- vi: set wrap: -->

# Core agent standards

These rules apply to every repository that links this file. Repo-specific
context lives in that repo's `AGENTS.md`. If the two conflict, this file wins
unless the repo's `AGENTS.md` explicitly says it overrides a rule here.

## 1. Plan first, as an RFD

Every actionable task is planned before it is done, and every plan is written
as an RFD.

- **Before any non-trivial change** (code, infrastructure, security, cost,
  data handling, tooling, process), check `rfds/` for an RFD that covers it.
  - If one exists and its state is `published`, implement it as written.
  - If one exists but isn't `published` yet, don't implement. Point the user
    to it and ask whether to proceed.
  - If none exists, write one first.
- **Use the `rfd` skill** to create, update or change the state of an RFD.
  Load it automatically whenever any of the above applies. Don't wait for the
  user to ask for it.
  - Agents without skill support: read `.agents/skills/rfd/SKILL.md` and
    follow it exactly, using `.agents/skills/rfd/template.md`.
- **After writing an RFD, stop.** Report the path, a short summary and the
  open questions, then wait for approval.
- **No RFD needed for trivial changes:** typos, formatting, broken links,
  comment fixes. When unsure, ask instead of skipping.
- **If the plan turns out wrong during implementation, stop.** Explain what
  changed and update the RFD (or propose a new one) before continuing.

## 2. Secrets and sensitive data

- Never write credentials, tokens, keys, account IDs or personal data into any
  file, commit, RFD or log. Use placeholders such as `<MGMT_ID>`.
- Never read, print or copy files that are likely to contain secrets
  (`*.tfstate*`, `.env*`, `~/.aws/`, SSO caches) unless the user asks for
  that specific file.
- Treat user-uploaded images and personal data as sensitive by default.
  Proposals that store them must state retention and deletion.

## 3. Git

- Never use `--no-verify`, never force-push, and never rewrite published
  history without explicit approval.
- If a hook fails (gitleaks or any other), stop and report the finding. Don't
  work around it.
- Small commits with clear messages. Reference the RFD number when a commit
  implements one (for example `RFD 3: add state bucket`).

## 4. Infrastructure

- All cloud changes go through infrastructure as code (OpenTofu). Never change
  cloud resources by hand or with ad hoc CLI write commands. Read-only CLI
  calls are fine.
- Never run `tofu apply` or `tofu destroy` yourself. Run `plan`, show the
  result, and let the user apply.
- Use the CLI profile the repo's `AGENTS.md` names for each task. Never switch
  to a more privileged profile to get past an access-denied error. Report it
  instead.
- Pin versions (providers, modules, hooks, actions) and say how they get
  updated.

## 5. Cost

- Any change that can create cost includes a rough monthly estimate and names
  the budget or guardrail that covers it.
- Prefer always-free or usage-priced options at current scale. Flag anything
  that bills by the hour while idle.

## 6. Communication

- Be direct and brief. Say what you did, what you didn't do, and why.
- Say clearly what you're unsure about. Don't present guesses as facts.
- Ask before anything destructive or hard to reverse.

### Defining tasks

- Any task that changes a file, is to be considered an actionalble task.
- Every actionable task must go through a plan first, unless it is a write action to some documentation.

## Code rules

- No banner comments, no extra hyphens.
- Do not start with upper case letter comments <!-- personal perference -->
- Comment only when the context matters.

