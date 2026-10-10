# **1. Copilot Instructions for STRM**

## **1.1 Mission**

This repository is the STRM workspace — Hermes Streaming Maintenance. It builds and maintains the request-driven streaming access-code workflow: receiving approved trigger messages, retrieving the latest one-time access code or activation state for a supported provider, and returning a safe, traceable response to the requester.

Keep all work aligned with the active repository, the project objective, and the current scoped instructions.

## **1.2 Sources of Truth**

Use current, active files in this order:

1. The user's request and explicitly tagged files.
2. This repository-wide instruction file.
3. Applicable files in `.github/instructions/`.
4. [The project objective](./docs/strm-objective.document.md).
5. Existing source, tests, configuration, and established repository patterns.

When guidance conflicts, follow the higher-priority and more specific applicable instruction. Do not use archived, generated, duplicated, or protected content as project guidance.

## **1.3 Working Rules**

- Inspect the relevant active files before changing them.
- Keep scope narrow and directly tied to the request.
- Prefer complete, minimal, readable changes over broad refactors or speculative additions.
- Preserve existing behavior unless the request intentionally changes it.
- Check `git status` before editing and preserve unrelated user changes.
- Reuse existing helpers, patterns, package managers, and repository-native commands.
- Do not create planning, tracking, or worklog files unless explicitly requested.
- Use the session workspace for temporary artifacts and remove them when finished.
- Surface failures and blockers explicitly; do not hide errors behind success-shaped output.

## **1.4 Protected Scope**

Follow [the ignore instructions](./instructions/copilot-ignore.instructions.md).

Do not inspect, validate, execute, or modify protected paths unless the user explicitly requests the specific path. This includes archives, logs, data, credentials, temporary files, dependency directories, generated output, ignored editor settings, and lockfiles listed by the ignore instructions.

Never expose secrets, tokens, passwords, allowlisted requester handles, provider login codes, or credential values in commands, logs, documentation, or responses.

## **1.5 Implementation and Validation**

- Apply the scoped coding, security, testing, and Markdown instructions relevant to each changed file.
- Make precise edits that address the root requirement without unrelated cleanup.
- Update directly related tests and documentation when behavior or setup changes.
- Validate with the smallest existing check that covers the change; expand only when targeted results require it.
- Do not install dependencies unless a manifest changed or validation fails because a required dependency is missing.
- Review the final diff for unintended changes, whitespace errors, invalid formats, stale references, and protected-path violations.
- Do not claim completion until applicable checks pass or a concrete blocker is reported.

## **1.6 STRM Copilot Layout**

Use the standard locations defined in [the Copilot layout standard](./instructions/copilot-standard.instructions.md):

- `.github/copilot-instructions.md` for repository-wide guidance
- `.github/instructions/*.instructions.md` for scoped rules
- `.github/agents/*.agent.md` for reusable custom agents
- `.github/skills/*/SKILL.md` for reusable workflows
- `.github/docs/*.document.md` for project documents and standards
- `.github/prompts/*.prompt.md` only for retained legacy compatibility

Do not create a prompt file when a skill is the appropriate reusable workflow.

## **1.7 Repository Conventions**

- Instruction files use `*.instructions.md`.
- Custom agent files use `*.agent.md`.
- Skill directories use a short descriptive name and contain `SKILL.md`.
- Project documents use `*.document.md`.
- Repository Markdown uses numbered headings such as `# **1. Title**`, `## **1.1 Section**`, and `### **1.1.1 Subsection**`.
- Text files use UTF-8 and LF endings.
- Hermes is the external AI runtime maintained in the separate HAP project. Use HAP's `hap-propagate-standard` skill when syncing shared `.github` files from HAP, and never expose Hermes credentials or unfiltered API-key status.

## **1.8 Implementation Location**

The provider scripts, tests, and config described in [the project objective](./docs/strm-objective.document.md) now live in this repository: `scripts/`, `tests/`, `config/`, along with the `strm-get-provider-codes` skill in `.github/skills/`. These were migrated from the HAP repository, which retains only its own Hermes/BASE runtime credentials in `config/KEYS.md`; STRM's own email and Telegram credentials live in this repo's `config/KEYS.md` (git-ignored).

This repository is not yet initialized as a git repo. Before the first commit, confirm `.gitignore` still excludes `config/KEYS.md` and any other credential files.

The launchd schedule at `~/Library/LaunchAgents/com.bennyskov.strm.viaplay-email.plist` was generated before this migration and still points at the old HAP script path. Re-run `scripts/install-wait-for-viaplay-email-schedule.sh` from this repository to regenerate it with the correct STRM path before relying on the scheduled poller.
