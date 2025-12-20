# nimdoc-deploy

A GitHub Action to set up Nim and run a nimble command (e.g. `nimble doc`) to build documentation and deploy it to GitHub Pages.

## Features
- Installs Nim (configurable version)
- Caches `~/.nimble`
- Runs a doc generation command (default: `nimble doc` with sensible flags and arguments)
- Copies generated project HTML to `index.html`
- Writes a `CNAME` from the configured homepage
- Uploads and deploys the generated site using Pages

## Requirement

- in Github of your repo, remember to switch `Settings` - `Pages` -
  `Source` to `"Github Action"` (whose default is `"Deploy from a branch"`)
- permissions:

```yaml
    permissions:
      pages: write
      id-token: write
```

## Inputs
(All inputs are optional):

- `nim-version` (string) — Nim version to install (default: `stable`).
- `branch` (string) — Branch to generate from (default: `master`).
- `deploy-dir` (string) — Output directory for generated docs (default: `.gh-pages`).
- `repo-token` (string) — GitHub token for actions that require it.
- `homepage` (string) — Repository homepage used for CNAME (defaults to repository homepage).
- `src-dir` (string) — the directory where your `proj-name` stays (default: `src`).
- `proj-name` (string) — Nim project name used to create `index.html` (defaults to repository name).
- `nimble-doc-cmd` (string) — Command to generate docs; if omitted the action runs a default `nimble doc` invocation.

## Example workflow

```yaml
name: Deploy Nim docs
on:
  push:
    branches: [ main ]

jobs:
  deploy-docs:
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - name: Deploy docs
        uses: nimpylib/doc-deploy
        with:
          nim-version: 'stable'
          branch: 'main'
          deploy-dir: '.gh-pages'
          proj-name: myproject
          # optional: override the default doc command
          # nimble-doc-cmd: 'nimble doc --index:on --project --outdir:.gh-pages src/myproject'


```

