# renovate-config

Shareable [Renovate](https://docs.renovatebot.com/config-presets/) presets for repositories under github.com/zoni.

## Usage

Presets are composable: always extend `default.json5`, then add language, automerge and schedule presets as needed.
For example, a Rust repository with automerged lock file maintenance:

```json5
{
  extends: [
    'github>zoni/renovate-config:default.json5',
    'github>zoni/renovate-config:rust.json5',
    'github>zoni/renovate-config:rust-automerge-lockfile.json5',
    'schedule:monthly',
  ],
}
```

The `.json5` suffix is required: without it, Renovate looks for `<name>.json`.

## Presets

### Base

- `default.json5`: base settings for every repository.
  Extends `config:best-practices`, groups linters, applies a 5 day minimum release age (requiring release timestamps where registries provide them) and includes `github-actions.json5`.
- `github-actions.json5`: groups non-breaking workflow action updates and bumps actions under `zoni/` without delay.
  Included by `default.json5`.

### Languages

- `golang.json5`: updates indirect dependencies, runs `go mod tidy` and rewrites import paths on major updates.
- `rust.json5`: updates the toolchain version in `rust-toolchain.toml`.
- `python.json5`, `elixir.json5`, `javascript.json5` (JavaScript and TypeScript): currently empty, since everything they need is in `default.json5`.
  They exist so repositories can declare their language now and pick up future rules.
- `container.json5`: monthly schedule with automerged lock file maintenance, for container image repositories.

### Automerge

- `automerge-lockfile.json5`, `automerge-all.json5`: automerge lock file maintenance, or all updates, for every ecosystem.
- `<language>-automerge-lockfile.json5`: automerge lock file maintenance for one ecosystem.
- `<language>-automerge-all.json5`: automerge all updates, including majors, for one ecosystem.
  This is a superset of the `-lockfile` variant, so use one or the other.

Available for `golang`, `python`, `rust`, `elixir` and `javascript`.

## Validation

CI validates every preset on push and pull request.
To run it locally:

```sh
npx --yes --package renovate@latest -- renovate-config-validator --strict --no-global *.json5
```
