# ドキュメント

確定した内容の正本は GitHub（このディレクトリ）。タスクは GitHub Issues と GitHub Projects で管理する。

```text
docs/
├── product/        目的・利用者・MVP スコープ・対象外
├── modeling/       ドメインモデル図・ユビキタス言語（開発者本人が作る）
├── architecture/   システム構成・モジュール・整合性・認証
├── engineering/    開発の流れ・コーディング規約・テスト・レビュー
└── adr/            意思決定の記録
```

## 読む順番

1. [product/overview.md](./product/overview.md) — 何を作るか、何を作らないか
2. [architecture/overview.md](./architecture/overview.md) — 全体の構成と層のルール
3. [architecture/modules.md](./architecture/modules.md) — コンテキストの分け方と境界のルール
4. [architecture/consistency.md](./architecture/consistency.md) — トランザクション・ロック・イベントソーシング・キュー
5. [architecture/authentication.md](./architecture/authentication.md) — 認証・認可
6. [engineering/delivery-workflow.md](./engineering/delivery-workflow.md) — Issue・ブランチ・コミット・PR
7. [engineering/coding-standards.md](./engineering/coding-standards.md) / [testing.md](./engineering/testing.md) / [review.md](./engineering/review.md)
8. [modeling/README.md](./modeling/README.md) — モデリングの進め方
9. [adr/](./adr/) — 判断の理由

## 更新のルール

- ルール・設計・図に影響する変更は、コードと同じ PR で docs も直す。
- 判断を変えるときは ADR を追加する（採用済みの ADR は書き換えない）。
