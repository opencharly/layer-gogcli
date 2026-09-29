# gogcli

The `gog` Google Workspace CLI — Gmail, Calendar, Drive, Contacts, Sheets, and
Docs from the shell.

`gogcli` builds the upstream `gog` binary from
[github.com/openclaw/gogcli](https://github.com/openclaw/gogcli) via `go install`,
landing it at `~/go/bin/gog` (placed on `PATH` by `path_append`). `gog` is a
single static Go binary that exposes the Google Workspace APIs as subcommands.
`gog --version` and `gog --help` run locally with no OAuth or network, so the
install is verifiable at build scope; live Workspace access additionally needs
OAuth credentials provisioned at deploy time.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `gogcli` |
| Requires | `@github.com/opencharly/layer-golang` |
| Binary | `~/go/bin/gog` |
| Env | `GOPATH=~/go`, `PATH` append `~/go/bin` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-gogcli:v2026.246.1816'
```

Then, inside the built image:

```bash
gog --version            # built binary runs
gog --help               # lists gmail, calendar, ... subcommands
gog gmail list           # once OAuth credentials are provisioned
```

## Layout

- `charly.yml` — the `gogcli:` candy entity: the `require:` dep, the `env:` /
  `path_append:` block, the `go install` step, and the `check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-tools:gogcli`
- `/charly-coder:golang` — the required Go toolchain dependency
- `/charly-tools:goplaces` — a sibling Google API CLI
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
