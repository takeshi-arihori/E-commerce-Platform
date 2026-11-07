# E-commerce Platform

実サービスを想定した EC サイト。**モジュラーモノリス + DDD + イベントソーシング**で構築し、在庫の同時更新・決済・アクセス集中などの整合性と負荷の問題を、ロック・キュー・イベントで解決する。

開発者本人のコーディング力の向上が目的のため、実装は手で書き、AI（Claude Code / Cursor）はレビューに使う。

## 技術スタック

| 分類 | 技術 |
|---|---|
| Frontend | Next.js（App Router）/ TypeScript |
| Backend | Laravel（モジュラーモノリス・オニオンアーキテクチャ） |
| DB | PostgreSQL（推奨。[ADR-0003](docs/adr/0003-database.md) で検討中） |
| キュー | Redis + Laravel Queue（Horizon） |
| 認証 | Google（OpenID Connect）+ JWT |
| 決済 | Stripe |
| CI | GitHub Actions |
| タスク管理 | GitHub Issues + GitHub Projects |

## 現在のフェーズ

🚧 **環境構築前のルール決め**

1. ✅ 目的・スコープ・アーキテクチャ・開発ルールを決める
2. ⬜ 「提案中」の ADR（[DB](docs/adr/0003-database.md)・[バージョンと開発環境](docs/adr/0007-tech-stack-and-dev-environment.md)）を確定する
3. ⬜ GitHub Projects を作り、環境構築の Epic と Task を作る
4. ⬜ 環境構築
5. ⬜ ドメインモデリング → 実装

## ドキュメント

[docs/README.md](docs/README.md) を参照。AI 向けのルールは [AGENTS.md](AGENTS.md)。
