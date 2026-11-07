# システム構成

## 全体像

```mermaid
flowchart LR
    Browser[ブラウザ]
    subgraph Front[frontend: Next.js App Router]
        RSC[Server Components]
        BFF[Route Handlers / Server Actions<br/>= BFF]
    end
    subgraph Back[backend: Laravel モジュラーモノリス]
        API[HTTP API]
        Worker[Queue Worker]
        Scheduler[Scheduler]
    end
    DB[(RDB)]
    Redis[(Redis<br/>Queue / Cache)]
    Google[Google OIDC]
    Stripe[Stripe]

    Browser -->|Cookie| Front
    RSC -->|Bearer JWT| API
    BFF -->|Bearer JWT| API
    API --> DB
    API -->|ジョブ投入| Redis
    Worker --> Redis
    Worker --> DB
    Scheduler --> DB
    BFF -->|認可コード| Google
    API -->|コード交換・ID トークン検証| Google
    API --> Stripe
    Stripe -->|Webhook| API
```

- **ブラウザは Next.js とだけ通信する**。Laravel API はブラウザから直接呼ばない（Stripe Webhook のエンドポイントだけ外部公開する）。
- Next.js は BFF として JWT を httpOnly Cookie で保持し、API 呼び出し時に `Authorization: Bearer` を付ける（[authentication.md](./authentication.md)）。
- Laravel は1つのアプリケーションの中に複数の境界づけられたコンテキストを持つ（[modules.md](./modules.md)）。
- API プロセスとキューワーカーは同じコードベースから別プロセスとして起動する。
- DB は [ADR-0003](../adr/0003-database.md) で決める。

## リポジトリ構成

```text
.
├── frontend/              # Next.js（App Router）
├── backend/               # Laravel
├── docs/
│   ├── product/           # 目的・スコープ・要求
│   ├── modeling/          # ドメインモデル図・ユビキタス言語
│   ├── architecture/      # 構成・モジュール・整合性・認証
│   ├── engineering/       # 開発フロー・規約・テスト・レビュー
│   └── adr/               # 意思決定の記録
├── .github/               # Issue / PR テンプレート、CI
├── compose.yaml           # ローカル開発環境（環境構築 Epic で作成）
├── AGENTS.md              # AI 向けルールの正本
└── CLAUDE.md              # AGENTS.md を読み込むだけ
```

理由は [ADR-0001](../adr/0001-monorepo.md)。

## バックエンドのレイヤー

教材に合わせ、**オニオンアーキテクチャ**を採用する（[ADR-0002](../adr/0002-modular-monolith-onion.md)）。

```mermaid
flowchart TB
    P[presentation<br/>Controller・Request/Response・認証情報の取得]
    U[usecase<br/>ユースケース・入出力クラス・クエリサービスの interface]
    D[domain<br/>エンティティ・値オブジェクト・ドメインイベント<br/>リポジトリ interface・ドメインサービス]
    I[infra<br/>リポジトリ実装・Eloquent・外部 API クライアント・キュー]
    P --> U --> D
    I -.実装.-> D
    I -.実装.-> U
```

| 層 | 依存してよいもの | 禁止 |
|---|---|---|
| domain | 同じコンテキストの domain、PHP 標準 | Laravel（Facade・Eloquent・Collection 含む）、他の層、他のコンテキスト |
| usecase | domain | presentation / infra、Eloquent、HTTP |
| presentation | usecase、domain（値オブジェクトの生成） | infra の具象クラス、Eloquent |
| infra | domain、usecase の interface、Laravel、外部ライブラリ | presentation |

- 具象クラスの組み立て（DI のバインド）はコンテキストごとの ServiceProvider で行う。
- この表は Deptrac（または Pest の arch テスト）で CI が検査する（[coding-standards.md](../engineering/coding-standards.md)）。

## フロントエンドの構成

```text
frontend/src/
├── app/                 # ルーティング（ページは薄く保つ）
├── features/<機能>/      # 画面部品・その機能専用の処理
├── lib/api/             # Laravel API クライアント（server-only）
├── lib/auth/            # Cookie・トークン更新（server-only）
└── components/          # 機能をまたいで使う UI 部品
```

- Laravel API を呼ぶコードは `server-only` にし、クライアントコンポーネントから import できないようにする。
- 画面から更新するときは Server Actions か Route Handlers を経由する。
- 業務ルールはフロントに書かない。表示用の整形と入力チェック（UX 向け）だけにする。
