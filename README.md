# E-commerce Platform

EコマースのWebアプリケーションを構築するプロジェクトです。

商品検索、購入、決済、注文管理、在庫管理など、ECサイトに必要な主要機能を実装します。

単純なCRUDアプリケーションではなく、実際のWebサービスを想定し、決済・在庫・注文処理・検索・スケーラビリティなどを考慮したシステムを構築することを目的とします。

## Project Goals

以下を満たすEコマースプラットフォームの構築を目指します。

* ユーザーが商品を検索・閲覧・購入できる
* ゲストユーザーでも商品を購入できる
* 会員ユーザーが購入履歴やウィッシュリストを利用できる
* 管理者が商品や在庫を管理できる
* Stripeを利用してオンライン決済を行う
* 決済結果に応じて注文・在庫情報を更新する
* 物理商品およびデジタル商品を販売できる
* 将来的なトラフィック増加に対応できる設計とする

## Development Phase

現在は **Requirements Definition（要求定義）** フェーズです。

実装に入る前に、以下を整理します。

1. ビジネス要求
2. ユーザー要求
3. 機能要求
4. 非機能要求
5. 制約
6. MVPのスコープ

要求定義完了後に、ユースケース、ドメインモデル、アーキテクチャ、データモデル、APIなどの設計を行います。

## Planned Features

現時点では、以下の機能を候補としています。

* Product Catalog
* Product Search
* Filtering / Sorting
* Shopping Cart
* Checkout
* Order Management
* Payment
* Inventory Management
* User Account
* Guest Checkout
* Purchase History
* Wishlist
* Digital Product Download
* Administration
* Stripe Integration
* Webhook Processing

詳細については要求定義の中で決定します。

## Technology

技術スタックについては、要求・非機能要件・システム設計を整理した上で決定します。

候補には以下を含みます。

* Backend: Go
* Database: MySQL
* Payment: Stripe
* Cloud: AWS
* CI/CD: GitHub Actions

技術選定については Architecture Decision Record（ADR）として意思決定の理由を記録する予定です。

## Documentation

プロジェクトの設計資料は `docs` ディレクトリで管理します。

```text
docs/
├── requirements/
├── use-cases/
├── architecture/
├── database/
├── api/
└── adr/
```

まずは `docs/requirements/` に要求定義を作成します。

## Status

🚧 Requirements Definition
