---
name: release-pr
description: Create a Changesets version pull request for this repository. Use when the user says `リリースして`, `releaseして`, asks to release, or asks to create a release PR for `@yu21/biome-config`.
---

# Release PR

Use this skill to turn a release request into a reviewed release PR from
`develop` to `main`. Do not publish directly by default.

## Workflow

1. Inspect release state before changing anything:
   - `package.json`
   - `.changeset/config.json`
   - `.github/workflows/version.yml`
   - `.github/workflows/release.yml`
   - `CHANGELOG.md`
   - `git status --short --branch`
   - `git remote -v`
2. Decide the release source:
   - Normal changes must be merged to `develop`.
   - If `.changeset/*.md` files exist on `develop`, wait for or create the
     Changesets version PR against `develop`.
   - If `develop` already has an updated `package.json` and `CHANGELOG.md`,
     treat it as the release candidate.
   - If `develop` has releasable changes but no changeset and no version bump,
     stop and ask for the intended semver bump and changelog text.
3. Validate before creating a PR:
   - `pnpm run check`
   - `pnpm run pack:dry-run`
4. Confirm package contents from dry-run:
   - Must include `README.md`, `LICENSE`, `CHANGELOG.md`, `package.json`, and
     `configs/*.json`.
   - Must exclude `.changeset`, `.github`, `.vscode`, `.npm-cache`, `.tmp`,
     `node_modules`, lockfiles, and workspace-only config.
5. Prepare the release PR:
   - Create branch `codex/release-v<version>` from `develop`.
   - Before committing, summarize the diff and validation performed.
   - If no new commit is needed, do not create an empty commit.
   - Push the branch.
   - Create a Ready PR targeting `main` with title `Release v<version>`.

## PR Body

Include:

- Version being released.
- Validation commands and pass/fail outcome.
- Dry-run package contents summary.
- Note that merging to `main` lets `.github/workflows/release.yml` publish
  through Changesets.

## Safeguards

- Never run `npm publish` unless the user explicitly asks for direct publish
  after the risk is stated.
- If `origin` is missing, GitHub authentication is unavailable, no initial
  commit exists, or the release workflow cannot publish, stop and report the
  blocker.
- Never force-push for a release PR unless the user explicitly asks and
  confirms the target branch.
