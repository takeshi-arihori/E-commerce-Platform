# ADR-0005: 認証方式（Google OIDC・パスワード + JWT + BFF）

- 状態: 採用
- 日付: 2026-11-07

## 背景

会員の認証が必要。Google アカウントでのログインと、メールアドレス + パスワードでのログインの両方を提供する。
フロントエンドは Next.js（App Router）で、Server Components からも API を呼ぶ。

## 制約

- Laravel Sanctum は使わない。JWT を使う。
- Apple ログインは作らない（Apple Developer Program の費用と、HTTPS のコールバックが必要になるため）。
- トークンをブラウザの JavaScript から読めない形で持つ。

## 検討した選択肢

### 誰が会員とトークンを管理するか
- **A. Laravel（Identity コンテキスト）が本人確認（Google の ID トークンの検証、またはパスワードの照合）を行い、自前の JWT を発行する**
  - 良い点: 会員というドメインがバックエンドにまとまる。ログイン手段を増やしてもトークンの仕組みは同じ。
  - 悪い点: トークンの発行・更新・無効化を自分で作る必要がある。
- B. Next.js（Auth.js）がログインとセッションを管理し、Laravel はそれを信用する
  - 悪い点: 会員の情報がフロント側に寄る。Laravel 側の認可が Auth.js の形式に依存する。

### トークンの持ち方
- **A. Next.js（BFF）が httpOnly Cookie で持ち、API 呼び出し時に Bearer で付ける**
  - 良い点: XSS で盗まれにくい。Server Components からも同じ方法で API を呼べる。Laravel をブラウザに公開しなくてよい。
  - 悪い点: クライアントからの更新系も Next.js を経由するため、一段増える。
- B. ブラウザの localStorage に JWT を保存する
  - 悪い点: XSS で盗まれる。
- C. BFF を使わず、Laravel が `Set-Cookie` で httpOnly Cookie を発行し、ブラウザが Laravel を直接呼ぶ
  - 良い点: 中継のコードが不要で、通信の経路が短い。アクセス集中時に Next.js が律速になりにくい。
  - 悪い点: Server Components からの呼び出しでは結局 Next.js が Cookie を転送するため、認証の経路が2つになる。Laravel 側に CORS と CSRF の対策が必要になる。API 全体をインターネットに公開する。

## 決定

- A + A を採用する。詳細は [authentication.md](../architecture/authentication.md)。
- アクセストークンは15分の JWT、リフレッシュトークンは30日でローテーションと再利用の検知を行う。
- MVP のログイン手段は Google とパスワードの2つ。どちらも Identity コンテキストのログイン手段とし、トークンの仕組みは共通にする（[authentication.md](../architecture/authentication.md#パスワードログイン)）。

## 採用しなかった案と理由

- Laravel Sanctum（SPA の Cookie セッション）: JWT を使う方針のため。また、Next.js のサーバー側から呼ぶときに Cookie と CSRF トークンの転送が必要になり複雑になる。
- Apple ログイン: 費用と開発環境の HTTPS の手間に対して、学習上の価値が小さい。
## 影響

- JWT の署名鍵を管理する必要がある（ローカルは `.env`、本番は Secrets Manager などをデプロイのフェーズで決める）。
- JWT のライブラリ（lcobucci/jwt、firebase/php-jwt など）は実装の Task で選ぶ。
- パスワードを預かるため、ハッシュ化・メール確認・再設定・総当たり対策を作って守る必要がある。メール送信の仕組みも MVP から必要になる。
- 全ての API 呼び出しが Next.js を経由するため、注文の集中時は Next.js も一緒にスケールさせる必要がある。負荷試験で問題が出たら、該当の API だけ直接呼ぶ形を別の ADR で検討する。
- アクセストークンはすぐには無効にできない（最大15分）。権限の取り消しを即時に反映したい場合は、別の仕組みが必要になる。
