---
name: ecc-capability-publisher
origin: community
description: Add, update, or remove a requested skill, agent, command, rule, hook, or connector in the user's ECC GitHub fork. Inspect the target repo, follow its current contribution and format rules, validate the change, and publish it to the user's fork without pushing to upstream.
---

# ECC Capability Publisher

Use this skill when the user asks to put a capability into their ECC repository, for example, "add this skill to my ECC GitHub repo." The requested capability and target repository must be clear from the current request or workspace. Do not infer a capability's purpose from its filename alone.

## When to Activate

Activate when a user asks to add, update, or remove a capability in their ECC fork or maintained mirror, or to import a capability from a GitHub repository into ECC.

## Identify the target safely

1. Inspect the current working directory, Git root, `git status`, and `git remote -v`. Read the target repository's `README`, `CONTRIBUTING`, manifests, and validation scripts before editing. Treat the target repository's current documentation and format as authoritative; do not assume the installed ECC version's paths or commands are current.
2. Use an explicit repository URL when supplied. Otherwise, use the current working repository only when it is clearly an ECC checkout and its `origin` is the user's repository. If there are multiple plausible ECC repositories, ask which one to use. If the target URL is known but not cloned, clone it to a new, non-existing workspace path; never overwrite another folder.
3. Confirm the checkout contains ECC's plugin structure and that `origin` is user-controlled. If GitHub does not identify it as a fork, accept it as a standalone mirror only when `upstream` points to the official ECC repository and the user owns `origin`. `origin` is the push target; `upstream` is read-only for fetching Affaan's changes. Never push to `upstream` or change a remote URL to bypass a permission problem.
4. If the repository is empty, is not an ECC checkout, or its ownership and remotes cannot be verified, stop before writing or publishing. Explain exactly what is missing. Do not treat a new empty repository as a fork or fill it with guessed files. If local Git cannot access the target, use an available GitHub connector only when it can perform and verify the required changes; otherwise state what access is missing.
5. Do not print, copy, commit, or upload credentials. Keep tokens in the user's credential manager or environment. Never put secret values in `.mcp.json`, manifests, skill files, agent files, logs, or command history.

## Prepare the requested capability

For a requested skill, first invoke ECC's `skill-scout` procedure if available to search for an existing match. If the user requests an exact capability or asks to proceed after the search, create or edit the requested files. ECC's `/skill-create` command is specifically for deriving a skill from local Git history; do not present it as a general ECC publisher.

Before writing, inspect the repository's instructions and the current examples of the same capability. Use its actual conventions, schemas, registry behavior, and supported clients. Common ECC layouts have included `skills/<name>/SKILL.md` for skills, `agents/<name>.md` for Claude agents, and `.codex/agents/<name>.toml` for Codex agents; verify these paths against the target checkout before using them. If a cross-client projection or manifest update is required, follow the target repository's own contribution instructions. Do not create speculative aliases, duplicate capabilities, or undocumented paths.

Make the smallest change that satisfies the request. Define a skill or agent from the user's stated purpose, role, inputs, outputs, boundaries, and checks. If essential details are missing, ask only for those details. For hooks and connectors, inspect the current event or MCP schema first. Keep connector secrets out of tracked configuration and use supported environment-variable or secret-store references.

## Validate and publish

1. Create a descriptive feature branch from the target repository's default branch. Preserve unrelated existing work. Do not discard, reset, or stage unrelated changes.
2. Run the repository's required validators for the changed capability. For a new skill, run the repository's skill validator and any cross-client compatibility validator it documents. Run relevant tests and `git diff --check`. If checks cannot run, state that clearly and do not describe them as passed.
3. Inspect the complete diff and staged filenames. Check for accidental credentials, unrelated files, and generated artifacts. Stage only the requested files and necessary manifests or cross-client projections.
4. Commit the change and push only to `origin`, the verified user fork or mirror. When a connected GitHub tool is available, open a pull request from the feature branch into the user's fork's default branch after checks pass. Otherwise, report the pushed branch and provide the next PR step. Never open a pull request to Affaan's upstream unless the user explicitly asks to contribute upstream.
5. Do not merge, release, publish a package, or install the changed plugin unless the user requests that next action. Update versions only for a release workflow, not for every capability commit.

If the user asks to sync from Affaan, fetch from the configured upstream remote, inspect the diff, merge into the user's fork branch, resolve conflicts without dropping the user's work, rerun checks, and push to `origin`. If upstream is unavailable or deleted, preserve the fork and its history; report that upstream synchronization is unavailable and do not replace or delete the user's repository.

## Report

Give a short factual summary of the capability added or changed, the exact files, the validation results, and the branch or commit pushed. Distinguish a pushed branch from a merged change. State any action that remains, such as creating or merging a pull request. Never claim the capability is installed or active unless that was separately verified.

## Anti-Patterns

Do not push to the upstream ECC repository without an explicit request. Do not assume a source repository is safe or compatible without inspecting it. Do not commit secrets, unrelated changes, or unverified file paths. Do not report a change as installed, merged, or active unless that state was verified.
