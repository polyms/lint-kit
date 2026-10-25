# @polyms/lint-kit

Biome 2.6 configuration presets for the Polyms ecosystem. The package expects `@biomejs/biome` >= 2.6.0.

## Presets

`biome-manifest.json` publishes four configs:

| Preset | Extends | What it adds                                                            |
| ------ | ------- | ----------------------------------------------------------------------- |
| base   | —       | Shared formatter and linter. Markdown and YAML formatters are disabled. |
| react  | base    | React and Tailwind lint domains, plus CSS, HTML, and SVG.               |
| next   | react   | Next.js lint domain.                                                    |
| node   | base    | Node.js globals. Includes JavaScript, TypeScript, and JSON only.        |

Extend one preset from `biome.json`:

```json
{
	"extends": ["@polyms/lint-kit/configs/react"]
}
```

The same path works for the other presets: `@polyms/lint-kit/configs/base`, `@polyms/lint-kit/configs/next`, and `@polyms/lint-kit/configs/node`.

An Nx monorepo that contains both Node and React projects should extend the react preset at the repository root. The node preset does not include JSX, CSS, or HTML.

Biome does not format or lint Flutter/Dart or C#.

## Markdown, MDX, and YAML

Those files stay on Prettier. The presets leave Biome's Markdown and YAML formatters off so the two tools do not format the same extension.

This repository checks both domains with:

```bash
pnpm check
```

That runs `biome check --write .`, then `prettier -l --write .`. `.prettierignore` keeps Prettier on Markdown, MDX, and YAML.

## Install

The package is published to GitHub Packages. Point the `@polyms` scope at that registry, then install:

```
@polyms:registry=https://registry.polyms.dev
```

```bash
pnpm add -D @polyms/lint-kit
```

## Commits

Subjects follow Conventional Commits: `<type>[scope]: <imperative summary>`. Types are `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `ci`, `style`, `perf`, and `build`. Lowercase after the colon, no trailing period, at most 72 characters. Scope is optional.

## Release

```bash
pnpm release
pnpm release minor
pnpm release 1.2.3
git push --follow-tags
```

`pnpm release` bumps `package.json` (patch by default), commits only that file as `chore(release): v{x.y.z}`, and creates an annotated tag. It does not push. `.github/workflows/publish.yml` publishes when that `v*` tag is pushed. The tag must match `version` (currently `0.1.0`).
