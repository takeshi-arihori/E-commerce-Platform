# E-commerce Platform

EコマースのWebアプリケーションを構築するプロジェクトです。

商品検索、購入、決済、注文管理、在庫管理など、ECサイトに必要な主要機能を実装します。

単純なCRUDアプリケーションではなく、実際のWebサービスを想定し、決済・在庫・注文処理・検索・スケーラビリティなどを考慮したシステムを構築することを目的とします。

## Project Goals

以下を満たすEコマースプラットフォームの構築を目指します。

- ユーザーが商品を検索・閲覧・購入できる
- ゲストユーザーでも商品を購入できる
- 会員ユーザーが購入履歴やウィッシュリストを利用できる
- 管理者が商品や在庫を管理できる
- Stripeを利用してオンライン決済を行う
- 決済結果に応じて注文・在庫情報を更新する
- 物理商品およびデジタル商品を販売できる
- 将来的なトラフィック増加に対応できる設計とする

## Development Phase

現在は **Phase 0 — Requirements Definition（要求定義）** です。

実装に入る前に、以下を整理します。

1. Business Requirements
2. User Requirements
3. Functional Requirements
4. Non-functional Requirements
5. Constraints
6. Scope / Out of Scope

要求定義完了後に設計フェーズへ進みます。

## Planned Features

現時点では、以下の機能を候補としています。

- Product Catalog
- Product Search
- Filtering / Sorting
- Shopping Cart
- Checkout
- Order Management
- Payment
- Inventory Management
- User Account
- Guest Checkout
- Purchase History
- Wishlist
- Digital Product Download
- Administration
- Stripe Integration
- Webhook Processing

詳細については要求定義の中で決定します。

## Technology

技術スタックについては、要求・非機能要件・システム設計を整理した上で決定します。

現時点の候補には以下を含みます。

- Backend: Go
- Database: MySQL
- Payment: Stripe
- Cloud: AWS
- CI/CD: GitHub Actions

重要な技術選定は Architecture Decision Record（ADR）として、採用理由だけでなく不採用案とその理由も記録します。

## Documentation

ドキュメントは Notion と GitHub で役割を分離します。

- **Notion**: 要求の草案、調査、比較検討、Open Questions、設計判断の検討過程
- **GitHub docs**: 確定した要求、設計方針、ADR
- **GitHub Issues**: 実装・調査タスク
- **GitHub Pull Requests**: 変更レビューと履歴

確定したプロジェクトドキュメントについては GitHub を Source of Truth とします。

```text
docs/
├── README.md
├── requirements/
│   ├── README.md
│   ├── business-requirements.md
│   ├── user-requirements.md
│   ├── functional-requirements.md
│   ├── non-functional-requirements.md
│   ├── constraints.md
│   └── scope.md
├── design/
│   └── README.md
└── adr/
    └── README.md
```

## Design Framework

設計フェーズでは、次の6層を厳密なウォーターフォール工程ではなく、**設計観点のチェックリスト**として使用します。

1. Upstream Design
2. Architecture Design
3. External Design
4. Internal Design
5. Cross-cutting Design
6. AI-assisted Development Design

設計上の重要なトレードオフは ADR に残します。

## Status

🚧 Phase 0 — Requirements Definition
