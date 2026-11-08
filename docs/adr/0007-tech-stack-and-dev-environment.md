# ADR-0007: 技術スタックのバージョンとローカル開発環境

- 状態: 提案中（バージョンは環境構築の Task で公式サイトを確認して確定する）
- 日付: 2026-11-07

## 背景

環境構築を始める前に、使う技術と、ローカル環境の作り方を決める。

## 技術スタック

| 分類 | 採用 | バージョンの方針 |
|---|---|---|
| Backend | Laravel | Laravel には LTS がない（v6 が最後）。**環境構築の時点で最新のメジャー**を使う。PHP はそのバージョンが対応する最新の安定版 |
| Frontend | Next.js（App Router）+ React + TypeScript | 最新の安定版 |
| Node.js | - | 最新の LTS |
| DB | PostgreSQL（[ADR-0003](./0003-database.md)） | 最新の安定版のメジャー |
| キュー / キャッシュ | Redis（互換の Valkey も可） | 最新の安定版 |
| 決済 | Stripe（テストモード） | - |
| CI | GitHub Actions | - |
| Backend のツール | Pest、Larastan、Laravel Pint、Deptrac（または Pest の arch テスト）、Horizon | - |
| Frontend のツール | Vitest、Testing Library、Playwright、zod、ESLint + Prettier（または Biome） | - |

確定したバージョンは、この ADR の表に追記してから「採用」にする。

## ローカル開発環境の選択肢

### A. 自分で `compose.yaml` を書く
- 良い点: 各サービスの役割と設定を理解できる。本番（Fargate のコンテナ）に近い構成を意識できる。
- 悪い点: 最初の設定に時間がかかる。

### B. Laravel Sail
- 良い点: すぐに起動できる。
- 悪い点: 中身を理解しないまま使うことになる。frontend を含めた構成には手を加える必要がある。

### C. devcontainer
- 良い点: エディタの環境まで揃えられる。
- 悪い点: Cursor との相性・設定の確認が必要。

## 決定（案）

A を採用し、必要になれば C を追加する。サービスの構成は次のとおり。

| サービス | 役割 |
|---|---|
| `frontend` | Next.js の開発サーバー |
| `backend` | Laravel の API（php-fpm + nginx、または FrankenPHP。環境構築時に決める） |
| `worker` | キューのワーカー（Horizon） |
| `relay` | Outbox の未送信分をキューへ投入する常駐プロセス |
| `scheduler` | 定期実行 |
| `db` | 開発用 DB。テスト用の DB も同じコンテナに作る |
| `redis` | キュー・キャッシュ |
| `mailpit` | メールの確認（会員登録のメール確認・パスワード再設定で使う） |

## 影響

- 環境構築の Epic では、まず「全サービスが起動し、CI が動く」までを1つの区切りにする。
- Stripe Webhook をローカルで受けるため、Stripe CLI の転送を使う。
