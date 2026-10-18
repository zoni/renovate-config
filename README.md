# renovate-config

Shareable [Renovate](https://docs.renovatebot.com/config-presets/) presets for repositories under github.com/zoni.

Use a preset by adding it to `extends` in a repository's Renovate config.
For example: `extends: ['github>zoni/renovate-config:rust.json5', 'schedule:monthly']`.
The `.json5` suffix is required: without it, Renovate looks for `<name>.json`.
