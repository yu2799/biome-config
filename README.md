# @yu21/biome-config

Shareable Biome configuration presets.

## For users

### Install

```sh
pnpm add --save-dev --save-exact @biomejs/biome @yu21/biome-config
```

This package expects `@biomejs/biome` `>=2.4.15 <3` in the consuming
project.

### Usage

Create a `biome.json` in your project and extend the base preset.

```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "extends": ["@yu21/biome-config/base"]
}
```

Exported presets do not extend each other. Compose them in the consuming
project's `extends` array from the least specific preset to the most specific
preset.

React projects can add the React preset after the base preset.

```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "extends": ["@yu21/biome-config/base", "@yu21/biome-config/react"]
}
```

Project-aware import and dependency checks can be enabled with the project
preset.

```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "extends": ["@yu21/biome-config/base", "@yu21/biome-config/project"]
}
```

Test suites can add the test preset after the base preset.

```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "extends": ["@yu21/biome-config/base", "@yu21/biome-config/test"]
}
```

Playwright projects can add the test and Playwright presets after the base
preset.

```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "extends": [
    "@yu21/biome-config/base",
    "@yu21/biome-config/test",
    "@yu21/biome-config/playwright"
  ]
}
```

Type-aware checks can be enabled with the types preset.

```json
{
  "$schema": "./node_modules/@biomejs/biome/configuration_schema.json",
  "extends": ["@yu21/biome-config/base", "@yu21/biome-config/types"]
}
```

### Presets

| Export | Purpose |
| --- | --- |
| `@yu21/biome-config` | Base preset. |
| `@yu21/biome-config/base` | Framework-independent formatter, assist, files, JavaScript, JSON, CSS, and common lint rules. |
| `@yu21/biome-config/react` | React and JSX rule overlays. |
| `@yu21/biome-config/project` | Project-aware import and dependency rule overlays. |
| `@yu21/biome-config/test` | Test-runner-agnostic test rule overlays. |
| `@yu21/biome-config/playwright` | Playwright rule overlays. Compose it after the test preset. |
| `@yu21/biome-config/types` | Type-aware rule overlays. |

## For developers

### Setup

```sh
pnpm install
```

### Checks

```sh
pnpm run check
pnpm run pack:dry-run
```

`pnpm run check` runs Biome in CI mode. `pnpm run pack:dry-run` verifies the
npm package contents without publishing.

### Changesets

```sh
pnpm run changeset
```

Create a changeset for user-facing package changes. The `pnpm run version` and
`pnpm run release` scripts are normally run by GitHub Actions, not as the
primary local release flow.

### Release flow

This repository uses pnpm for dependency management and npm for package
publishing. npm publishing is expected to use Trusted Publishing from GitHub
Actions.

Changes are merged to `develop`. Changesets creates version pull requests
against `develop`. Release pull requests merge `develop` into `main`, and
`.github/workflows/release.yml` publishes from `main`.
