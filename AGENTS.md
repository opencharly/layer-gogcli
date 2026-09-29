# AGENTS.md — layer-gogcli

Standalone candy repo for the `gogcli` layer — the `gog` Google Workspace CLI
built with `go install` and landed at `~/go/bin/gog`. The candy lives in
`charly.yml` at the repo root and projects the `gogcli` skill entity
(`family: tools`).

Canonical files:

- `charly.yml` — the `gogcli:` candy entity and the `gogcli-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-tools:gogcli` — the owning skill: the `go install` install story,
  the `GOPATH`/`PATH` env, the OAuth-at-deploy boundary, and the observable
  checks. Load before editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package
  sections). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the
  `~/go/bin/gog` file check, the `gog --version` exit check, the `gog --help`
  stdout checks for `gmail`/`calendar`, and the `agent-check:` for live
  authenticated access.
- Build-scope proof needs no OAuth; the `agent-check:` covers the live path and
  is graded only when credentials exist.

## Modify this repo

- Edit the `gogcli:` candy entity in `charly.yml`; keep the matching
  `gogcli-skill:` entity in step with it.
- Keep the `env:`/`path_append:` block aligned with where the build lands the
  binary.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
