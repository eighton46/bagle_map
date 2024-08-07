## サービス概要

ベーグル専門店を可視化する地図アプリ。ベーグル好きの人に、現在地近辺のお店を地図上に表示させることで外出先等でふとしたタイミングでベーグル専門店を検索することができます。

## このサービスへの思い・作りたい理由

母がベーグル好きで（ベーグルにハマって）、自分も一緒に食べるようになりました。スーパーやコンビニでも売っていますが、ベーグル専門店のベーグルを食べたときにとても美味しかったの覚えています。以降、都内などに遠出した際に、ベーグル専門店を探して買って帰るようになりました。しかし、ベーグル専門店は比較的小さなスペースで個人でやられている事が多いので、駅から少し距離があったり、入り組んだ路地の中にあったりすることもよくあり、探さないと見つからないことが多いです。また、某マップアプリで検索すると上手く見つからなかったり（検索から漏れたり）、検索範囲が狭いとベーグル専門店以外（ベーカリー、カフェ etc）が検索結果に表示されることがありもどかしさを感じていました。そこで、「いっそのことベーグル専門店だけマップ上にすぐ表示されればいいのにな」と思ったのが作りたい理由です。

## ユーザー層について

ベーグル好きの方、ベーグル専門店に訪れてみたい方。ベーグルは卵、牛乳、バターを使用していないので、比較的罪悪感なく食べられます。ベーグル好きの方にはもちろん、興味が持ってお店に訪れてみたい方の助けになったらと思いました。

## サービスの利用イメージ

現在地や検索した場所の周辺のベーグル専門店を地図上に表示します。ユーザーが外出先、在宅時関係なくベーグル専門店に行ってみたいと思ったときに検索できます。また、お店の情報（評価、レビュー含む）を確認でき、現在地からのルート検索ができます。

## ユーザーの獲得について

SNS で公開します。

## サービスの差別化ポイント・推しポイント

類似サービスとして、以下のものが挙げられます。
- Google マップ
- 食べログ
- PUG BAGLE Works さんの Google My Maps

対して、当サービスの差別化ポイントは以下の通りです。
- ベーグル専門店検索時の煩わしさの解消（ベーカリー、カフェが検索に引っかからない）
- 最寄りのお店を優先して検索する機能
- 営業時間の可視化

## 機能候補

### MVP リリース

**【すべてのユーザー】**

- 検索機能
  - 現在地または目的地付近のスポット検索
  - Mapと一覧表示（表示範囲のスポットのみ）
  - 上記のスポットの詳細表示

### 本リリース

**【すべてのユーザー】**

- 検索機能
  - 評価（星）から絞り込み
  - 営業時間による表示変更（ex. 営業中：青、営業終了：赤　もしくは営業時間外を非表示）
  - タグ検索
- ルート機能
  - 現在地または目的地からスポットまでの経路を表示（Google マップのルートに飛ばす？）

**【登録ユーザーのみ】**

- 評価機能

  - コメント
  - タグ（ex. 「やわらかめ」、「ふつう」、「かため」の 3 つから 1 つ選択し、割合を表示）

- スポットをお気に入りに追加
- マイページの編集
  - コメント、タグ、お気に入りリストを編集・削除
  - プロフィールの変更

**【管理ユーザーのみ】**

- 登録ユーザーの検索、一覧、詳細、編集
- スポットの検索、一覧、詳細、編集、削除
- コメントの検索、一覧、詳細、編集、削除
- 管理ユーザーの一覧、詳細、編集、削除
- 新しいスポットを追加（Google マップ上のカテゴリーとしてはベーグル専門店ではないが、ベーグルが有名なベーカリーやカフェの追加）

## 機能の実装方針予定
- バックエンド
  - Ruby
  - Ruby on Rails
- フロントエンド
  - HotWire
- インフラ
  - Fly.io
- API
  - Google Maps Platform

## 画面遷移図
https://www.figma.com/design/fUJwAjQd0BwRJ7LpGOf2pv/%E7%94%BB%E9%9D%A2%E9%81%B7%E7%A7%BB%E5%9B%B3?node-id=0-1&t=5DstWTUJ0hwCyyRt-1

## ER図
```mermaid
erDiagram

  USERS {
    integer id PK
    string name
    string email
    string crypted_password
    string avatar
    integer role
    string salt
    datetime created_at
    datetime updated_at
    string reset_password_token
    datetime reset_password_token_expires_at
    datetime reset_password_email_sent_at
    integer access_count_to_reset_password_page
  }

  BAGELSHOPS {
    integer id PK
    string name "店名"
    string address "住所"
    float latitude "緯度"
    float longitude "経度"
    string place_id "Google Places APIの一意識別子"
    string opening_hours "開店時間"
    string photo_reference "写真"
    integer rating "評価"
    integer user_ratings_total "レビュー総数"
    string website "WEBサイト"
    string formatted_phone_number "電話番号"
    datetime last_updated_at "最終更新日"
    datetime created_at
    datetime updated_at
  }

  REVIEWS {
    integer id PK
    integer user_id FK
    integer bagel_shop_id FK
    text comment
    datetime created_at
    datetime updated_at
  }

  TAGS {
    integer id PK
    string name
    datetime created_at
    datetime updated_at
  }

  REVIEWTAGS {
    integer id PK
    integer review_id FK
    integer tag_id FK
    datetime created_at
    datetime updated_at
  }

  FAVORITES {
    integer id PK
    integer user_id FK
    integer bagel_shop_id FK
    datetime created_at
    datetime updated_at
  }

  USERS ||--o{ REVIEWS : "has many"
  BAGELSHOPS ||--o{ REVIEWS : "has many"
  USERS ||--o{ FAVORITES : "has many"
  BAGELSHOPS ||--o{ FAVORITES : "has many"
  REVIEWS ||--o{ REVIEWTAGS : "has many"
  TAGS ||--o{ REVIEWTAGS : "has many"
```
