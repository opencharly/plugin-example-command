# AGENTS.md — plugin-example-command

Standalone plugin repo for the `examplecommand` capability
(`command:examplecommand`) — the reference out-of-tree command-class plugin. The
plugin is a Go module at `candy/plugin-example-command/` (module path
`github.com/opencharly/plugin-example-command/candy/plugin-example-command`); the
root `charly.yml` only declares `discover: candy` so the repo is a project and
its candy is scanned.

Canonical files:

- `candy/plugin-example-command/charly.yml` — the `plugin-example-command:`
  candy entity (`plugin:` block, `plan:` check).
- `candy/plugin-example-command/plugin.go` — the provider (`NewProvider()` +
  `NewMeta()` + `CliMain`/`runCommand`).
- `candy/plugin-example-command/schema/examplecommand.cue` — the served
  declaration surface.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model (incl. the `command` class), the per-plugin
  CUE-schema contract, placement. Load before touching the provider or schema.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-example-command/` — compile the plugin
  module.
- `go test ./...` in `candy/plugin-example-command/` — the plugin's Go tests
  (`schema_serve_test.go`).
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- The full `charly examplecommand` end-to-end is exercised by the Go e2e + the
  live R10, not by the candy's build-context `plan:` check.

## Modify this repo

- Edit the `plugin-example-command:` candy entity, the Go source, and
  `schema/examplecommand.cue` **together**.
- The command is **external-capable** (charly prescans it into the CLI grammar
  and fork/execs the binary); keep `CliMain` and the in-proc `Invoke(OpRun)` path
  consistent.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
