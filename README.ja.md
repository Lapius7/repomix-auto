<div align="center">

<p align="center"><a href="README.md">English</a> | <b>日本語</b></p>

<h1 align="center">Repomix Auto</h1>

<h3 align="center">プロジェクト全体を読まずに、Repomix のソースマップを作る。</h3>

<p align="center">Repomix のソースマップ生成を自動化する CLI です。鮮度の確認、config の作成、重いファイルの自動除外を行います。新しいプロジェクトに触り始めるときに使います。</p>

<p align="center">
  <a href="https://github.com/Lapius7/repomix-auto/releases"><img alt="Release" src="https://img.shields.io/github/v/release/Lapius7/repomix-auto?label=release&style=flat-square&color=e8833a&logo=github"></a>
  <a href="https://www.npmjs.com/package/@lapius/repomix-auto"><img alt="npm" src="https://img.shields.io/npm/v/@lapius/repomix-auto?style=flat-square&color=cb3837&logo=npm"></a>
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/github/license/Lapius7/repomix-auto?style=flat-square&color=97ca00"></a>
  <a href="https://repomix.com"><img alt="Repomix" src="https://img.shields.io/badge/for-Repomix-7c3aed?style=flat-square"></a>
  <a href="https://github.com/Lapius7/repomix-auto/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/Lapius7/repomix-auto?style=flat-square&color=e3b341&logo=github"></a>
  <a href="https://github.com/Lapius7/repomix-auto/commits/main"><img alt="Last commit" src="https://img.shields.io/github/last-commit/Lapius7/repomix-auto?style=flat-square&color=3b82f6"></a>
</p>

<p align="center">
  <a href="#クイックスタート">クイックスタート</a> ·
  <a href="#オプション">オプション</a> ·
  <a href="#仕組み">仕組み</a> ·
  <a href="#安全のために">安全のために</a>
</p>

</div>

---

## クイックスタート

Node 18 以降が必要です（Repomix は `npx` で実行します）。

```sh
pnpm add -g @lapius/repomix-auto
repomix-auto [ディレクトリ] [--force] [--check] [--path] [--out <dir>] [--no-auto-exclude]
```

プロジェクトのディレクトリで `repomix-auto` を実行すると、`~/.cache/repomix-auto/<名前>-<パスのハッシュ4桁>.md` にマップを出力します。

## オプション

| オプション | 説明 |
|---|---|
| `--check` | 生成せず鮮度だけ確認（fresh=0 / stale=1） |
| `--force` | 鮮度に関係なく再生成 |
| `--path` | 出力パスだけを標準出力に表示（スクリプト向け。必要なら生成する） |
| `--out <dir>` | 出力先ディレクトリ |
| `--no-auto-exclude` | Top 5 の自動除外をしない |
| `--update` | 最新版に更新（`pnpm add -g @lapius/repomix-auto@latest`） |
| `-y, --yes` | プロジェクトに見えないディレクトリでも確認せず実行 |

環境変数: `REPOMIX_AUTO_OUT`（出力先ディレクトリを固定）、`REPOMIX_AUTO_PM=npm|pnpm`（`--update` が使うパッケージマネージャ。pnpm があれば pnpm、なければ npm）。

## 仕組み

- **出力先**: 既定は `~/.cache/repomix-auto/<名前>-<パスのハッシュ4桁>.md` です。`--out <dir>` か `REPOMIX_AUTO_OUT` を使うと、固定ディレクトリの `<名前>.md` になります。既存の `repomix.config.json` に `output.filePath` があれば、それを優先します。
- **鮮度確認**: 出力より新しいソースがなければ再生成しません（git 管理下では gitignore 対象を除く）。`--check` なら確認だけを行います。
- **config**: `repomix.config.json` を自動作成し、git 管理下では `.git/info/exclude` に追記します。
- **自動除外**: 初回実行後の Top 5 のうち、i18n、マイグレーション、データ、生成物は自動で除外し、理由を `headerText` に記録します。判断できないものは一覧で表示します。

## 安全のために

> [!WARNING]
> 誤爆を防ぐため、ホームディレクトリとファイルシステムのルートでは実行を拒否します。`.git` や `package.json` などの目印がないディレクトリでは、先に確認します（非対話では `--yes` が必要）。Repomix が失敗したときは、その回に作った config を消します。

> [!NOTE]
> この README は以前の文面を書き直し、`npm/package/bin/repomix-auto`（オプション、環境変数、実行を拒否する条件、出力パス）と照合しました。書き直しにあたって、コマンドは実行していません。

## 開発

`v*` タグを push すると、`.github/workflows/release.yml` がリリースをビルドします。`npm/build.mjs` がタグの版を `npm/package/package.json` に書き込みます。

## ライセンス

MIT
