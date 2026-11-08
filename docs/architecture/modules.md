# モジュール（境界づけられたコンテキスト）

## 方針

- 1つの Laravel アプリケーションの中で、**コンテキストをディレクトリ（名前空間）で分ける**。後からアプリケーションを分割できるように、コンテキスト間の依存を最小にする（教材: モデリング/実装ガイド 第5章 5.7）。
- コンテキストの中では**層を最上位**に置き、その下を集約ごとに分ける（教材: モデリング/実装ガイド 第10章 10.2）。

```text
backend/
├── app/                         # Laravel の起動・共通設定だけ（業務コードを置かない）
└── modules/
    ├── <Context>/
    │   ├── Domain/<Aggregate>/  # エンティティ・値オブジェクト・イベント・リポジトリ interface
    │   ├── UseCase/<Aggregate>/ # ユースケース・入出力クラス・クエリサービス interface
    │   ├── Presentation/        # Controller・ジョブ・Request・Resource・ルート定義
    │   ├── Infra/               # リポジトリ実装・Eloquent モデル・外部クライアント
    │   ├── Contract/            # 他コンテキストへ公開する interface と統合イベント
    │   └── <Context>ServiceProvider.php
    └── Shared/
        ├── Domain/              # 複数コンテキストで使うドメインの部品（最小限）
        ├── UseCase/             # 複数コンテキストで使う interface（TransactionManager など）
        └── Infra/               # イベントストア・Outbox などの技術基盤
```

共有するものの置き場所は範囲で分ける。1つのコンテキストの中だけで共有するなら `<Context>/<層>/Shared/`、複数のコンテキストで共有するなら `modules/Shared/<層>/` に置く。各層が何に依存してよいかは [overview.md](./overview.md#バックエンドのレイヤー) の表に従う。

名前空間は `Modules\<Context>\<Layer>\...`（composer の PSR-4 で `modules/` を割り当てる）。

## コンテキスト間のルール

| ルール | 理由 |
|---|---|
| 他コンテキストの `Domain` / `UseCase` / `Infra` を import しない。使ってよいのは相手の `Contract` だけ | 内部の変更が他に波及しないようにする |
| 他コンテキストのテーブルを直接 SELECT / JOIN しない。外部キーもコンテキストをまたいで張らない | DB を分割できる状態を保つ |
| コンテキストをまたいで1トランザクションで更新しない | 結合を強めないため。結果整合性で扱う（[consistency.md](./consistency.md)） |
| 同期で情報が必要なら、相手の `Contract` の interface（クエリ）を呼ぶ | 依存の向きを明示する |
| 状態変化を伝えるときは統合イベントを Outbox 経由で発行する | 相手が落ちていても処理が進む |
| ID だけを相手に渡す。相手のエンティティを受け取らない | 集約を別のコンテキストに持ち込まない |
| `Shared` に置くものは最小限にし、追加するときはレビューで理由を確認する | 共有部分が増えるとコンテキストを分けた意味がなくなる |

DB のテーブルはコンテキストごとに PostgreSQL の schema で分ける（[ADR-0003](../adr/0003-database.md)）。

## コンテキストの一覧

モデリングの後に、開発者本人がここへ追記する。分け方の理由は ADR に残す。
