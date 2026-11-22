<div align="center">

<p align="center"><b>English</b> | <a href="README.ja.md">日本語</a></p>

<h1 align="center">Repomix Auto</h1>

<h3 align="center">Generate a Repomix source map without reading the whole project.</h3>

<p align="center">A CLI that automates Repomix source map generation: freshness check, config creation, and automatic exclusion of heavy files. It helps when you start working on a new project.</p>

<p align="center">
  <a href="https://github.com/Lapius7/repomix-auto/releases"><img alt="Release" src="https://img.shields.io/github/v/release/Lapius7/repomix-auto?label=release&style=flat-square&color=e8833a&logo=github"></a>
  <a href="https://www.npmjs.com/package/@lapius/repomix-auto"><img alt="npm" src="https://img.shields.io/npm/v/@lapius/repomix-auto?style=flat-square&color=cb3837&logo=npm"></a>
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/github/license/Lapius7/repomix-auto?style=flat-square&color=97ca00"></a>
  <a href="https://repomix.com"><img alt="Repomix" src="https://img.shields.io/badge/for-Repomix-7c3aed?style=flat-square"></a>
  <a href="https://github.com/Lapius7/repomix-auto/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/Lapius7/repomix-auto?style=flat-square&color=e3b341&logo=github"></a>
  <a href="https://github.com/Lapius7/repomix-auto/commits/main"><img alt="Last commit" src="https://img.shields.io/github/last-commit/Lapius7/repomix-auto?style=flat-square&color=3b82f6"></a>
</p>

<p align="center">
  <a href="#quick-start">Quick start</a> ·
  <a href="#options">Options</a> ·
  <a href="#how-it-works">How it works</a> ·
  <a href="#safety">Safety</a>
</p>

</div>

---

## Quick start

You need Node 18 or later (Repomix is run with `npx`).

```sh
pnpm add -g @lapius/repomix-auto
repomix-auto [directory] [--force] [--check] [--path] [--out <dir>] [--no-auto-exclude]
```

Run `repomix-auto` in a project directory. It writes the map to `~/.cache/repomix-auto/<name>-<4-char path hash>.md`.

## Options

| Option | What it does |
|---|---|
| `--check` | Only check freshness, do not generate (fresh = exit 0, stale = exit 1) |
| `--force` | Regenerate regardless of freshness |
| `--path` | Print only the output path to stdout (for scripts; generates if needed) |
| `--out <dir>` | Output directory |
| `--no-auto-exclude` | Do not auto-exclude files from the Top 5 |
| `--update` | Update to the latest version (`pnpm add -g @lapius/repomix-auto@latest`) |
| `-y, --yes` | Run without asking, even in a directory that does not look like a project |

Environment variables: `REPOMIX_AUTO_OUT` (fixed output directory), `REPOMIX_AUTO_PM=npm|pnpm` (package manager used by `--update`; pnpm if available, otherwise npm).

## How it works

- **Output**: by default `~/.cache/repomix-auto/<name>-<4-char path hash>.md`. With `--out <dir>` or `REPOMIX_AUTO_OUT`, it becomes `<name>.md` in a fixed directory. An `output.filePath` in an existing `repomix.config.json` takes priority.
- **Freshness**: it does not regenerate unless a source file is newer than the output (under git, gitignored files are ignored). `--check` only checks.
- **Config**: it creates `repomix.config.json` and, under git, adds it to `.git/info/exclude`.
- **Auto exclude**: after the first run, files in the Top 5 that are i18n, migrations, data, or generated output are excluded automatically, and the reason is recorded in `headerText`. Files it cannot judge are listed.

## Safety

> [!WARNING]
> To avoid running it by mistake, it refuses to run in your home directory or at the filesystem root. In a directory with no `.git`, `package.json`, or similar marker, it asks first (non-interactive runs need `--yes`). If Repomix fails, the config created in that run is removed.

> [!NOTE]
> This README was rewritten from the previous text and checked against `npm/package/bin/repomix-auto` (options, environment variables, refusal rules, output path). The command was not run for this rewrite.

## Development

Releases are built by `.github/workflows/release.yml` when a `v*` tag is pushed. `npm/build.mjs` writes the tag version into `npm/package/package.json`.

## License

MIT
