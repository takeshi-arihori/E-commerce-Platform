# 開発の進め方

## 全体の流れ

```text
Epic Issue → Task Issue（sub-issue）→ Branch → PR → AI レビュー → 自分でマージ
```

- **1 Task = 1 PR**。Task は「1つの PR で届けられ、単独で確認できる成果」の大きさにする。
- 決まっていないことがあれば、先に ADR の Task を作って決めてから実装の Task に着手する。
- 依存している Task がマージされるまで、次の Task に着手しない。

## GitHub Projects

Issue はすべて GitHub Projects（Project 名: `E-commerce Platform`）で管理する。

| フィールド | 値 |
|---|---|
| Status | `Backlog` → `Ready` → `In Progress` → `In Review` → `Done` |
| Type | `Epic` / `Task` / `ADR` / `Bug` |
| Context | モデリングで決めたコンテキスト / `Shared` / `Frontend` / `Infra` |
| Priority | `P0`（MVP に必須）/ `P1` / `P2` |
| Size | `S`（半日）/ `M`（1〜2日）/ `L`（3日以上。分割を検討する） |

- `Ready` にする条件: Task テンプレートの Requirement・Done Criteria・テスト方針が書かれ、依存先の Task が `Done` になっている。
- `In Progress` は同時に1つまで。
- Epic と Task は GitHub の **sub-issue** で親子にする。依存関係は Task の「依存」欄に書く。
- ビュー: Board（Status 別）、Epic 別の Table、Context 別の Table。

## Epic と Task の分け方

- Epic には「目的・範囲・Task の一覧と依存関係」だけを書く。Task の詳細は Task Issue に書く。
- Epic を分けた時点では、Task はタイトル・親 Epic・依存だけ書く。残りは着手するときに書く。
- 分ける単位の目安:
  1. ADR（決めること）
  2. ドメイン層（集約・値オブジェクト + 単体テスト）
  3. ユースケース層（+ Fake のリポジトリを使ったテスト）
  4. インフラ層（リポジトリ実装 + 実 DB の統合テスト / 並行実行テスト）
  5. プレゼンテーション層（API + Feature テスト）
  6. フロントエンド（画面）
- 例:

```text
epic: 在庫引き当て
- [ ] adr: 在庫の引き当て方式（悲観ロック / 条件付き UPDATE）を決める
- [ ] task: 集約と値オブジェクトを TDD で作る ← ADR
- [ ] task: 在庫を引き当てるユースケースを作る ← 集約
- [ ] task: 在庫リポジトリを実装し、並行実行テストで売り越しがないことを確認する ← ユースケース
- [ ] task: 在庫引き当て API を作る ← リポジトリ
```

## ブランチ

- 既定ブランチは `develop`。`main` はリリース用（デプロイのフェーズで運用を決める）。
- `develop` から `<type>/<Issue番号>-<説明>` の名前で作る（例: `feat/12-short-description`）。
- `develop` と `main` に直接 push しない（ブランチ保護を設定する）。

## コミット

- [Conventional Commits](https://www.conventionalcommits.org/ja/v1.0.0/) の形式にする。scope にはコンテキストを書く。

```text
feat(<context>): 在庫を引き当てるメソッドを追加
test(<context>): 注文確定のイベントのテストを追加
```

- type: `feat` / `fix` / `test` / `refactor` / `docs` / `chore` / `ci` / `build`
- TDD のサイクルが履歴から読めるように、`test:` → `feat:` → `refactor:` の順でコミットする。

## Pull Request

- 本文に `Closes #<Task 番号>` を書く。テンプレートに従う。
- 変更は生成物を除いて**約400行以内**。超えそうなら Task を分ける。
- ルール・設計・図に影響する変更は、同じ PR で docs も直す。
- マージ前に [review.md](./review.md) の手順で AI レビューを受け、指摘への対応（直した / 直さない理由）を PR のコメントに残す。
- CI が通ってからマージする。マージ方法は **Squash merge**（PR のタイトルを Conventional Commits の形式にする）。

## リポジトリの初期設定

環境構築 Epic の最初の Task で次を行う。

- ラベル `epic`・`task`・`adr` を作る（Issue テンプレートが使う。`bug` は既にある）
- GitHub Projects を作り、上記のフィールドとビューを設定する
- `develop` と `main` にブランチ保護を設定する

## ADR

- `docs/adr/NNNN-<英語の短い名前>.md` に書く（[ADR の README](../adr/README.md)）。
- 状態が「採用」になったら本文を書き換えない。変えるときは新しい ADR を作り、古い方を「置換済み」にする。
