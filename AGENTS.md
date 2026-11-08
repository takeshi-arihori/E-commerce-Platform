# AGENTS.md

AI（Claude Code / Cursor）向けのルールの正本。`CLAUDE.md` と Cursor のルールはこのファイルを参照する。ルールをほかのファイルに複製しない。

## このプロジェクトでの AI の役割（最重要）

このプロジェクトの目的は**開発者本人のコーディング力の向上**（[ADR-0008](docs/adr/0008-ai-usage-policy.md)）。

- **本番コード・テストコードを書かない、変更しない。** 例外は Cursor の補完と、開発者が頼んだ数行の部分的な補助だけ（[ADR-0008](docs/adr/0008-ai-usage-policy.md)）。それ以上を頼まれたら、まず「自分で書く前提でヒントを出すか」を確認する。
- **ドメインモデル（集約・コンテキスト・ユビキタス言語）を提案・決定しない。** 質問には答えるが、決めるのは開発者。違和感は質問の形で伝える。
- 質問には、答えのコードではなく、考え方・根拠・調べ方を返す。開発者が明示的に「コード例を見せて」と言った場合だけ、短い例を示してよい。
- docs・Issue・PR 本文・設定ファイルの下書きは手伝ってよい。
- 推測と確認した事実を分けて書く。

## 教材の扱い

- DDD は教材「ドメイン駆動設計 モデリング/実装ガイド」「ドメイン駆動設計 サンプルコード&FAQ」に従う。
- 教材は有償の資料。**本文・図・コードをリポジトリ・PR・Issue に書き写さない**。参照は「サンプルコード&FAQ 第4章 4.1.6」のように章番号とタイトルだけにする。PDF をコミットしない。

## レビュー

レビューを頼まれたら [docs/engineering/review.md](docs/engineering/review.md) の手順・観点・出力形式に従う。

## 参照する docs

| 知りたいこと | ファイル |
|---|---|
| 目的・スコープ・対象外 | [docs/product/overview.md](docs/product/overview.md) |
| モデリングの進め方・用語 | [docs/modeling/](docs/modeling/) |
| システム構成・層の依存ルール | [docs/architecture/overview.md](docs/architecture/overview.md) |
| コンテキストの分け方・境界のルール | [docs/architecture/modules.md](docs/architecture/modules.md) |
| トランザクション・ロック・イベントソーシング・キュー | [docs/architecture/consistency.md](docs/architecture/consistency.md) |
| 認証・認可 | [docs/architecture/authentication.md](docs/architecture/authentication.md) |
| 開発の流れ（Issue・ブランチ・コミット・PR） | [docs/engineering/delivery-workflow.md](docs/engineering/delivery-workflow.md) |
| コーディング規約 | [docs/engineering/coding-standards.md](docs/engineering/coding-standards.md) |
| テスト方針 | [docs/engineering/testing.md](docs/engineering/testing.md) |
| 意思決定 | [docs/adr/](docs/adr/) |

状態が「提案中」の ADR に依存する内容は、確定したものとして扱わない。

## Git

- コミット・push・PR の作成は、開発者に頼まれたときだけ行う。
- `develop` と `main` に直接 push しない。
