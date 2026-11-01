# Contributing MCP server metadata

This public repository is the metadata companion to https://awesome-mcp.com.
It is not the private website source repository. Do not submit credentials,
personal information, private notes, install commands or executable code.

## Propose a server

1. Fork this repository and copy `examples/server.json` to
   `data/servers/<your-slug>.json`. The example is illustrative, not a real listing.
2. Use the actual public GitHub repository or `tree/<ref>/<component>` URL.
   Separate monorepo components must have distinct paths or package identities.
3. Use Node.js 24 or later. No dependency installation is needed:

   ```sh
   node scripts/validate-contributions.mjs data/servers
   node scripts/export-catalog.mjs --contributions data/servers README.md
   ```

4. Commit the JSON file and regenerated README, then open a pull request.
   Maintainers must review source identity, accuracy and all changed files.
   Workflow/tooling changes require separate explicit review.

## Schema version 1

Required fields: `schemaVersion` (1), `slug`, `title`, `description`, `repoUrl`,
`category`. `tags` defaults to an empty array. `npmPackage` and `pypiPackage` are
optional unversioned canonical package names. All other keys are rejected.

- Slug: lowercase ASCII letters, numbers, dots, hyphens and underscores; starts
  with a letter or number, at most 120 characters, no reserved/device filenames
  or trailing dot. The filename must be exactly `<slug>.json`.
- Title: 1-150 characters. Description: 10-5,000 characters. Do not include
  secrets: both JSON and descriptions will be public and remain in Git history.
- Repository URL: HTTPS `github.com`, without credentials, query or fragment.
- Tags: at most 20 unique case-insensitive strings, at most 80 characters each.
- Files: regular non-symlink JSON, at most 32,000 bytes each. Snapshot: at most
  10,000 entries and 32 MB. No extra directories/files under `data/servers`.
- Route, repository/component and npm/PyPI identity conflicts fail closed.

Categories: `developer-tools`, `databases-storage`, `browser-automation`,
`ai-agents-models`, `cloud-devops`, `search-web`, `file-systems-docs`,
`communication-social`, `auth-security`, `productivity-work`, `media-design`,
`finance-crypto`.

No proposal can assert installation/configuration review, official ownership,
client compatibility, tools/resources/prompts, security verification or measured
popularity. These require separate evidence in the website's review workflow.

## What CI checks

PR validation runs the validator/exporter from the trusted base commit against
the proposed JSON directory, not scripts from the proposed branch. The job has
read-only contents permission, no supplied secrets, no dependency installs and
no persisted checkout credentials. It also checks deterministic README output.
CI is not a substitute for maintainer review, especially for workflow changes.

Website-created draft PRs contain one metadata JSON file. A maintainer must
regenerate README and review the proposal before merging; their initial README
check intentionally fails until that review step is performed.

## Website synchronization

A merged proposal does not directly overwrite the deployed website or grant
review badges. Production synchronization/import is a separate maintainer task.
Existing reviewed records and private submissions must be preserved. If the
website's optional preview worker is not configured, web submissions remain in
the authenticated review queue and no PR is claimed to have been created.
